# remote

A set of commands to set up and manage data remotes:
[add](/doc/command-reference/remote/add),
[default](/doc/command-reference/remote/default),
[list](/doc/command-reference/remote/list),
[modify](/doc/command-reference/remote/modify), and
[remove](/doc/command-reference/remote/remove).

## Synopsis

```usage
usage: dvc remote [-h] [-q | -v] {add,default,remove,modify,list} ...

positional arguments:
  COMMAND
    add                 Add remote.
    default             Set/unset default remote.
    remove              Remove remote.
    modify              Modify remote.
    list                List available remotes.
```

## Description

What is data remote?

The same way as GitHub provides storage hosting for Git repositories, DVC
remotes provide a central place to keep and share data and model files. With
this remote storage, you can pull models and data files created by colleagues
without spending time and resources to build or process them locally. It also
saves space on your local environment – DVC can
[fetch](/doc/command-reference/fetch) into the <abbr>cache directory</abbr> only
the data you need for a specific branch/commit.

DVC supports several types of remote storage: local file system, SSH, Amazon S3,
Google Cloud Storage, HTTP, HDFS, among others. Refer to `dvc remote add` for
more details.

> If you installed DVC via `pip`, depending on the remote storage type you plan
> to use you might need to install optional dependencies: `[s3]`, `[ssh]`,
> `[gs]`, `[azure]`, `[gdrive]`, and `[oss]`; or `[all]` to include them all.
> The command should look like this: `pip install "dvc[s3]"`. This installs
> `boto3` library along with DVC to support S3 storage.

Using DVC with a remote data storage is optional. By default, DVC is configured
to use a local data storage only (usually the `.dvc/cache` directory). This
enables basic DVC usage scenarios out of the box.

[Add](/doc/command-reference/remote/add),
[default](/doc/command-reference/remote/default),
[list](/doc/command-reference/remote/list),
[modify](/doc/command-reference/remote/modify), and
[remove](/doc/command-reference/remote/remove) commands read or modify DVC
[config files](/doc/command-reference/config). Alternatively, `dvc config` can
be used or these files could be edited manually.

For the typical process to share the <abbr>project</abbr> via remote, see
[Sharing Data And Model Files](/doc/use-cases/sharing-data-and-model-files).

## Options

- `-h`, `--help` - prints the usage/help message, and exit.

- `-q`, `--quiet` - do not write anything to standard output. Exit with 0 if no
  problems arise, otherwise 1.

- `-v`, `--verbose` - displays detailed tracing information.

## Example: Add a default local remote

<details>

### What is a "local remote" ?

While the term may seem contradictory, it doesn't have to be. The "local" part
refers to the machine where the project is stored, so it can be any directory
accessible to the same system. The "remote" part refers specifically to the
project/repository itself.

</details>

```dvc
$ dvc remote add -d myremote /path/to/remote
$ dvc remote list
myremote        /path/to/remote
```

The <abbr>project</abbr>'s config file should now look like this:

```ini
['remote "myremote"']
url = /path/to/remote
[core]
remote = myremote
```

## Example: Add Amazon S3 remote and modify its region

> **Note!** Before adding a new remote be sure follow the instructions at
> [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

```dvc
$ dvc remote add mynewremote s3://mybucket/myproject
$ dvc remote modify mynewremote region us-east-2
```

## Example: Remove remote:

```dvc
$ dvc remote remove mynewremote
```
