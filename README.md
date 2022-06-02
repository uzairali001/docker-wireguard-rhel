# Configure Rocky Linux / Centos for linuxserver/docker-wireguard

**Imporant:** Make sure secure boot is disabled on host OS i.e. Rocky Linux / Centos

## Step 1: install epel and elrepo
```
sudo dnf install elrepo-release epel-release -y
```
### Step 1 - optional: fix elrepo mirror issue
if you face any issue running `dnf update` after installing `elrepo` then edit `/etc/yum.repos.d/elrepo.repo` and comment out `mirrorlist=http://mirrors.elrepo.org/mirrors-elrepo.el8`
```
sudo vi /etc/yum.repos.d/elrepo.repo
# mirrorlist=http://mirrors.elrepo.org/mirrors-elrepo.el8
```

## Step 2: update
```
sudo dnf update -y
```

## Step 3: install wireguard and enable it
```
sudo dnf install kmod-wireguard wireguard-tools -y
sudo dnf copr enable jdoss/wireguard
sudo dnf update -v
sudo dnf install wireguard-dkms -y
sudo modprobe wireguard
```

## Step 4: reboot the server
```
reboot
```

Now you should be able to run wireguard on Rocky Linux / Centos
```
git clone https://github.com/uzairali001/docker-wireguard-rhel/ ./wireguard
cd ./wireguard
docker compose up -d
```

if you get this error
```
modprobe: ERROR: could not insert 'ip_tables': Exec format error
wireguard  | iptables v1.6.1: can't initialize iptables table `filter': Table does not exist (do you need to insmod?)
wireguard  | Perhaps iptables or your kernel needs to be upgraded.
wireguard  | [#] ip link delete dev wg0
```
then run this
```
sudo modprobe iptable_raw
```
To make it load automatically after reboot
```
sudo echo "iptable_raw" | sudo tee /etc/modules-load.d/iptable_raw.conf
```
