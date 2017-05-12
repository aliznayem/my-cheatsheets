AUR packages cannot be installed as root user. So first you need to add a non root user.
## Add a User ##
```
useradd -m -g [initial group] -G [additional groups] -s [login shell] [username]
```
`-m` creates the user home directory as /home/username.

`-g` defines the group name or number of the user's initial login group. If specified, the group name must exist; if a group number is provided, it must refer to an already existing group.

`-G` introduces a list of supplementary groups which the user is also a member of. Each group is separated from the next by a comma, with no intervening spaces. 

`-s` defines the path and file name of the user's default login shell. 

N.B. If initial group is not defined. user name and initial group name will be same, that is UID = GID.

Setting of password:
```
passwd username
```
**Example:**
```
useradd -m -G wheel -s /bin/bash arch
passwd arch
```

## Install Package ##
#### Change User ####
```
su username
```
#### Acquire Build Files ####
There are several methods to acquire build files. Can be downloaded directly or cloning git repository. 
Clone git repository in a local directory:
```
cd ~/example
git clone https://aur.archlinux.org/package_name.git
```
Or download from web browser and extract:
```
tar -xvf package_name.tar.gz
```
#### Build and Install Packages ####
cd to `PKGBUILD` containing directory. Should carefully check `PKGBUILD` or `.install` file if containing any malicious command.
```
cd package_name
vim PKGBUILD
vim package_name.install
```
Make the package. After manually confirming the integrity of the files, run makepkg as a normal user: 
```
makepkg -si
```
`-s/--syncdeps` automatically resolves and install any dependencies with pacman before building. If the package depends on other AUR packages, you will need to manually install them first.

`-i/--install` installs the package if it is built successfully. Alternatively the built package can be installed with `pacman -U package.pkg.tar.xz`.


Other useful flags:

`-r/--rmdeps` removes build-time dependencies after the build, as they are no longer needed. However these dependencies may need to be reinstalled the next time the package is updated.

`-c/--clean` cleans up temporary build files after the build, as they are no longer needed. These files are usually needed only when debugging the build process.
