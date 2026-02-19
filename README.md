# 💫 AURA - Arch User Repository Automation

A way to automate updates of binaries published to AUR.

This repository provides packages of CLI tools developed and/or maintained by me.

Fork and customize to your needs.

## Packages

A list of the provided packages:

|Name|GitHub|AUR|
|---|---|---|
|`poof`|[GitHub](https://github.com/pirafrank/poof)|[AUR](https://aur.archlinux.org/packages/poof-bin)|
|`vault-conductor`|[GitHub](https://github.com/pirafrank/vault-conductor)|[AUR](https://aur.archlinux.org/packages/vault-conductor-bin)|

## How it works

Each package is maintained as a git submodule pointing to its AUR repository.

A Python update script reads a YAML configuration file, fetches the latest stable release information from GitHub releases, and renders a `PKGBUILD` file from Jinja template. Then, it generates the corresponding `.SRCINFO` file.

A GitHub Action is used to automate the update process, ensuring that package definitions remain in sync with the latest releases from GitHub. In the action, `vault-conductor` itself is used as SSH-Agent to load both:

- the SSH Key to sign the commit with updated `PKGBUILD` and `.SRCINFO`,
- and the SSH Key dedicated to push those changes to AUR.

## Setup environment

### 1. Install Requirements

On Arch Linux:

```sh
pacman -Sy
pacman -S namcap
```

On Debian-based:

```sh
sudo apt-get update
sudo apt-get install -y pacman-package-manager
```

### 2. Setup virtualenv

```sh
cd scripts
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

## Generate

```sh
cd scripts
source .venv/bin/activate
# generate PKGBUILD
python3 update.py <configuration filename>
# note: 'makepkg --printsrcinfo > .SRCINFO' is run by the script itself!
# test (Arch Linux-only)
cd ../packages/[submodule]
namcap PKGBUILD
```

## Fork it

Fork and use for your own tools!

**Requirements:**

1. You have manually created your tool repo on AUR (because [that is how AUR works](https://wiki.archlinux.org/title/AUR_submission_guidelines)),
2. You have cleaned the `configurations/` directory and removed any existing submodules from `packages/`.

**Add a repository:**

It is as easy as:

1. Add the required repository as submodule in `packages/`
2. Write configuration in `configurations/`
3. Create a Jinja template for it in `templates/`
4. Commit and push changes!

## Brother project

- [🍺 pirafrank's Homebrew Tap](https://github.com/pirafrank/homebrew-tap), Formulae for my CLI tools

## License

MIT
