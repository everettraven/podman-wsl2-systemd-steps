# podman-wsl2-systemd-steps
Repo to remember the steps taken to allow podman to run systemd within containers when using WSL2

*NOTE: This has only been tested in an Ubuntu 20.04 WSL2 installation*

# Change cgroup_manager in /etc/containers/containers.conf
Change cgroup_manager in /etc/containers/containers.conf to:
```
cgroup_manager="cgroupfs"
```
# Change events_logger in /etc/container/containers.conf
Change events_logger in /etc/container/containers.conf to:
```
events_logger="file"
```
# Make the systemd directory in the /sys/fs/cgroup folder
This is the folder that podman looks for when running systemd within a container to mount a volume to. Create it by running:
```
sudo mkdir /sys/fs/cgroup/systemd
```
*NOTE: This change is not saved in WSL2 and will be reset upon a wsl shutdown*

# Mount systemd folder to cgroup
Mount the folder to the cgroup file system
```
sudo mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/systemd
```
*NOTE: This change is not saved in WSL2 and will be reset upon a wsl shutdown*

# Perform on startup
To make sure that the folder is created and mounted on WSL startup, add this to the bottom of your ~/.bashrc file:
```
if [ ! -d "/sys/fs/cgroup/systemd" ]; then
  sudo mkdir /sys/fs/cgroup/systemd
  sudo mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/systemd
fi
```
