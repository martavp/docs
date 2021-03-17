# Long term storage

## Ceph file system

The file systems mounted under `/home` and `/groups` are [ceph](https://ceph.io/) file systems. Your home folder is for configuration files and personal data, for example, ssh-keys, local copies of code for development, publication drafts and referee reports. Never store any data intended for sharing in your home folder.

Group folders are for data shared within a group of researchers, for example, members of a department or section, members of a project or users of a licensed or otherwise restricted software. Group folders should be used for storing all research data, including data resulting from master-, Ph.D.- and post-doc projects. Supervisors of such projects should ask for creation of a group folder for this purpose.

Furthermore, these two file systems are intended for storing *warm and cold data* but ***not** hot or temporary* data:

- Cold data (archival data): Data with a long life-time (years to decades to forever). This data is typically immutable and non-reproducible.
- Warm data (work in progress data): Data with intermediate life-time (days to weeks to months). Some of this data will become cold data, others will be deleted at the end of a project or after publication.
- Hot data (job working set): Data with a life-time of a job execution. This is all data that is required during a run of a job, but has no purpose after job completion.

**Never save hot data on the ceph file systems!** This produces unnecessary load on the shared file systems and will pollute the ceph snapshots. Please use the burst buffer or the local RAM disk for hot and temporary data.


### Performance considerations

The ceph filesystem is optimized for capacity, data protection, cost and sequential throughput - in this order. In addition, ceph is not a parallel file system like, for example, Lustre or BeeGFS. This has important implications on performance and how to run jobs using the various file systems in the best possible way.


#### Ideal work flow

The ceph file system is designed for best performance with a temporary scratch workflow:

1. Copy multi-access data to temporary storage (burst buffer or RAM disk). Ideally, this is an extraction from a single large archive (tar, hdf5, zip, ...) directly to temporary storage, which will benefit from the high streaming bandwidth of ceph.
1. Data accessed only once by an application should be directly read from ceph. Ideally, access is sequential from large files.
1. During job execution, all new data is produced on temporary storage. Use the burst buffer if parallel multi-node access is required, or the RAM disk for single-node access. RAM disk is the fastest option but provides also the smallest amount of temporary storage capacity.
1. At the end, copy all permanent data back to ceph. Ideally, this is a creation of an archive (tar, hdf5, zip, ...) directly to ceph, which will again benefit from the high streaming bandwidth of ceph.

You can use a simplified workflow if your application

1. sequentially reads some (large) files only once,
1. does not require any disk access during computation and
1. writes some (large) files sequentially after completion.

Here, 'some' is a small number, like 5-10 files but not hundreds or thousands. Applications with this access pattern should run directly on the ceph file systems.


#### Performance characteristics, Do's and Dont's

Our ceph file systems

- are optimized for large-file I/O. Creating, copying, deleting, listing large numbers of small files is slow. Ideally, use archival data formats like tar or hdf5 on ceph.
- provide ca. 2000 aggregated IOP/s (random 4K writes) and an aggregated bandwidth of ca. 4.7GB/s (sequential 1M writes).
- reach link speed (ca. 850MB/s) with single node sequential 1M writes.

Differently to the [Lustre](https://www.lustre.org/) file system used on Sophia predecessor systems, the ceph file system is *not* a parallel file system. Furthermore, the ceph file system is *not* a low-latency storage system either, the latency is of the order of 1ms. The ceph file system is designed for cloud applications, where every client (VM, user, app) has exclusive access to its own directory sub-tree. In other words, no two clients access files in the same directory sub-tree.

While ceph supports multi-client (multi-node) access to files in the same directory sub-tree, this access is *not* necessarily concurrent as with parallel file systems. Rather, the ceph storage cluster serializes concurrent access whenever necessary to present consistent data and meta-data information across all clients. In addition, ceph maintains cache coherency between clients, meaning that a write on one node can invalidate read cache on another node.

These characteristics can result in an counter-intuitive experience when transitioning from a parallel file system to a ceph file system. Most of the pitfalls can be avoided by following our ideal workflow outlined above. We hope this "Do's and Dont's" list helps avoiding others:

- *Do* check this [best practices guide](https://docs.ceph.com/docs/master/cephfs/app-best-practices/) for good and bad workloads.
- *Don't* use [multi-node concurrent file access](https://docs.ceph.com/docs/master/cephfs/cephfs-io-path/). *Do* collection of data on the job's master node and write data to disk only from the master node.
- *Don't* send a ticket just because this `ls -l` doesn't finish in 5s. A slow response is almost certainly *not* indicating a malfunction of the ceph cluster, but rather a bad workload like concurrent access of a directory sub-tree.
- *Don't* run a job and check its progress with `ls -l` on the head node. This creates a concurrent access with all the synchronization overhead. It will not only be slow on the head node, it will also stall the job. *Do* ssh/mrsh into the master node of the job and run your `ls -l` there (i.e. use single-node access).
- *Don't* submit a sequence of jobs using the same application. After each job the SLURM prologue script flushes the file system cache on the compute nodes. To avoid reloading the same data from disk for every computation, *do* run a sequence of computations with the same application in one job. This is particularly important for applications that open thousands of files, like Matlab. Use `srun` to execute single-application multiple-input jobs in an efficient way.
- *Don't* call `fflush` and `fsync`, `sync` excessively, for example, after every single write. Let writes accummulate in the system buffer and call one of these functions after at least 4MB worth of writes are processed.
- *Do* [use `fclose()` after writes are completed to ensure data is committed](https://docs.ceph.com/docs/master/cephfs/full/).



### Snapshots

In case of accidental deletion, corruption or modification of data by malware, previous versions are available in ceph snapshots. To access a snapshot of a file, change to the file's directory and execute `cd .snap`. Note that ".snap" is a meta-directory, not a real directory. Therefore, tab-completion and the like will not work and one needs to type it explicitly as stated here. Inside the snapshot directory are folders with current snapshots, for example,

    [frans@sophia1 .snap]$ ls
    _2021-02-23_183554+0100_weekly_1099511719926  _2021-02-27_000611+0100_daily_1099511719926
    _2021-02-24_000611+0100_daily_1099511719926   _2021-02-28_000611+0100_daily_1099511719926
    _2021-02-25_000611+0100_daily_1099511719926   _2021-03-01_000611+0100_daily_1099511719926
    _2021-02-26_000611+0100_daily_1099511719926   _2021-03-01_000911+0100_weekly_1099511719926

To access snapshots of a specific date, cd into the corresponding folder and browse through the files. Note that files in snapshots are read-only regardless of the file permissions shown by `ls -l`.

!!! info "Disclaimer"
    Snapshots are provided as a *courtesy service* on the ceph file systems `/home` and `/groups`. The availability of snapshots depends on the amount of currently unused ceph storage capacity and the retention time may be reduced without warning to adjust to increased utilisation. The default retention scheme is:

    - daily snapshots for 7 days and
    - weekly snapshots for 4 weeks.

    We cannot guarantee snapshots to be present in situations where excessive amounts of temporary data have been captured in snapshots. We aim, however, to have at least the last two days available.


### Advanced topics

- [LazyIO provided by libcephfs](https://docs.ceph.com/docs/master/cephfs/lazyio/)
- [Ceph fs and POSIX](https://docs.ceph.com/docs/master/cephfs/posix/).
