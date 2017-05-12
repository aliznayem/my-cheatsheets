 Installed openfoam4.1 form https://openfoam.org/download/4-1-linux/
 
 ## Installation flow for Arch linux ##
 
 # Install docker
 Docker is a lightweight package container. Container is kind of Virtual Machine but cantains only libraries and settings required to make the software work are needed. 
 
 pacman -Sy  (update indexes)
 
 pacman -S docker (install)
 
 # Enable docker service
 sudo systemctl enable docker.service (to start service at startup)
 
 sudo systemctl start docker
 
 # Install openfoam
 sh -c "wget http://dl.openfoam.org/docker/openfoam4-linux -O /usr/bin/openfoam4-linux"
 
 chmod 755 /usr/bin/openfoam4-linux
 
 # Launching openfoam
 mkdir -p $HOME/OpenFOAM/${USER}-4.1
 
 cd $HOME/OpenFOAM/${USER}-4.1
 
 openfoam4-linux
 
 First run takes several time to download bunch of data (around 600-700MB)
 
 # Testing with a sample
 mkdir -p $FOAM_RUN
 
 cd $FOAM_RUN
 
 cp -r $FOAM_TUTORIALS/incompressible/simpleFoam/pitzDaily .
 
 cd pitzDaily
 
 blockMesh
 
 simpleFoam
 
 paraFoam
 
 # Problems faced
 Protocol not defined
 
 Cannot connect to X server:0.0
 
 # Solved by running
 xhost +local:root 

