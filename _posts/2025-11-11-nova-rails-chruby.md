---
title: Getting Rails tasks in Nova to work with chruby
layout: post
tags:
  - til
  - tech
  - rails
image: nova.jpg
image_alt: The Nova logo and a screenshot of the app displaying a website project.
description: Non-interactive shells are surprisingly challenging to get chruby to work in
---

**TL;DR: Set up chruby in your `.zshenv` file instead of `.zshrc`.**

I recently bought [Nova](https://nova.app) by [Panic](https://panic.com). I absolutely love it. I was formerly a die-hard [Sublime Text](https://www.sublimetext.com) fan --- and I still am, really, for more 'general purpose' editing outside of a Project™ --- but before that, I used to use Coda (RIP) by Panic, and Nova feels like its spiritual successor.

Like most IDEs, Nova has [tasks](https://help.nova.app/tasks) to allow you to run build scripts and that sort of thing. Because of its [extensions framework](https://help.nova.app/extensions), you can get loads of tasks that work with different frameworks and languages. (I promise this is not a sponsored post.)

## Ruby, Rails, and controlling snakes with gems

Perhaps unfortunately for me given recent developments, I am a Rails programmer.[^1] But on the bright side, there's an excellent Nova extension for Rails that makes life a lot easier.

Until, that is, you go to run the dev server, and it does this:

```
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/bundler/lockfile_parser.rb:108:in `warn_for_outdated_bundler_version':
You must use Bundler 2 or greater with this lockfile. (Bundler::LockfileError)
```

Bother. It's using the built-in macOS Ruby there, isn't it?

See, the world of Ruby has moved on, but macOS's built-in utilities have not. Ruby is up to version 3.4.7 at time of writing, and as you can see above the version bundled with macOS is ossified at version 2.6.0. And, well, the big number at the front [means breaking changes](https://semver.org).

I manage my Rubies on my Mac with [chruby](https://github.com/postmodern/chruby). That is one of many ways to do this, and you can argue amongst yourselves[^2] what the best one is. chruby requires you to `source` a script in your `.profile` or `.bashrc` / `.zshrc` file, which sets up the `chruby()` function used to switch different versions of it into your PATH. But the problem is, Nova runs its tasks in a **non-interactive** shell. And before I bought Nova, I had no idea what one of those was.

Now, I use zsh as my shell on my Mac, which [you probably do too](https://support.apple.com/en-gb/102360) if you have updated past Catalina or care about this sort of thing. You probably have to do this sort of nonsense for Bash as well, and as dearly as I love Bash I have only done this in zsh. Just so you're aware.

## The thing I did not know

Very, _very_ simply:

* **Interactive** shells are the ones you can type in --- one of which is your **login** shell. You can run commands in these shells and do all your usual stuff.
* **Non-interactive** shells run scripts and you can't touch them. Everything you need to access needs to be in your shell config when it starts, or your script will fail.

The key difference: non-interactive shells **don't source your `.zshrc` or `.zprofile` configs** when they start. This is really tricky to get to the bottom of, because when you open the _terminal_ in Nova, it **is** interactive (because you can type in it).

Fortunately, those clever people who made zsh have thought of this, and you can create a `.zshenv` file in your home directory that _does_ get sourced in non-interactive shells. So, contrary to the instructions you're given when you install chruby, what you actually want to do is set up chruby in there.

So I stuck these lines in my newly-created `.zshenv` file:

```zsh
source /opt/homebrew/share/chruby/chruby.sh
source /opt/homebrew/share/chruby/auto.sh
```

And it _still_ didn't work!

## Another gotcha

I use chruby's `auto.sh` script to automatically select the correct version of Ruby based on a `.ruby-version` file in the project root --- but Nova _still_ wanted to run the Rails server with the built-in Ruby. How bizarre!

`auto.sh` works by adding a pre-exec hook to run itself, like this:[^3]
```zsh
if [[ -n "$ZSH_VERSION" ]]; then
  if [[ ! "$preexec_functions" == *chruby_auto* ]]; then
    preexec_functions+=("chruby_auto")
  fi
elif [[ -n "$BASH_VERSION" ]]; then
  trap '[[ "$BASH_COMMAND" != "$PROMPT_COMMAND" ]] && chruby_auto' DEBUG
fi
```
(The `chruby_auto()` function is defined further up the file.)

It transpires that pre-exec hook doesn't run in non-interactive shells. So you need to force `chruby_auto()` to run. Mercifully, this is as easy as adding it to the end of your `.zshenv`:
```zsh
source /opt/homebrew/share/chruby/chruby.sh
source /opt/homebrew/share/chruby/auto.sh
chruby_auto
```

And as if by magic:
![Screenshot of Nova successfully running the Rails Server task.]({% link assets/blog/nova_rails_server.png %}){:.blog_photo}
It works!

## Et voila

I mostly wrote this up so I can remember how to do it again if I ever need to. But hopefully that helps you if you find yourself in this situation. I imagine this also works if you're trying to use chruby-ed Rubies in other non-interactive environments, but this is the context in which I had to figure it out.

Merry chruby-ing!


[^1]: Although _he_ and I have vastly different recollections of London. Especially given that I actually live here!
[^2]: You may not argue _with me_ about this, because I don't care.
[^3]: [See the full source file here.](https://github.com/postmodern/chruby/blob/master/share/chruby/auto.sh)
