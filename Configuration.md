## Let's talk abt configuration.nix

As you know this is declarative file and it uses nix language to describe your system and nix itself does all the work to make that system the way you wanted.
Got it... Lets understand configuration then;

#### Basic Structure 

```
{ config, pkgs, ... }:

{
  # Your configuration options go here
}
```

`{ config, pkgs, ... }` : This inputs system packages (pkgs) and system configurations (config) into the file.
`{ ... }` The main body where you define your settings.

```
{ config, pkgs, ... }:

{
  # Import your hardware scan results
  imports = [ ./hardware-configuration.nix ];

  # Use the systemd-boot EFI boot loader
  boot.loader.systemd-boot.enable = true;
  boot.loader.efi.canTouchEfiVariables = true;

  # Set your time zone and networking info
  time.timeZone = "America/New_York";
  networking.hostName = "nix-laptop";
  networking.networkmanager.enable = true;

  # Enable a Desktop Environment (GNOME)
  services.xserver.enable = true;
  services.xserver.desktopManager.gnome.enable = true;
  services.xserver.displayManager.gdm.enable = true;

  # Define user accounts
  users.users.alex = {
    isNormalUser = true;
    extraGroups = [ "wheel" "networkmanager" ]; # Enable sudo
    packages = with pkgs; [ firefox thunderbird ]; # User-specific apps
  };

  # System-wide packages
  environment.systemPackages = with pkgs; [
    vim
    git
    wget
    curl
  ];

  # Allow proprietary software (like Nvidia drivers or Steam)
  nixpkgs.config.allowUnfree = true;

  # This value determines the NixOS release from which your default
  # settings are taken. Do not change this after installation.
  system.stateVersion = "23.11"; 
}
```

**After defining your sytem, run following to build the system**
```
sudo nixos-rebuild switch  # Applies changes immediately to your running system
sudo nixos-rebuild boot  # sets the changes to apply only after the next system restart 
```
> [!NOTE]
> `sudo nixos-rebuild switch --upgrade` : It forcefully updates your root nix-channel to the absolute latest commit available upstream. It updates your underlying package sources first, then builds the system, Because the channel updates to a brand new commit instantly, you ran into a classic Nix timing problem: you got ahead of the binary cache, Since Nix couldn't find precompiled binaries on cache.nixos.org for that exact hourly commit, it fallback to building packages locally on your machine. The official Nix build servers (Hydra) take a few hours to compile everything every time a new change is pushed to the repository.

&nbsp;

As you understand, this is system wide installation, you don't want everytime & everything to install this way, to fix this is issue there is stuff called `home-manager` which lets you install packages to user specific.

> OK, what if you want to install packages temp : You can use `nix-shell` for this
> What if you want to set only specific packages, dependencies to your work env : use `nix develop`
> What if you want to install package that is not in nix package? : use `nix flakes`

I'll write separate docs for this...
