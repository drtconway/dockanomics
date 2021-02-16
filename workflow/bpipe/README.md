# bpipe

This Docker image provides a containerized way to run [bpipe](http://docs.bpipe.org/) workflows.

From the bpipe documentation:

> Many people working with bioinformatics data end up running jobs
> as shell scripts. While this makes running them easy it has a lot
> of limitations. For example, when scripts fail half way through it
> is often hard to tell where, or why they failed, and even harder
> to restart the job from the point of failure. There is no automatic
> log of the commands executed or automatic capture of console output
> to ensure it is possible to later on see what happened. Sometimes
> jobs fail half way through, leaving half created files that can get
> confused with good files. Modifying the pipeline is also time
> consuming and error prone - adding or removing a stage causes
> modifications in multiple places, meaning that it is all too easy
> to have a change of file name cause later commands to fail or worse,
> run on incorrect data. Bpipe tries to solve all these problems (and
> more!) while departing as little as possible from the simplicity
> of the shell script. In fact, your Bpipe scripts will often look
> very similar to the original shell scripts you might have started
> with.

## Singularity-in-Docker

To make it easier to run workflows within a containerized environment,
this image includes [singularity](https://sylabs.io/singularity/).
Using singularity has two benefits:

* Running workflow stages inside containers gives the usual benefits of
  isolation and platform independence, as well as making it easier to
  control precisely which tool versions are run, and allows different
  workflows to use different version - or even to use multiple different
  versions within a single workflow if necessary.

* When running a workflow inside a container it is generally inconvenient
  to install new tools, since the container itself is ephemeral, so it
  would otherwise be necessary to create a new image with the extra tools
  _baked in_. By contrast, a singularity image is a single file that can
  be added in a mounted volume, making it easy to _install_ and persist
  between containers.

Note that in order to run singularity inside a docker container, it is
neccessary to use the `--privileged` flag. 

### Siffy

The bpipe image includes a simple shell-script `siffy` to pull down
Docker images and convert them to Singularity images. It simply takes
`docker://` image URLs and invokes `singularity pull`, with appropriate
name-mangling to save the converted image into a directory with a
repository of such images. For each URL, it also prints the full pathname
for the Singularity image it just created. By default, images are saved
into `/var/lib/singularity`, but this can be over-ridden by setting the
`SINGULARITY` environment variable.

Here is an example of how this can be used:

```
$ docker run -it --privileged -v ${PWD}/singularity/:/var/lib/singularity/ dockanomics/bpipe:latest bash
root@9faa3926fc1a:/data# ls -lh /var/lib/singularity
total 0
root@9faa3926fc1a:/data# siffy docker://dockanomics/bwa:latest
/var/lib/singularity/dockanomics_bwa_latest.sif
root@9faa3926fc1a:/data#
root@9faa3926fc1a:/data# ls -lh /var/lib/singularity
total 63M
-rwxr-xr-x 1 root root 62M Feb 12 01:58 dockanomics_bwa_latest.sif
root@9faa3926fc1a:/data#

```

## Example

In order to provide data, including reference files to run the workflow,
we typically need to mount several volumes on the docker container. To
make this easier to read and to save typing (in the long run!), it is
quite convenient to use `docker-compose` to specify the parameters for
creating the container:

```yaml
version: '3'
services:
  bpipe:
    image: dockanomics/bpipe:latest
    privileged: true
    volumes:
    - type: bind
      source: ${SINGULARITY:-./singularity}
      target: /var/lib/singularity
    - type: bind
      source: ${REFERENCE:-./reference}
      target: /reference
    - type: bind
      source: ./data
      target: /data
```

In this `docker-compose` file, we specify a single container `bpipe` which
uses our docker image for bpipe. We specify that it needs to be privileged
to run (a requirement for running singularity inside docker). We then specify
several volumes to mount in the container:

* A volume for storing all our singularity images (`.sif` files). This is
  handy to remove the need to pull the images and create the files each time,
  and it allows the image files to be shared across multiple invocations.

* A path for reference files. This is not strictly necessary, but it is
  convenient, since it allows our workflow definitions to assume any reference
  files (such as genome sequences, annotation databases, etc) are in a
  fixed location, even if they are actually in different locations on the
  underlying filesystem. If you know you do not need to create any files
  in this directory (e.g. alignment indexes, etc), you could add the
  property `read_only: true` to make it read only.

* A path for the data we wish to manipulate in our workflow, which in this
  case will include the workflow script itself.

To use it, assume you have a bpipe pipeline in `data/mypipe.groovy`, then
you can run:

```
$ docker-compose run bpipe bash
# pwd
/data
# bpipe run mypipe.groovy SRR13319535_{1,2}.fastq.gz
```
