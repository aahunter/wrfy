# wrfy ('wharfie')

A grab-bag of useful functionality to keep your docker host tidy.

`wrfy` is a replacement for a number of fairly hinky shell aliases - hopefully a more
robust and useful replacement. Anyone who does local development using docker will at
some point have wondered how to get rid of old containers, old images, and dangling
volumes. `wrfy` has been written to help you out. See the list of functionality below.

# Included functionality

## doctor

`wrfy doctor` will check your docker host for common issues. The checks are:

  - **containers running from an old version of the image they were launched from**. for example, 
    if you were to do `docker run -it alpine:latest /bin/sh`, leave that
	container going, and then pull a newer version of `alpine:latest`, `wrfy`
	will let you know that your Alpine container is running from an old
	image.
  - **dangling volumes**. dangling volumes, which are not attached to a container.
  - **dangling images**. dangling images, which do not have a tag.
  - **stopped containers**. docker hosts can build up a large number of stopped containers
    whose purpose was ephemeral.

Each check suggests a `wrfy` tool to address each particular issue identified.

## kill-all

`wrfy kill-all` will kill all running containers.
It asks for confirmation, unless `--force` is passed as an argument.

## pull-all

`wrfy pull-all` will pull all images present on the docker host. This is very useful when you want to make sure everything is up to date.

## rm-matching

`wrfy rm-matching <pattern>` will remove containers matching the provided glob pattern.
If `-e` is passed, the pattern is interpreted as a regular expression.
It asks for confirmation, unless `--force` is passed as an argument.

A useful command is `wrfy rm-matching -e '^[a-z]+_[a-z]+$'`, which will remove all containers with a name
comprised of two words seperated by an underscore. This will match containers with names automatically
generated by docker.

## rm-stopped

`wrfy rm-stopped` will remove all containers which are not running. It is somewhat of a blunt instrument,
you might want to use `rm-matching` instead.
It asks for confirmation, unless `--force` is passed as an argument.

## rmi-dangling

`wrfy rmi-dangling` will remove all dangling images - images which haven't got a name. 
It asks for confirmation, unless `--force` is passed as an argument.

## rmi-matching

`wrfy rmi-matching <pattern>` will remove images matching the provided glob pattern.
If `-e` is passed, the pattern is interpreted as a regular expression.
It asks for confirmation, unless `--force` is passed as an argument.

A useful command to clean up after `docker-compose` is 
`wrfy rmi-matching -e '^[a-z]+_[a-z]+:latest$'`,
which will remove all containers with a name comprised of two words
seperated by an underscore, and a tag of `latest`. Such images
probably came from `docker-compose`.

## rmv-dangling

`wrfy rmv-dangling` will remove all dangling volumes - volumes not attached to any container.
It asks for confirmation, unless `--force` is passed as an argument.

## scrub

`wrfy scrub` chains together `rm_stopped`, `rmi_dangling`, and `rmv_dangling`.
It asks for confirmation at each stage, unless `--force` is passed as an argument.

