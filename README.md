# setup

Everything that makes up my machines, as git submodules:

- `nixos`: the NixOS flake (hosts, systems, apps). The entry point.
- `pi-config`: Home Manager module for pi, input `pi-config` of `nixos`.
- `agent-markdown-files`: AGENTS.md, agents and skills, input of `pi-config`.
- `quickshell-bar`: the top bar, input `quickshell-bar` of `nixos`.

```sh
git clone --recursive https://github.com/nicolaj-blach/setup ~/setup
sudo nixos-rebuild switch --flake ~/setup/nixos#personal-desktop
```

`nixos/flake.lock` is the real pin; the submodule commits here are only for cloning.
Submodules check out detached: run `git switch <branch>` before working in one, or
`git submodule update --remote` to move all of them to their branch heads.

Test a local change without pushing:

```sh
nix build --no-link ~/setup/nixos#nixosConfigurations.personal-desktop.config.system.build.toplevel \
  --override-input pi-config ~/setup/pi-config \
  --override-input pi-config/agent-markdown-files ~/setup/agent-markdown-files \
  --override-input quickshell-bar ~/setup/quickshell-bar
```
