# System Build

Working on a process to automate standing up a new Linux system after installation.

Currently using LinuxMint as the base OS. Ansible is the primary though on the automation process. Might look at something like Makefile/Taskfile a well.

## Base Setup

From the base install, the software sources need to be configured. This ensures packages are downloaded from the location with the best bandwidth to the geographic location. There are a few ways this can be done.

1. Main Menu
    * At the top of the main menu is a search bar. Start typing `sources` and the Software Sources item will appear, probably before even typing the word.
2. Software Manager
    1. For a base Mint installation, this can be found in the menu along the left side. Usually an icon or two below the top item.
    2. Once inside the Software Manager, click te 3 line menu item in the top right of the window (sometimes called a hamburger icon). Select Software Sources from the menu.

Regardless of how it's opened, you'll need to enter your account password since it's going to be changing system configurations. Once the sources window is available, on the right are two entries under Mirrors, Main and Base. These items might have a word in parenthesis after them which is the codeword for the version of Mint and Ubuntu being ran. The Main item is for Mint, while the Base is Ubuntu.

To the right of the Mirror names are selection boxes. Start with whichever is preferrable. Click on it, and when the new screen appears, give it a few moments. The system will begin running some ping tests to the various mirrors available for that item, showing the average throughput to the right of each mirror.

Once some mirrors are present with decent speeds compared to your expected ISP speeds, select the one you want. I try to go with .edu based domains since those are education facilities. I feel like they are more likely to have individuals actively monitoring and ensuring the mirror stays active and up to date.

After selecting the best mirrors, it's time to update the apt cache. If the sources were accessed through Software Manager, then closing the sources window will cause the manager to auto-update with the data from the new mirrors. Otherwise, open a command prompt and do a `sudo apt update` to get the cache fully setup.

## Apt/flatpak Software to Install

Now that the apt mirrors are setup properly, it's time to get some items from apt that will be of assistance going forward. There are two primary tools needed from apt at the very beginning of a fresh system setup.

1. Vim
    * While Mint (like Ubuntu) has Nano istalled by default, I much prefer to use vim for text editing. So that's the first step. Install Vim and configure it as the default command line editor.
    * `sudo apt install vim`
    * It's not enough to just install vim though. The symbolic link to `usr/bin/editor` needs to be updated to use vim. For this, the update-alternatives tool is used.
    * `sudo update-alternatives --config editor` then choose the number associated with vim.basic.
2. Git
    * Git is a necessity for managing personal repositories. It's also needed to clone the repositories of tools needing to be installed later. Once installed, there are a few items that need to be configured. The configuration will be done with `git config` once it's installed.
    * `sudo apt install git`
    * `git config --global init.defaultBranch main`
    * `git config --global user.name "My Name"`
    * `git config --global user.email "me@me.com"`
    * Name and email should be update as desired. GitHub provides a `users.noreply.github.com` domain based email for registered users who wish to obfuscate their personal address. Check account settings on [github.com](https://github.com) for the value.
3. KeePassXC
    * An awesome password manager tool. This is in the flatpak repository. The homepage for the tool also has alternate install methods if desired.
    * `flatpak install org.keepassxc.KeePassXC`
    * While this command has worked on the last couple of version of Mint I've installed, the homepage has instructions for adding the flathub repository and installation with the `--user` flag.
4. rclone
    * rclone is rsync for commercial cloud storage providers. It can integrate with Google Drive, AWS, Dropbox, and more. I use it for backing up data to the cloud.

## External Software to Install

Below are the base list of external software that I install into a fresh built Linux system.

* [Chrome](#chrome)
* [Zed](#zed-editor)
* [AppImageLauncher](#appimagelauncher)
* [Obsidian](#obsidian)
* [neovim](#neovim)
* [Docker](#docker)
* [DevPod](#devpod)

### [Chrome](https://www.google.com/chrome/)

I've been a fan of Google for a long time. While I cut my web browsing teeth in the infancy of the Internet with NCSA Mosaic and had my fun during the Netscape vs Internet Explorer browser wars, I've had Chrome installed on every system I've used that could support it since it was available publicly. In fact, the first thing I do with the pre-installed browser on any fresh build is heading to Google's Chrome page and download the latest version.

As part of the automation piece of this project, I will be looking to figure out how to find and download the current stable version during the buildout.

### [Zed Editor](https://zed.dev/download)

The Zed editor is a great opensource alternative to VSCode. From my understanding, it's got most of the stuff VSCode offers, but in a much friendlier license and with really good default configuration options. I honestly can't remember how I came across it, but after my first installation, I never looked back. The installation is really simple, a one liner using curl piped into sh with no need for sudo or root privelages. The final step of the installation includes the commands needed to add the installed binary directory to the path. On Mint, this isn't needed, as .profile already has a call to add that specific directory to the path variable if it exists.

### [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher)

This is an awesome tool. It runs a daemon that monitors for runs of AppImage files. It will configure the system with the appropriate .desktop file so the AppImage program can be found in the menu. This also ensures that the proper icon for the program is available to be seen on the taskbar and in the menu. This will also move the AppImage files into a single location in the users HOME directories. Finally, the launcher helps update checks for the AppImages.

### [Obsidian](https://obsidian.md/)

This is a really good knowledge management tool. It uses a combination between markdown and wiki formatting to display the data. Similar to a wiki allows for interconected notes and files.

### [Neovim](https://github.com/neovim/neovim)

As mentioned previously, vim is the preferred editor for me. Neovim started as a fork of vim, which had been getting stale with regards to releases and improvements. Neovim update the code base while implementing a variety of requested features for vim. Since the initial fork/release of Neovim, vim has made some forward movements, even implementing some of the Neovim improvements. I've only just recently started looking at Neovim. I've have yet to decide on a GUI to use with Neovim. The only one I've tried so far had to be compiled from source and the stable release branch still ended up crashing on me.

### [Docker](https://docs.docker.com/engine/install/ubuntu/)

Docker is a great containerization tool. I use it for a few tools in my homelab. I've also started looking at it as a remote development envionment. I'm not so much after the ephemeral aspect of the remote development. I do like the idea of using a container to install the development toolchains needed for any project. This keeps my host system free of any potential dependency issues.

### [Devpod](https://devpod.sh/docs/getting-started/install)

Devpod is an opensource alternative to the devcontainer feature of VSCode. It even uses the devcontainer file yaml format standard for environment defiitions. It allows using multiple types of containers for remote development. It also integrates with multiple editors, include Zed. This works in concert with docker as part of my development environment.
