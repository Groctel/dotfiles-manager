# dotfiles-manager (Dotfiler)

**A powerful POSIX shell dotfiles manager program**

You can see it in action in [my personal dotfiles repository](https://github.com/Groctel/dotfiles).

<p align="center">
<a href="https://ko-fi.com/groctel"><img height="70px" src="https://raw.githubusercontent.com/Groctel/Groctel/main/images/BuyMeACoffee_blue-2x.png" alt="Buy me a coffee"></a>
</p>

## Installation

Run `./dotfiler.sh` or copy it to your `.local/bin` or other directory in your `$PATH`.
Feel free to remove the `.sh` extension.
You may need to `chmod +x q`.

```sh
mkdir -p ~/.local/bin && cp dotfiler.sh ~/.local/bin/dotfiler
```

## Usage

```
dotfiler.sh [-h] (-[dipu])+ [system-tags]
```

### Operational arguments

  - `-h, --help`**:** Print a complete help text and exit.
  - `-d, --deploy`**:** Deploys dotfiles to the system.
  - `-i, --install`**:** Installs packages and configures the system.
  - `-p, --pull`**:** Copies dotfiles into the repository.
  - `-u, --update`**:** Updates the installed packages list.
  - `-x, --extra`**:** Install dotfiles' extra dependencies.

### System tags

System tags are optional arguments that MUST NOT start with the dash `-` character.
They're used to specify a system to work with.
Example systems are `laptop`, `desktop`, `shared-pc`, `sister`, `Groctel`, etc.
If no system was specified, Dotfiler will look for list files (explained below) with no system suffixes.

### Filelist file:

Dotfiler needs a filelist file to keep track of the files in your system.
This file should be named `filelist` by default or `filelist-SYSTEM` for an specific system, e.g. `filelist-desktop`.
This file keeps track of your dotfiles using the following syntax:

- **Regular files:** The path to the file from `~/`.
- **Directories:** The path to the file from `~/` terminated with `/*`.

For example, here are a regular file and a directory:

```
.config/i3/config
.local/share/fonts/*
```

### Pkglist file:

Dotfiler keeps tracks of your installed packages in a pkglist file that follows the same naming conventions as the filelist file but does not need to be explicitly created by the user.
To keep it simple, it only tracks the packages explicitly installed by the user.

### Deplist file:

You can store some commands to be run by Dotfiler in a deplist file that follows the same naming conventions as the filelist and pkglist files.
Commands are parsed line by line and must not contain linefeeds.
For example, these are two valid lines containing commands:

```sh
curl -fLo $HOME/.antigen.zsh git.io/antigen
vim +`so $HOME/.vimrc` +PlugInstall +qa!
```

### Argument order:

Arguments are stored in a list of operations and a list of systems.
Dotfiler will run all operations in the order they were passed in all systems in the order they were passed.
For example, the following call:

```sh
dotfiler.sh -p -u common laptop
```

Will run the following tasks:

- `pull common`
- `pull laptop`
- `update common`
- `update laptop`

### CAVEATS:

This program only keeps track of your packages with `yay`.
Pull request the project to add support other managers.
