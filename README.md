# syncing

A small shell utility to sync the current working directory to a remote host using `rsync` over SSH.

## Requirements

- `sh`
- `rsync`
- `ssh`
- an SSH key that can access the remote host

## What it does

- uses the current directory as the source by default
- syncs to `fabio@home.fabio.eti.br:/home/fabio/toshiba/jellyfin/Medias` by default
- uses SSH port `40822` by default
- uses `~/.ssh/ubuntu` as the default SSH key
- preserves metadata, shows progress, and deletes removed files on the destination
- can load configuration from `.syncing.env` or `.env`

The source defaults to `$(pwd)/`, with a trailing slash. In `rsync`, that means it syncs the contents of the current directory into the remote destination.

## Installation

1. Create the `~/.bin` directory if it does not exist:

```sh
mkdir -p "$HOME/.bin"
```

2. Make sure the script is executable by you only:

```sh
chmod 700 ~/.bin/syncing
```

3. Add `~/.bin` to your `PATH` in `~/.bashrc` or `~/.zshrc`:

```sh
export PATH="$HOME/.bin:$PATH"
```

4. Reload your shell:

```sh
source ~/.bashrc
```

or for Zsh:

```sh
source ~/.zshrc
```

5. Run the command:

```sh
syncing
```

## Quick Use

From the folder you want to sync:

```sh
syncing
```

Preview what would happen without transferring files:

```sh
syncing --dry-run
```

Sync to another destination:

```sh
syncing --to 'user@host:/remote/path'
```

Sync another local folder:

```sh
syncing --from '/home/fabio/Videos/'
```

## Configuration

You can override the destination, port, key, and source without editing the script.

Configuration files are loaded automatically from the current directory, in this order:

1. `.env`
2. `.syncing.env`

If `SYNCING_ENV_FILE` is set, only that custom file is loaded.

Example `.syncing.env`:

```sh
SYNCING_TO='user@host:/remote/path'
SYNCING_SSH_PORT=40822
SYNCING_SSH_KEY='~/.ssh/ubuntu'
FROM='/home/fabio/Videos/'
```

Environment variables:

- `TO` or `SYNCING_TO` — remote target host and path
- `SSH_PORT` or `SYNCING_SSH_PORT` — SSH port
- `SSH_KEY` or `SYNCING_SSH_KEY` — SSH private key
- `FROM` — source directory (defaults to current working directory)
- `SYNCING_ENV_FILE` — custom config file to load instead of `.syncing.env`/`.env`

Configuration priority:

1. command-line options
2. `SYNCING_ENV_FILE`, when set
3. `.syncing.env`
4. `.env`
5. shell environment variables
6. script defaults

Command-line options:

```sh
syncing --dry-run --to 'user@host:/remote/path' --port 40822 --key '~/.ssh/ubuntu'
```

All options:

```text
--dry-run      show what would be transferred without sending files
--to DEST      remote destination, e.g. user@host:/path
--from DIR     source directory
--port PORT    SSH port
--key PATH     SSH private key
--help         show help
```

## Examples

Use a project-local config file by creating `.syncing.env` in the folder you run `syncing` from:

```sh
SYNCING_TO='fabio@home.fabio.eti.br:/home/fabio/toshiba/jellyfin/Medias'
SYNCING_SSH_PORT=40822
SYNCING_SSH_KEY='~/.ssh/ubuntu'
```

Then run:

```sh
syncing --dry-run
syncing
```

Use a one-off custom config file:

```sh
SYNCING_ENV_FILE="$HOME/.config/syncing/media.env" syncing
```

## Notes

- Keep your SSH key path correct: `~/.ssh/ubuntu`
- This script is meant to run from the directory you want to sync.
- If you want to use a different source folder, just `cd` into that folder and run `syncing`.
- The command uses `--delete`, so files removed locally are also removed from the destination.
- Run `syncing --dry-run` first when changing destinations or source folders.
- Config files are sourced as shell scripts, so only use files you trust.
