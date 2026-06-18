# This workaround for setting up home-manager in nixos

**Add home-manager channel**
```
nix-channel --add https://github.com/nix-community/home-manager/archive/master.tar.gz home-manager
nix-channel --update
```

**Explicitly enable Home Manager to manage itself**
`
programs.home-manager.enable = true;
`

**Run the activation command**
```
nix-shell '<home-manager>' -A install
```
