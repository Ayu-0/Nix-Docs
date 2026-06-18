### This workaround for setting up home-manager in nixos

**Add home-manager channel**
```
nix-channel --add https://github.com/nix-community/home-manager/archive/master.tar.gz home-manager
nix-channel --update
```
**Run this command to initialize the configuration:**
```
nix-shell '<home-manager>' -A install
```

**Still not worked? Explicitly enable home-manager**
`
programs.home-manager.enable = true;
`

**Run the activation command, Again**
```
nix-shell '<home-manager>' -A install
```
