# gazebo_tensorflow_turtlebot_docker

Reader, VirtualGL did not work on the lab machine, but did (as the commit points out) work on MY machine.

So, you can't run GLX applications, but you can run gzserver and tensorflow. You need to have access to a "real" X server in order to run VirtualGL. There's something "unreal" about the X Server I started with `xinit -- :1` on the lab machine.

## In progress debugging

The difference between local and lab gpu are first noticed by the errors I got trying to run the following on the lab machine.
```
export DISPLAY=:0
xhost +si:localuser:root
```
or
```
xinit -- :1
export DISPLAY=:1
xhost +si:localuser:root
```
