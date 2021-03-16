# Temporary storage performance-hierachy

The temporary storage options are listed below; fastest first.

## Each compute node's local `/tmp` directory

Sophia compute nodes are *ephemeral* meaning they fetch a minimal
[CentOS](https://www.centos.org/) installation at each reboot which then runs in the node's memory.

This means that the underlying [XFS](https://www.kernel.org/doc/html/latest/admin-guide/xfs.html)
file system runs on the compute node's random access memory
hardware with orders of magnitude higher bandwidth and lower latency compared with disk drives.
Thus, e.g. to eliminate I/O as the limiting factor in a application performance benchmark
experiment, a Sophia user can run test code from the `/tmp` directory on up to
32 MPI processes on a single Sophia node with 32 physical cores.

## Burst buffer

Two servers with
[NVMe disks](https://www.samsung.com/semiconductor/global.semi.static/Brochure_Samsung_PM1725a_NVMe_SSD_1805.pdf) - Sophia's burst buffer nodes - are connected to
Sophia compute nodes via the HPC cluster's Mellanox EDR Infiniband interconnect,
and each server connects to the Ceph file system via bonded 10Gbps connections
(i.e. 20Gbps).

Sophia's burst buffer runs the [BeeGFS file system](https://www.beegfs.io), striping data written from 
compute nodes across BeeGFS storage targets.
The storage resource is mounted on `/mnt/beegfs` on Sophia's head node and all compute nodes and 
current usage is manual. Remember to clean up after use since lingering data is removed periodically.
