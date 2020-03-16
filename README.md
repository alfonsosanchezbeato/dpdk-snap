# dpdk-snap

This is the snapcraft.yaml to build a DPDK Snap.
It is currently still under development.

To build, you need to have snapcraft installed on your system (Ubuntu 18.04):

```
sudo snap install snapcraft --classic 
git clone https://github.com/wililupy/dpdk-snap
cd dpdk-snap
snapcraft
```
Once the snap is installed, you will need to connect these plugs:

```
snap connect dpdk-wililupy:dpdk-control
snap connect dpdk-wililupy:hardware-observe
snap connect dpdk-wililupy:hugepages-control
snap connect dpdk-wililupy:kernel-module-control
snap connect dpdk-wililupy:kernel-module-observe
snap connect dpdk-wililupy:network
snap connect dpdk-wililupy:network-bind
snap connect dpdk-wililupy:network-control
snap connect dpdk-wililupy:network-observe
snap connect dpdk-wililupy:process-control
snap connect dpdk-wililupy:system-observe
```
Then you can test by doing the following (/dev/hugepages can be another
folder accessible by the snap):

```
sudo -s
mkdir -p /dev/hugepages
mount -t hugetlbfs -o pagesize=2M nodev /dev/hugepages
echo 1024 > /sys/devices/system/node/node0/hugepages/hugepages-2048kB/nr_hugepages
sudo dpdk-wililupy.testpmd -- -i
```

Instead of dynamically creating the hugepages pool, it is possible to
creating them by using kernel parameters. For instance, on Mellanox
SmartNICs, add to the kernel command line:

    default_hugepagesz=2097152 hugepagesz=2097152 hugepages=1024

Make sure you have compiled the snap on a chroot with the appropriate
Mellanox libraries installed.
