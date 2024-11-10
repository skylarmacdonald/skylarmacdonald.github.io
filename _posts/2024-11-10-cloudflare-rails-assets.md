---
title: Cloudflare Pages as an asset host for a Rails app
layout: post
tags:
  - til
  - tech
  - rails
image: cloudflare_pages.png
image_alt: The Cloudflare Pages logo.
description: You can store your Rails app's compiled assets on Cloudflare Pages. Should you? I have no idea.
toot: # to follow
---

Today I learned: you can 'build' a Rails app in Cloudflare Pages to use it as an asset host.

Start by creating a new Cloudflare Pages site and point it at your Rails app's SCM repository. (You could do this the long way around with Direct Uploads, but who wants to bother with that?) Set your build command to `rails assets:precompile`, and tell it that your output directory is `public`. (This will deploy _everything_ in your `public/` folder to Cloudflare, but this is basically fine. It makes sure the right path is used for your assets.)

The first build will fail, because Cloudflare won't be able to find your database library on its runner.[^1] But that's fine --- you can use the [NullDB adapter](https://github.com/nulldb/nulldb) to build your site without needing a real database.[^2]

Add it to your Gemfile like so:
```ruby
gem 'activerecord-nulldb-adapter', group: :production
```

I added it to my `:production` group because I only want to use this when I'm compiling my assets on Cloudflare. You could even create a group just for assets and do it that way; I thought that was overkill.

You will also need to set a `DATABASE_URL` environment variable in Cloudflare Pages to tell Rails that you want to use your null database. You can use this:

```
nulldb://user:pass@127.0.0.1/dbname
```

So long as it's a valid database URL and references the `nulldb://` adapter, you'll be fine. Have fun. Get creative with it.

You might want to not build any gems that you don't need to precompile your assets. You can do this by setting the `BUNDLE_WITHOUT` environment variable to a space-separated list of environments you want to ignore, for example, `development test`.

If you use [Rails credentials](https://edgeguides.rubyonrails.org/security.html#custom-credentials), you'll need to provide your master key, or make sure your application doesn't try and load any of them. This is harder than it sounds, because Rails initialises your entire application just to run `rails assets:precompile`. This is a little silly, so watch out for that. If you're not using credentials, this should be fine.

You will, however, need a `SECRET_KEY_BASE` --- but you can fudge this easily. Set the `SECRET_KEY_BASE_DUMMY` environment variable to `1`. (Why there isn't a `RAILS_MASTER_KEY_DUMMY` is beyond me.)

That should be the lot --- push your changes to your SCM of choice and Cloudflare should build the assets as if by magic.

You probably want to create a custom domain for your Pages 'site', so head over to that part of the settings and set that up. A subdomain on your app domain is a grand idea. You can, of course, still use the `pages.dev` subdomain you get for free --- but you don't want everyone to know your nifty trick that I'm openly blogging about!

The last step is to uncomment (or add) the `asset_host` setting in your `config/environments/production.rb` file.

```ruby
config.asset_host = 'https://assets.example.com'
```

That will tell Rails to write this domain into all your asset URLs, which will cause them to be served by your shiny new Cloudflare Pages thingy-whatsit. Because you set your output directory to `public/`, you shouldn't have to add any other configuration.

## Should you do this?

I have no idea. Probably not. If you are putting the rest of the app behind Cloudflare anyway, the performance gains are probably marginal at best. If you aren't doing that, maybe it will make it slightly faster. If you're using some kind of shiny PaaS thing, it does mean you won't have to serve your static assets from your application server --- but I'm loathe to call that a 'benefit', because there's absolutely nothing wrong with doing that.

So, this probably doesn't win you anything if we're being realistic. But it's an interesting experiment, and I learned some useful stuff doing it.


<small>Image: Cloudflare</small>


[^1]: This is definitely true with Postgres. I haven't tried it with SQLite or anything like that.
[^2]: This doesn't currently work out of the box with Rails >= 7.2, because of a change in how you have to register ActiveRecord adapters; there are open PRs that fix this, so you could fiddle your Gemfile to point to those instead.
