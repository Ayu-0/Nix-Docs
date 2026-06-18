
### Add home-manager channel
> [!CAUTION]
> You must strictly match your Home Manager channel version with your system's underlying nixpkgs version. If you do not align them, your configuration will eventually break due to mismatched options and package attributes.  If you use Nix Flakes, you can do whatever you want without worrying about system channels
```
nix-channel --add https://github.com/nix-community/home-manager/archive/master.tar.gz home-manager
nix-channel --update
```
### Run this command to initialize the configuration:
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

### Checks for channel versioning
```
sudo nix-channel --list
```
You'll get output like this, In this case both are stable channel


`home-manager https://github.com/nix-community/home-manager/archive/release-26.05.tar.gz`

`nixos https://channels.nixos.org/nixos-26.05`

