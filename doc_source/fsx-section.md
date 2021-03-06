# `[fsx]` Section<a name="fsx-section"></a>

**Topics**
+ [`shared_dir`](#id15)
+ [`fsx_fs_id`](#fsx-fs-id)
+ [`storage_capacity`](#storage-capacity)
+ [`imported_file_chunk_size`](#imported-file-chunk-size)
+ [`export_path`](#export-path)
+ [`import_path`](#import-path)
+ [`weekly_maintenance_start_time`](#weekly-maintenance-start-time)

Defines configuration settings for an attached Amazon FSx for Lustre file system\. For more information about Amazon FSx for Lustre, see [Amazon FSx CreateFileSystem](https://docs.aws.amazon.com/fsx/latest/APIReference/API_CreateFileSystem.html)\.

Amazon FSx for Lustre is supported when `[`base_os`](cluster-definition.md#base-os)` is either `centos7` or `alinux`\.

When using Amazon Linux, the kernel must be >= `4.14.104-78.84.amzn1.x86_64`\. For detailed instructions, see [Installing the Lustre Client](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/install-lustre-client.html) in the *Amazon FSx for Lustre User Guide*\.

**Note**  
Amazon FSx for Lustre is not currently supported when using `awsbatch` as a scheduler\.

If using an existing file system, it must be associated to a security group that allows inbound TCP traffic to port `988`\. Setting the source to `0.0.0.0/0` on a security group rule provides client access from all IP ranges within your VPC security group for the protocol and port range for that rule\. To further limit access to your file systems we recommend using more restrictive sources for your security group rules, for example more specific CIDR ranges, IP addresses, or security group IDs\. This is done automatically when not using `[`vpc_security_group_id`](vpc-section.md#vpc-security-group-id)`\.

To use an existing Amazon FSx file system, specify `[`fsx_fs_id`](#fsx-fs-id)`\.

The format is `[fsx <fsxname>]`\.

```
[fsx fs]
shared_dir = /fsx
fsx_fs_id = fs-073c3803dca3e28a6
```

To create and configure a new file system, use the following parameters:

```
[fsx fs]
shared_dir = /fsx
storage_capacity = 3600
imported_file_chunk_size = 1024
export_path = s3://bucket/folder
import_path = s3://bucket
weekly_maintenance_start_time = 1:00:00
```

## `shared_dir`<a name="id15"></a>

 **\(Required\)** Defines the mount point for the Amazon FSx for Lustre file system on the master and compute nodes\.

Do not use `NONE` or `/NONE` as the shared directory\.

The following example mounts the file system at `/fsx`\.

```
shared_dir = /fsx
```

## `fsx_fs_id`<a name="fsx-fs-id"></a>

**\(Optional\)** Attaches an existing Amazon FSx file system\.

If this option is specified, only the [`shared_dir`](#id15) and [`fsx_fs_id`](#fsx-fs-id) settings in the [[fsx] section](#fsx-section) are used and any other settings in the [[fsx] section](#fsx-section) are ignored\.

```
fsx_fs_id = fs-073c3803dca3e28a6
```

## `storage_capacity`<a name="storage-capacity"></a>

**\(Required\)** Specifies the storage capacity of the file system, in GiB\.

The storage capacity has possible values of 1200, 2400, and any multiple of 3600\.

```
storage_capacity = 3600
```

**Note**  
Prior to AWS ParallelCluster 2\.5\.0, `storage_capacity` had a minimum size of 3600\.

## `imported_file_chunk_size`<a name="imported-file-chunk-size"></a>

**\(Optional\)** Determines the stripe count and the maximum amount of data per file \(in MiB\) stored on a single physical disk, for files that are imported from a data repository \(using `import_path`\)\. The maximum number of disks that a single file can be striped across is limited by the total number of disks that make up the file system\.

The chunk size default is `1024` \(1 GiB\), and it can go as high as 512,000 MiB \(500 GiB\)\. Amazon S3 objects have a maximum size of 5 TB\.

```
imported_file_chunk_size = 1024
```

## `export_path`<a name="export-path"></a>

**\(Optional\)** Specifies the Amazon S3 path where the root of your file system is exported\. The path **must** be in the same Amazon S3 bucket as the `import_path` parameter\.

The default value is `s3://import-bucket/FSxLustre[creation-timestamp]`, where `import-bucket` is the bucket provided in the `[`import_path`](#import-path)` parameter\.

```
export_path = s3://bucket/folder
```

## `import_path`<a name="import-path"></a>

**\(Optional\)** Specifies the S3 bucket to load data from into the file system\. Also serves as the export bucket\. For more information, see `[`export_path`](#export-path)`\.

Import occurs on cluster creation\. For more information, see [Importing Data from your Amazon S3 Bucket](https://docs.aws.amazon.com/fsx/latest/LustreGuide/fsx-data-repositories.html#import-data-repository) in the *Amazon FSx for Lustre User Guide*\.

If a value is not provided, the file system is empty\.

```
import_path =  s3://bucket
```

## `weekly_maintenance_start_time`<a name="weekly-maintenance-start-time"></a>

**\(Optional\)** Specifies a preferred time to perform weekly maintenance, in the UTC time zone\.

The format is \[day of week\]:\[hour of day\]:\[minute of hour\]\. For example, Monday at Midnight is:

```
weekly_maintenance_start_time = 1:00:00
```