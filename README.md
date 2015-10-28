Laptop
======

Laptop is a script to set up an OS X laptop for web development (LAMP).

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.
Unfortunately we need some sudo commands for apache config, so the script might ask for your password to use sudo.

This is a modified version of https://github.com/thoughtbot/laptop/

Thanks guys for all the hard work and inspiration for me to create my own version.

Usage
-----

The script will aim to create an development environment inside this folder:
`/User/USERNAME/Sites/`.

Every folder you create inside can be accessed through `.localhost` domain.

Example:

`my-new-site.localhost => /User/USERNAME/Sites/my-new-site/`

Requirements
------------

We support:

* OS X Mavericks (10.9)
* OS X Yosemite (10.10)
* OS X El Capitan (10.11)

Older versions may work but aren't regularly tested. Bug reports for older
versions are welcome.

Install
-------

Download and execute the script:

```sh
curl --remote-name https://raw.githubusercontent.com/trandel/mac-lamp/master/mac && sh mac 2>&1 | tee ~/laptop.log
```

Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/trandel/mac-lamp/issues/new) for us.
Or, attach the whole log file as an attachment.

OS X El Capitan (10.11)
-----------------------

You may have problems installing Homebrew for the first time on OS X El
Capitan due to permission changes to the /usr directory (within which the Homebrew
installation is typically located). See the [Homebrew El Capitan troubleshooting instructions](https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/El_Capitan_and_Homebrew.md)
for steps to resolve the permissions issues that interfere with Homebrew's
installation.

What it sets up
---------------

* [Bundler] for managing Ruby libraries
* [Exuberant Ctags] for indexing files for vim tab completion
* [Foreman] for managing web processes
* [hub] for interacting with the GitHub API
* [Heroku Toolbelt] for interacting with the Heroku API
* [Homebrew] for managing operating system libraries
* [ImageMagick] for cropping and resizing images
* [Node.js] and [NPM], for running apps and installing JavaScript packages
* [Postgres] for storing relational data
* [Qt] for headless JavaScript testing via Capybara Webkit
* [Rbenv] for managing versions of Ruby
* [RCM] for managing company and personal dotfiles
* [Redis] for storing key-value data
* [Ruby Build] for installing Rubies
* [Ruby] stable for writing general-purpose code
* [The Silver Searcher] for finding things in files
* [Tmux] for saving project state and switching between projects
* [Zsh] as your shell

[Bundler]: http://bundler.io/
[Exuberant Ctags]: http://ctags.sourceforge.net/
[Foreman]: https://github.com/ddollar/foreman
[hub]: http://hub.github.com/
[Heroku Toolbelt]: https://toolbelt.heroku.com/
[Homebrew]: http://brew.sh/
[ImageMagick]: http://www.imagemagick.org/
[Node.js]: http://nodejs.org/
[NPM]: https://www.npmjs.org/
[Postgres]: http://www.postgresql.org/
[Qt]: http://qt-project.org/
[Rbenv]: https://github.com/sstephenson/rbenv
[RCM]: https://github.com/thoughtbot/rcm
[Redis]: http://redis.io/
[Ruby Build]: https://github.com/sstephenson/ruby-build
[Ruby]: https://www.ruby-lang.org/en/
[The Silver Searcher]: https://github.com/ggreer/the_silver_searcher
[Tmux]: http://tmux.sourceforge.net/
[Zsh]: http://www.zsh.org/

It should take less than 15 minutes to install (depends on your machine).

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew_tap 'caskroom/cask'
brew_install_or_upgrade 'brew-cask'

brew cask install dropbox
brew cask install google-chrome
brew cask install rdio

gem_install_or_update 'parity'

brew_install_or_upgrade 'tree'
brew_install_or_upgrade 'watch'
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo`,
`brew_install_or_upgrade`, and
`gem_install_or_update`
can be used in your `~/.laptop.local`.


Contributing
------------

Edit the `mac` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck] and [Syntastic].

```sh
brew install shellcheck
```

[ShellCheck]: http://www.shellcheck.net/about.html
[Syntastic]: https://github.com/scrooloose/syntastic

Thank you, [contributors]!

[contributors]: https://github.com/trandel/mac-lamp/graphs/contributors

License
-------

Laptop is © 2015 TNT-IT ltd.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE