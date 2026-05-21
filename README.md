# syncing

A small shell utility to sync the current working directory to a remote host using `rsync` over SSH.

## What it does

- uses the current directory as the source by default
- syncs to a default remote destination that can be overridden
- uses SSH port `40822` by default
- preserves metadata, shows progress, and deletes removed files on the destination
- can load configuration from `.syncing.env` or `.env`

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

## Configuration

You can override the destination, port, key, and source without editing the script.

Configuration files are loaded automatically from the current directory, in this order:

1. `.env`
2. `.syncing.env`

Example `.syncing.env`:

```sh
SYNCING_TO='user@host:/remote/path'
SYNCING_SSH_PORT=40822
SYNCING_SSH_KEY='~/.ssh/ubuntu'
```

Environment variables:

- `TO` or `SYNCING_TO` — remote target host and path
- `SSH_PORT` or `SYNCING_SSH_PORT` — SSH port
- `SSH_KEY` or `SYNCING_SSH_KEY` — SSH private key
- `FROM` — source directory (defaults to current working directory)
- `SYNCING_ENV_FILE` — custom config file to load instead of `.syncing.env`/`.env`

Command-line options:

```sh
syncing --dry-run --to 'user@host:/remote/path' --port 40822 --key '~/.ssh/ubuntu'
```

## Notes

- Keep your SSH key path correct: `~/.ssh/ubuntu`
- This script is meant to run from the directory you want to sync.
- If you want to use a different source folder, just `cd` into that folder and run `syncing`.
- Config files are sourced as shell scripts, so only use files you trust.
