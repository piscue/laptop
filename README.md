# Laptop

![CI](https://github.com/piscue/laptop/actions/workflows/main.yml/badge.svg)

Laptop is a script to set up a macOS laptop for software development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages based on what is already installed.

## Prerequisites

- macOS 13 (Ventura) or later
- Xcode Command Line Tools (`xcode-select --install`)
- Administrator access (the script will prompt for your password)
- Internet connection

## Install

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/piscue/laptop/main/mac
```

Review the script (avoid running scripts you haven't read!):

```sh
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```

## What Gets Installed

The full package list is in [`Brewfile`](Brewfile). High-level categories:

- **CLI Utilities** — bat, fzf, jq, ripgrep, tmux, neovim, and more
- **Dev Tools** — git, go, node, python, kubectl, helm, terraform tools, docker
- **AI/ML** — ollama, claude-code-router, LM Studio
- **Browsers** — Arc, Chrome, Firefox, DuckDuckGo
- **Communication** — Slack, Discord, Telegram, WhatsApp, Zoom, Lark
- **Productivity** — 1Password, Notion, Raycast, iTerm2, Warp, Keyboard Maestro
- **Media & Creative** — OBS, Spotify, Audio Hijack, Descript, MacWhisper
- **VPN & Network** — Tailscale, NordVPN, CyberGhost
- **Mac App Store** — Amphetamine

## Customize in `~/.laptop.local`

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.

### Example `~/.laptop.local`

```sh
#!/bin/sh

# Install global npm packages
npm install -g typescript eslint

# Set up dotfiles
ln -sf ~/dotfiles/.gitconfig ~/.gitconfig

# Configure git
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

## Troubleshooting

### Mac App Store installs fail

You must be signed in to the Mac App Store in the App Store app before running the script.
If you're not signed in, MAS installs are skipped automatically.

### Link conflicts

The script uses `brew install --overwrite` for a few packages to resolve
pre-existing link conflicts. If you see warnings about overwriting files,
this is expected and safe.

### Homebrew prefix permission errors

The script creates or fixes ownership of the Homebrew prefix directory.
You'll be prompted for your password via `sudo`.

### Network timeouts

If `brew update` or package downloads timeout, re-run the script.
It is idempotent and will pick up where it left off.

### Third-party taps

This project uses the following third-party Homebrew taps:

| Tap | Purpose |
|-----|---------|
| `dteoh/sqa` | SQL query analyzer |
| `hashicorp/homebrew-tap` | Terraform and HashiCorp tools |
| `jackielii/tap` | Community formulae |
| `romkatv/powerlevel10k` | Zsh prompt theme |
| `warrensbox/homebrew-tap` | tfswitch (Terraform version manager) |

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b add-my-tool`)
3. Edit [`Brewfile`](Brewfile) to add or remove packages
4. Commit your changes (`git commit -m "add my-tool"`)
5. Push and open a Pull Request

### Adding a package

Add it to the appropriate section in `Brewfile`:

```ruby
# CLI Utilities
brew "my-new-tool"
```

### Adding a cask

```ruby
# Casks - Productivity
cask "my-new-app"
```

### Adding a Mac App Store app

```ruby
mas "App Name", id: 123456789
```

You can find the App Store ID from the app's URL on the Mac App Store.

## Credits

Originally based on [thoughtbot/laptop](https://github.com/thoughtbot/laptop).
Licensed under the MIT License. See [LICENSE](LICENSE) for details.
