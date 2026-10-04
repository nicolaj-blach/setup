# setup

Parent repo of Nicolaj Blach Jensen's setup (`github.com/nicolaj-blach/setup`, private). Each folder is its own git repo, added here as a submodule. Read the submodule's own `AGENTS.md` or README before changing it.

## Repos
| Folder | Branch | What | Used by |
|---|---|---|---|
| `nixos` | `modix-config` | NixOS flake: hosts, systems, apps. The entry point. | — |
| `pi-config` | `main` | Home Manager module for pi: wrapper, settings, packages, theme, permissions. | input `pi-config` of `nixos` |
| `agent-markdown-files` | `main` | `AGENTS.md`, `assistant.md`, subagents and skills. | input `pi-config/agent-markdown-files` |
| `quickshell-bar` | `main` | The top bar (QML). | input `quickshell-bar` of `nixos` |

`nixos/secrets` is a nested private submodule (sops). Never print or decrypt secrets.

## How changes flow
`nixos` gets the other repos from GitHub, not from these folders. `nixos/flake.lock` is what gets built; the submodule commits in this repo only matter for cloning.

1. Change and commit in the submodule, then push.
2. In `nixos`: `nix flake update <input>` (`pi-config`, `pi-config/agent-markdown-files`, `quickshell-bar`), build, commit, push.
3. Optionally bump the pointer here: `git add <folder> && git commit`.

Test before pushing with `--override-input`, e.g. `--override-input pi-config ~/setup/pi-config` (see `README.md`).

## Rules
- Submodules check out detached: `git switch <branch>` before committing in one.
- Never commit or push unless I ask. Commits are signed automatically.
- Never run `sudo`, `nixos-rebuild` or `home-manager switch`. I rebuild with `sudo nixos-rebuild switch --flake ~/setup/nixos#personal-desktop`.
