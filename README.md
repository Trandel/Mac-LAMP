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

Every folder you create inside can be accessed through `.build` domain.
The apache config supports two levels of folder that can be accessed.

Examples:

`my-new-site.build => /User/USERNAME/Sites/my-new-site/`
`site.client1.build => /User/USERNAME/Sites/client1/site/`
`other-site.client2.build => /User/USERNAME/Sites/client2/other-site/`

Note: To access phpMyAdmin append `/phpmyadmin` at the end of the url.

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

Download and execute the script (**As your normal User**):

```sh
curl --remote-name https://raw.githubusercontent.com/trandel/mac-lamp/master/mac && sh mac 2>&1 | tee ~/laptop.log
```

Note: You will be asked to type in your password to use `sudo` command.


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

### Software

* [Git] for managing git repositories
* [Homebrew] for managing operating system libraries
* [ImageMagick] for cropping and resizing images
* [Node.js] and [NPM], for running apps and installing JavaScript packages
* [Mysql] for storing relational data
* [Ruby] stable for writing general-purpose code
* [Dnsmasq] to configure .localhost domain
* [Phpmyadmin] for managing mysql databases
* [Grunt] to run automated tasks
* [Sass] to extend CSS for your projects
* [Compass] to add additional funcions and mixins to [Sass]
* [Less] as an alternative to [Sass]
* [Brew Cask] to install tools and link them to your Applications folder

[Git]: https://git-scm.com/
[Homebrew]: http://brew.sh/
[ImageMagick]: http://www.imagemagick.org/
[Node.js]: http://nodejs.org/
[Mysql]: https://www.mysql.com/
[Ruby]: https://www.ruby-lang.org/en/
[Dnsmasq]: http://www.thekelleys.org.uk/dnsmasq/doc.html
[Phpmyadmin]: http://www.phpmyadmin.net
[Grunt]: http://gruntjs.com/
[Sass]: http://sass-lang.com/
[Compass]: http://compass-style.org/
[Less]: http://lesscss.org/
[Brew Cask]: http://caskroom.io/

### Tools (using brew cask)

* [LaunchRocket] - `launchd` management
* [Google Chrome] - browser
* [Firefox] - browser
* [Source Tree] - GIT client
* [FileZilla] - FTP client
* [Sublime Text] - Text Editor
* [Sequel Pro] - DB management

[LaunchRocket]: https://github.com/jimbojsb/launchrocket
[Google Chrome]: https://www.google.com/chrome/browser/desktop/index.html
[Firefox]: https://www.mozilla.org/en-US/firefox/desktop/
[Source Tree]: https://www.sourcetreeapp.com/
[FileZilla]: https://filezilla-project.org/
[Sublime Text]: http://www.sublimetext.com/
[Sequel Pro]: http://www.sequelpro.com/

It should take less than 30 minutes to install (depends on your machine and connection speed).

What files it will create and change
------------------------------------

#### Changes to `/private/etc/apache2/httpd.conf`
* Uncomment `#LoadModule php5_module`
* Uncomment `#LoadModule vhost_alias_module`
* New line `Include /private/etc/apache2/users/*.conf`

#### Changes to `/private/etc/apache2/users/USERNAME.conf`
Contents of this file will be replaced by following config:
```ApacheConf
<Directory "/Users/USERNAME/Sites">
  Options Indexes MultiViews FollowSymLinks
  AllowOverride All
  <IfModule mod_authz_core.c>
    Require all granted
  </IfModule>
  <IfModule !mod_authz_core.c>
    Order allow,deny
    Allow from all
  </IfModule>
</Directory>

<Virtualhost *:80>
  VirtualDocumentRoot "/Users/USERNAME/Sites/%1"
  ServerName sites.localhost
  ServerAlias *.localhost
  UseCanonicalName Off
</Virtualhost>

Alias /phpmyadmin /usr/local/share/phpmyadmin
<Directory /usr/local/share/phpmyadmin/>
  Options Indexes FollowSymLinks MultiViews
  AllowOverride All
  <IfModule mod_authz_core.c>
    Require all granted
  </IfModule>
  <IfModule !mod_authz_core.c>
    Order allow,deny
    Allow from all
  </IfModule>
</Directory>
```

#### Changes to `$(brew --prefix)/share/phpmyadmin/config.inc.php`
* `['AllowNoPassword']` will be set to `true`
* `['host']` will be set to `127.0.0.1`

#### New files for .localhost domain set up
* `$(brew --prefix)/etc/dnsmasq.conf` will contain `address=/.localhost/127.0.0.1`
* `$(brew --prefix)/etc/resolver/localhost` will contain `nameserver 127.0.0.1`

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

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