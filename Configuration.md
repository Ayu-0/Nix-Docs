## Let's talk abt configuration.nix

As you know this is declarative file and it uses nix language to describe your system and nix itself does all the work to make that system become real to the best of it's abilities. 
you're telling the system to run those commands and `YOU` are the one who builds the system. Got it... Lets understand configuration then;

#### Basic Structure 

```
{ config, pkgs, ... }:

{
  # Your configuration options go here
}
```


