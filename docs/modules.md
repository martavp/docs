# Software modules

List available modules
```md
ml avail
```
and load a particular module, e.g.
```md
ml OpenMPI/3.1.1-GCC-7.3.0-2.30
```
to use [OpenMPI](https://www.open-mpi.org/) version 3.1.1 compiled with [GNU Compiler Collection](https://gcc.gnu.org/) version 7.3.

To get an overview of modules into your environment do
```md
ml
```

Unload a particular module with `ml unload <module>`, e.g.
```md
ml unload GCC/7.3.0-2.30
```
or just `ml unload GCC` will remove the GNU Compiler Collection from the environment
(and OpenMPI would stop functioning!).


## Installing software

As an alternative to building software manually a HPC package manager can be used.

### EasyBuild

Sophia users can load the [EasyBuild](https://easybuild.io/) framework with
```md
ml EasyBuild
```
and e.g. go through [a workflow example](https://docs.easybuild.io/en/latest/Typical_workflow_example_with_WRF.html) to get the hang of it. An alphabetically sorted list of software recipies - so-called *easyconfigs* - are
[available on GitHub](https://github.com/easybuilders/easybuild-easyconfigs/tree/develop/easybuild/easyconfigs).

### Spack

Another option is the [Spack package manager](https://spack.io/); we refer to [the docs](https://spack.readthedocs.io/en/latest/) for
further information.
