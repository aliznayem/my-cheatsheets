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
