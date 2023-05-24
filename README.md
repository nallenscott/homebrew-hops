<h1 align="center">
  <img src="tmpl/hops.png" width=240 alt=""><br>
  homebrew-hops<br>
</h1>

> **Table of contents**
> - [Installation](#installation)
> - [Quick start](#quick-start)
> - [Dependencies](#dependencies)
> - [Authentication](#authentication)
> - [Environment](#environment)
>   - [Hopsfile](#hopsfile)
>   - [Brewfile](#brewfile)
> - [Commands](#commands)
>   - [`init`](#init)
>   - [`sync`](#sync)

Hops allows you to sync Homebrew across multiple machines by using a Gist backend to store the state of your taps, formulae, casks, and vscode extensions. Use it to keep your work and home machines in sync, or to quickly get a new machine up and running.

## Installation

Homebrew will pull the latest version of hops every time you run `brew update`. To install:

`brew tap nallenscott/hops`

## Quick start

1. To sync a machine with the backend, run `init` with a personal access token, and then `sync`.
```sh
# hostname: home_machine
$ brew tap nallenscott/hops
$ brew hops init --token ghp_...
$ brew hops sync
```

2. To sync Homebrew with another machine, run `sync` with the hostname of the target machine.
```sh
# hostname: work_machine
$ brew tap nallenscott/hops
$ brew hops init --token ghp_... --gist aad9...
$ brew hops sync --host home_machine
```

## Dependencies

Hops use [`gh`](https://cli.github.com/manual/) to interact with the GitHub API. Hops will check for it and prompt you to install it if it's missing.

## Authentication

Hops uses a personal access token to authenticate with GitHub. You can create a new token [here](https://github.com/settings/tokens). The minimum required scopes are: `repo`, `read:org`, `gist`. The `repo` and `read:org` scopes are required by `gh`.

## Environment

### Hopsfile

The first time `init` is run, hops will create a new Gist named `.hops` and store the Gist ID in `~/.hops/Hopsfile`. The Gist URL will be printed to the console after `init` completes.

> `~/.hops/backend.env`
```env
GITHUB_TOKEN="ghp_5bbd682cfbbe427297e1360fa9fd02f2einA"
GIST_ID="aad9e4b592a34fefb29515475244edfe"
GIST_URL="https://gist.github.com/foob/aad9e4b592a34fefb29515475244edfe"
```

### Brewfile

Running `sync` will update the Brewfile with the currently installed taps, formulae, casks, and vscode extensions. The local copy is stored in `~/.hops/Brewfile`. It's also uploaded to the Gist backend using the hostname of the machine as the filename.

> `~/.hops/Brewfile`
```txt
tap "aws/tap"
tap "cargo-lambda/cargo-lambda"
brew "sqlite"
brew "python@3.11"
cask "discord"
cask "docker"
vscode "golang.go"
vscode "hashicorp.hcl"
```

## Commands

### `init`

Creates a new Gist and stores the Gist ID and GitHub token in `~/hops/Hopsfile`.

```sh
$ hops init [options]
```

**Options**

| Name | Description |
| :--- | :--- |
| `--token` | A personal access token with gist permissions [1] |
| `--gist` | The ID of an existing Gist to use as the backend |
> [1] Requires the `repo`, `read:org`, and `gist` scopes.

### `sync`

Updates the Brewfile with the currently installed taps, formulae, casks, and vscode extensions.

```sh
$ hops sync [options]
```

**Options**

| Name | Description |
| :--- | :--- |
| `--host` | The hostname of another machine to sync with [1] |
> [1] The other machine must have the same Gist backend.
