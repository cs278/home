Chris' Home
===========

My home directory is managed using a combination of git, [mr](http://myrepos.branchable.com/)
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
* [etc-puppet](https://github.com/cs278/etc-puppet) — Machine configuration
* [etc-desktop](https://github.com/cs278/etc-desktop) — Configuration for graphical tools
* [etc-php](https://github.com/cs278/etc-php) — Configuration for PHP and related things
* [etc-atom](https://github.com/cs278/etc-atom) — Configuration for [Atom Text Editor](https://atom.io)
* [etc-sublime](https://github.com/cs278/etc-sublime) — Configuration for [Sublime Text](http://www.sublimetext.com/) editor

Items of Interest
-----------------

A select few highlights if you like, things that others might find useful.

* [gitconfig](https://github.com/cs278/dotfiles/blob/master/.gitconfig) — Git aliases, shortcuts, and settings I've
  collected over the past few years.
* [network-location](https://github.com/cs278/bin/blob/master/bin/network-location) — Script which identifies
  which network the machine I'm using is on, this allows things like bespoke SSH configuration
  depending if I'm inside or outside a firewall.
* [ssh](https://github.com/cs278/dotfiles/blob/master/.ssh) — Location aware SSH configuration, compiled by a vcsh hook.

Bootstrapping
-------------

Configuring my home directory on a new host.

```bash
export PATH="$HOME/bin:$PATH"
which sudo &>/dev/null || su -c "apt-get install -y sudo && adduser $USER sudo"
which git &>/dev/null || sudo apt-get install -y git
which mr &>/dev/null || sudo apt-get install -y myrepos
wget -qO- "https://github.com/cs278/bin/raw/master/bin/ssh-mkkey" | bash
# Ensure proper SSH agent is available
eval `ssh-agent`
ssh-add ~/.ssh/${USER:-$LOGNAME}@`hostname --fqdn`
ssh -T git@github.com
# Stub in init hooks to trigger for the initial clone, these delete themselves to avoid any conflicts before checking out
mkdir -p .config/vcsh/hooks-enabled && for hook in 00-sparse-checkout 10-pull-rebase; do wget -O .config/vcsh/hooks-enabled/post-init.$hook https://github.com/cs278/home/raw/master/.config/vcsh/hooks-available/post-init.$hook && chmod +x .config/vcsh/hooks-enabled/post-init.$hook && echo 'exec rm $0' >> .config/vcsh/hooks-enabled/post-init.$hook; done
bash -c "$(wget -q https://github.com/cs278/home/raw/master/bin/vcsh -O-)" -- clone git@github.com:cs278/home home
mr update
```
