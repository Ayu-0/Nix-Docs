## Nix-Flakes : What is this?

Nix Flakes is the modern, standardized packaging and configuration system. It replaces the traditional channels system by strictly defining dependencies (inputs) and generating a flake.lock file, which guarantees that your system builds identically across different machines

To use flakes, you need to create the file flake.nix, which will be the basis of everything. but first you have to enable it via configuration.nix file
 `nix.settings.experimental-features = [ "nix-command" "flakes" ];`

Explanation : someone from reddit
The basic idea of flakes isn't that complicated, but it does require a little bit of basic understanding of the nix programming language. 
A flake.nix file is an attribute set with two attributes called inputs and outputs. The inputs attribute describes the other flakes that you would like to use; 
things like nixpkgs or home-manager. You have to give it the url where the code for that other flake is, and usually people use GitHub. The outputs attribute is a function, 
which is where we really start getting into the nix programming language. Nix will go and fetch all the inputs, load up their flake.nix files, 
and it will call your outputs function with all of their outputs as arguments. The outputs of a flake are just whatever its outputs function returns,
which can be basically anything the flake wants it to be. Finally, nix records exactly which revision was fetched from GitHub in flake.lock so that the versions
of all your inputs are pinned to the same thing until you manually update the lock file.


As of now I have to learn quite a lot....I'm going... bye bye
