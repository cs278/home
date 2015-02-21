Chris' Home
===========

My home directory is managed using a combination of git, [mr](http://joeyh.name/code/mr/)
and [vcsh](https://github.com/RichiH/vcsh). vcsh manages checking out multiple git repositories
into `$HOME` without making `$HOME` a git checkout itself, [rtfm](https://github.com/RichiH/vcsh#readme)
for more informations.

Repositories
------------

As a consequence of vcsh my home directory is assembled from a multiple git repositories, some private and
others public.

* [dotfiles](https://github.com/cs278/dotfiles) — Stores configuration for simple tools
* [home](https://github.com/cs278/home) — Contains the necessary information to bootstrap vcsh
* [bin](https://github.com/cs278/bin) — Little scripts and tools, my `~/bin` directory basically
* [etc-desktop](https://github.com/cs278/etc-desktop) — Configuration for graphical tools
* [etc-php](https://github.com/cs278/etc-php) — Configuration for PHP and related things
* [etc-sublime](https://github.com/cs278/etc-sublime) — Configuration for [Sublime Text](http://www.sublimetext.com/) editor

Items of Interest
-----------------

A select few highlights if you like, things that others might find useful.

* [gitconfig](https://github.com/cs278/dotfiles/blob/vcsh/.gitconfig) — Git aliases, shortcuts, and settings I've
  collected over the past few years.
* [network-location](https://github.com/cs278/bin/blob/master/bin/network-location) — Script which identifies
  which network the machine I'm using is on, this allows things like bespoke SSH configuration
  depending if I'm inside or outside a firewall.
* [ssh](https://github.com/cs278/dotfiles/blob/vcsh/.ssh) — Location aware SSH configuration, compiled by a vcsh hook.

Bootstrapping
-------------

Configuring my home directory on a new host.

```bash
wget -qO- "https://github.com/cs278/bin/raw/master/bin/ssh-mkkey" | bash
PATH="$HOME/bin:$PATH" bash -c "$(wget -q https://github.com/cs278/home/raw/master/bin/vcsh -O-)" -- clone git://github.com/cs278/home home
PATH="$HOME/bin:$PATH" mr update
```
