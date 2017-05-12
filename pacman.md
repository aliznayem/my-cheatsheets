Pacman is the package manager used by Arch Linux.

## Some Important Options ##

#### Search for a Package ####
Search for a package to install:
```
pacman -Ss string1 string2 ...
```
Search from installed packages:
```
pacman -Qs string1 string2 ...
```

#### Install a Package ####

```pacman -S package_name1 package_name2 ...```

#### Update and Upgrade ####
Package index update:

```pacman -Sy```

System upgrade:

```pacman -Su```

Do foresaid both tasks in one command:

```pacman -Syu```

#### Remove a Package ####

Just remove:

```pacman -R package_name```

To remove a package and its dependencies which are not required by any other installed package:

```pacman -Rs package_name```

To remove a package, its dependencies and all the packages that depend on the target package:

```pacman -Rsc package_name```

To remove a package, which is required by another package, without removing the dependent package:

```pacman -Rdd package_name```

Pacman saves important configuration files when removing certain applications and names them with the extension: .pacsave. To prevent the creation of these backup files use the `-n` option:

```pacman -Rn package_name```
