## Introduction to NixOs :  An Overview
NixOS is a Linux distribution built entirely around the Nix package manager. It's declarative system means you write a single configuration file that tells what packages 
you want to install, which option/service to enable & include other program or system module specific configuration. This configuration describe everything about
how your system will run. It uses it's own language called nix language to serve this purpose.

In normal or traditional linux distro, files are grouped by type like, all binaries go to /bin, libraries to /lib, and configurations to /etc, right! They share libraries, In this way two programs needing different versions of the same library? Good luck — they'll fight over the same files, but this boi doesn't give a shit to that. It completely rejects the traditional Linux Filesystem Hierarchy Standard. It forges its own path and pull all bitches(programs) to `/nix/store`.

Now you get an idea where it store all the packages, but how it manage so many & different packages? -  It manage via cryptographic hashes and symlinking. Instead of dumping every file into one messy pile, `/nix/store` functions like a database where **every single package version gets its own completely isolated subfolder.** When Nix builds or downloads a package, it calculates a unique 32-character hash. This hash is computed from every single input that goes into making that program, Like :
* The exact source code version
* The exact compiler version used
* Every configuration flag enabled
* All specific library dependencies required
  
If you change even a single character in a configuration, the hash changes. This allows completely different versions of the same package to live in /nix/store side-by-side without overlapping.
When NixOS compiles a program, it modifies the executable so that it looks only inside the specific /nix/store folders it needs. In traditional Linux,
a program looks for libc.so globally in /lib. In NixOS, the executable contains a hardcoded path directly to its exact dependency:
`LOOK_FOR_LIBRARY = /nix/store/h93...-glibc-2.37/lib/libc.so`
Because the program never looks outside its assigned /nix/store paths, it is completely self-contained and immune to global system updates 

Everytime you make a changes to your system, all previous versions of your system are saved. If you break your system trying something, you can restart the machine, choose then previous version in grub and back again... yooo, cool hn...


#### If everything is locked away in its own isolated /nix/store folder, how does your terminal find them when you type a command?

NixOS solves this by using symlinks (shortcuts) to dynamically build a temporary directory structure called a profile. When you log in, NixOS generates a custom folder
for your user that aggregates all your active programs into a clean layout. NixOS then simply points your system's $PATH environment variable to this ~/.nix-profile/bin/ folder.
When you run firefox, your system goes through the shortcut and executes the real binary hiding inside the store


Because every package installation creates a brand-new folder, you might worry about running out of disk space.When you upgrade a package, NixOS doesn't delete the old folder immediately—it just changes the symlinks to point to the new folder. The old folder remains in the store so you can rollback instantly if needed
To delete old pacakge just 

```
# Remove all old generations + unreferenced store paths
nix-collect-garbage -d

# Only clean up packages not used by current gen (safer)
nix-collect-garbage
```

#### Where main system-wide configuration file lives?
This file is used to declare and configure your entire operating system, hardware, user accounts, and system services
```
/etc/nix/configuration.nix
```

&nbsp;

### Comparision with Normal Linux distro :

| Traditional Linux (Debian, Arch etc... )  | NixOS                                               |
| ----------------------------------------- | --------------------------------------------------- |
| Install packages directly into the system | Packages are stored in the Nix store (`/nix/store`) |
| Edit configuration files manually         | Declare configuration in a Nix file                 |
| Upgrades modify the existing system       | Upgrades create a new system generation             |
| Rolling back can be difficult             | Rollbacks are built in                              |
| Different machines may drift over time    | Systems stay reproducible from the same config      |


&nbsp; 
