# Enable Intel MPI<a name="intelmpi"></a>

Intel MPI is available on the AWS ParallelCluster AMIs for `alinux`, `centos7`, and `ubuntu1604` [`base_os`](cluster-definition.md#base-os)\. Open MPI is placed on the path by default\. To enable Intel MPI instead of Open MPI, the Intel MPI module must be loaded\. The exact name of the module changes with every update\. To see which modules are available, run `module avail`\.

```
$ module avail

----------------------------------------- /usr/share/Modules/modulefiles ------------------------------------------
dot                        module-git                 null
intelmpi/2019.5            module-info                openmpi/4.0.2
libfabric-aws/1.8.1amzn1.2 modules                    use.own

------------------------------------------------ /etc/modulefiles -------------------------------------------------
mpi/mpich-3.0-x86_64 mpi/mpich-x86_64
```

To load a module, run `module load modulename`\. You can add this to the script used to run `mpirun`\.

```
$ module load intelmpi
```

To see what modules are loaded, run `module list`\.

```
$ module list
Currently Loaded Modulefiles:
  1) intelmpi/2019.5
```

To verify that Intel MPI is enabled, run `mpirun --version`\.

```
$ mpirun --version
Intel(R) MPI Library for Linux* OS, Version 2019 Update 5 Build 20190806 (id: 7e5a4f84c)
Copyright 2003-2019, Intel Corporation.
```

After the Intel MPI module has been loaded, multiple paths are changed to use the Intel MPI tools\. To run code that was compiled by the Intel MPI tools, the Intel MPI module must be loaded first\.

**Note**  
Prior to AWS ParallelCluster 2\.5\.0, Intel MPI is not available on the AWS ParallelCluster AMIs in China \(Beijing\) and China \(Ningxia\)\.