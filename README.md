<img src="https://user-images.githubusercontent.com/1292576/190241835-41469235-f65d-4d4b-9760-372cdff7a70f.png" width="48">

# Dustin's Nix / NixOS config
![GitHub last commit](https://img.shields.io/github/last-commit/dustinlyons/nixos-config?style=plastic)

# Overview
> "All we have to decide is what to do with the time that is given us." - J.R.R. Tolkien

Hey, thanks for showing up! 🤓

You've stumbled upon my personal journey with Nix. For over a year, I've been hacking away on this configuration. It drives my office PC, Macbook, and virtual machines in my home lab. Along with syncthing to manage data, this Nix configuration guarantees I have a working, seamless experience across each machine I use. 

Immutable, reproducible infrastructure rocks! It's game changing and I'll never go back to typing commands in a terminal.

While developing, I've done my best to keep it simple - for both future me and readers like you. You'll see that in how I've organized code, for example, as I keep filename conventions the same across modules. To get you started, I've included step-by-step instructions on bootstrapping a new machine below.

Feel free to open a Github Issue if you run into any problems or have questions. Enjoy Nix!

# Features
* Multiple Nix and NixOS configurations, including desktop, laptop, server
* Support for MacOS, even managing Mac App Store apps!
* Fully managed, auto-updating [homebrew](https://github.com/dustinlyons/nixos-config/blob/main/macos/home-manager.nix#L51) environment
* Optimized for simplicity and readability in all cases 
* Minimal shell scripts for basic functions of running systems
* Easily [share](https://github.com/dustinlyons/nixos-config/tree/main/common) config across Linux and Mac, both Nix and Home Manager
* Extensively configured NixOS environment with clean aesthetic + window animations
* Large Emacs [literate configuration](https://github.com/dustinlyons/nixos-config/blob/main/common/config/emacs/Emacs.org) to explore
* Auto-loading of Nix [overlays](https://github.com/dustinlyons/nixos-config/tree/main/overlays): drop a file in a dir and it runs (great for patches!)
* Step-by-step instructions to start from zero, both x86 and MacOS platforms
* Defined using a single flake and two targets, not small files spread across collections of modules

## Coming Soon
* Opt-in persistence using [impermanence](https://github.com/nix-community/impermanence) and snapshot reset with `btrfs`
* Persistence defined under [XDG](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html)
* Secrets managed with `sops-nix`

# Layout

```
.
├── bin          # Simple scripts used to wrap the build
├── common       # Shared configurations applicable to all machines
├── hardware     # Hardware-specific configuration
├── macos        # MacOS and nix-darwin configuration
├── nixos        # My NixOS desktop-related configuration
├── overlays     # Drop an overlay file in this dir, and it runs. So far mainly patches.
└── vms          # VM-specific configs running in my home-lab
```

# Bootstrap New Computer

## Step 1 - For MacOS, install Nix package manager
Install the nix package manager, add unstable channel:
```sh
sh <(curl -L https://nixos.org/nix/install) --daemon
```
```sh
nix-channel --add https://nixos.org/channels/nixpkgs-unstable nixpkgs
```
```sh
nix-channel --update
```

## Step 2 - For NixOS, create a disk partition and install media
Follow this [step-by-step guide](https://github.com/dustinlyons/nixos-config/blob/main/vm/README.md) for instructions to install using `ZFS` or `ext3`.


## Step 3 - Install home-manager
Add the home-manager channel and install it:
```sh
nix-channel --add https://github.com/nix-community/home-manager/archive/master.tar.gz home-manager
```
```sh
nix-channel --update
```

## Step 4 - If MacOS, install Darwin dependencies
Install Xcode CLI tools and nix-darwin:
```sh
xcode-select --install
```
```sh
nix-build https://github.com/LnL7/nix-darwin/archive/master.tar.gz -A installer
```
```sh
./result/bin/darwin-installer
```

## Step 5 - Build the environment
Download this repo and run:
```sh
./bin/build
```

## Step 6 - Add Yubikey and generate key
Insert [Yubikey](https://www.yubico.com/) and generate private keys
```sh
ssh-keygen -t ecdsa-sk
```

## Step 7 - Reboot computer
That's it. You're done.

# Update Computer

## Download the latest updates and update lock file
```sh
nix flake update
```
## Run  build
```sh
./bin/build
```

## You made it this far
Add me on [Twitter](https://twitter.com/dustinhlyons).

_Thank you Nix and the [communities](https://github.com/nix-community/emacs-overlay) around [nixpkgs](https://github.com/NixOS/nixpkgs)._
