# yq

[![Build Status](https://api.travis-ci.com/mikefarah/yq.svg?branch=master)](https://travis-ci.com/mikefarah/yq/)  ![Docker Pulls](https://img.shields.io/docker/pulls/mikefarah/yq.svg) ![Github Releases (by Release)](https://img.shields.io/github/downloads/mikefarah/yq/total.svg) ![Go Report](https://goreportcard.com/badge/github.com/mikefarah/yq)


a lightweight and portable command-line YAML processor

The aim of the project is to be the [jq](https://github.com/stedolan/jq) or sed of yaml files.

## New version!
V3 is officially out - if you've been using v2 and want/need to upgrade, checkout the [upgrade guide](https://mikefarah.gitbook.io/yq/upgrading-from-v2).

## Install

### Download the latest binary

[Here](https://github.com/mikefarah/yq/releases/latest)

### On MacOS:
```
brew install yq
```
### On Ubuntu and other Linux distros supporting `snap` packages:
```
snap install yq
```

#### Snap notes
`yq` installs with with [_strict confinement_](https://docs.snapcraft.io/snap-confinement/6233) in snap, this means it doesn't have direct access to root files. To read root files you can:

```
sudo cat /etc/myfile | yq r - a.path
```

And to write to a root file you can either use [sponge](https://linux.die.net/man/1/sponge):
```
sudo cat /etc/myfile | yq w - a.path value | sudo sponge /etc/myfile
```
or write to a temporary file:
```
sudo cat /etc/myfile | yq w - a.path value | sudo tee /etc/myfile.tmp
sudo mv /etc/myfile.tmp /etc/myfile
rm /etc/myfile.tmp
```

### On Ubuntu 16.04 or higher from Debian package:
```
sudo add-apt-repository ppa:rmescandon/yq
sudo apt update
sudo apt install yq -y
```
### Go Get:
```
GO111MODULE=on go get github.com/mikefarah/yq/v3
```

## Run with Docker

Oneshot use:

```bash
docker run --rm -v ${PWD}:/workdir mikefarah/yq yq [flags] <command> FILE...
```

Run commands interactively:

```bash
docker run --rm -it -v ${PWD}:/workdir mikefarah/yq sh
```

It can be useful to have a bash function to avoid typing the whole docker command:

```bash
yq() {
  docker run --rm -i -v ${PWD}:/workdir mikefarah/yq yq $@
}
```

## Features
- Written in portable go, so you can download a lovely dependency free binary
- [Deep read a yaml file with a given path expression](https://mikefarah.gitbook.io/yq/commands/read#basic)
- [List matching paths of a given path expression](https://mikefarah.gitbook.io/yq/commands/read#path-only)
- Update a yaml file given a [path expression](https://mikefarah.gitbook.io/yq/commands/write-update#basic) or [script file](https://mikefarah.gitbook.io/yq/commands/write-update#basic)
- Update creates any missing entries in the path on the fly
- Keeps yaml formatting and comments when updating
- [Validate a yaml file](https://mikefarah.gitbook.io/yq/commands/validate)
- Create a yaml file given a [deep path and value](https://mikefarah.gitbook.io/yq/commands/create#creating-a-simple-yaml-file) or a [script file](https://mikefarah.gitbook.io/yq/commands/create#creating-using-a-create-script)
- [Prefix a path to a yaml file](https://mikefarah.gitbook.io/yq/commands/prefix)
- [Convert to/from json to yaml](https://mikefarah.gitbook.io/yq/usage/convert)
- [Pipe data in by using '-'](https://mikefarah.gitbook.io/yq/commands/read#from-stdin)
- [Merge](https://mikefarah.gitbook.io/yq/commands/merge) multiple yaml files with various options for [overriding](https://mikefarah.gitbook.io/yq/commands/merge#overwrite-values) and [appending](https://mikefarah.gitbook.io/yq/commands/merge#append-values-with-arrays)
- Supports multiple documents in a single yaml file for [reading](https://mikefarah.gitbook.io/yq/commands/read#multiple-documents), [writing](https://mikefarah.gitbook.io/yq/commands/write-update#multiple-documents) and [merging](https://mikefarah.gitbook.io/yq/commands/merge#multiple-documents)

## [Usage](https://mikefarah.gitbook.io/yq/)

Check out the [documentation](https://mikefarah.gitbook.io/yq/) for more detailed and advanced usage.

```
Usage:
  yq [flags]
  yq [command]

Available Commands:
  delete      yq d [--inplace/-i] [--doc/-d index] sample.yaml 'b.e(name==fred)'
  help        Help about any command
  merge       yq m [--inplace/-i] [--doc/-d index] [--overwrite/-x] [--append/-a] sample.yaml sample2.yaml
  new         yq n [--script/-s script_file] a.b.c newValue
  prefix      yq p [--inplace/-i] [--doc/-d index] sample.yaml a.b.c
  read        yq r [--printMode/-p pv] sample.yaml 'b.e(name==fr*).value'
  validate    yq v sample.yaml
  write       yq w [--inplace/-i] [--script/-s script_file] [--doc/-d index] sample.yaml 'b.e(name==fr*).value' newValue

Flags:
  -h, --help      help for yq
  -j, --tojson    output as json
  -v, --verbose   verbose mode
  -V, --version   Print version information and quit

Use "yq [command] --help" for more information about a command.
```

## Contribute

**Note: v3 is currently in progress - for the moment I won't be accepting new feature PRs until v3 is ready :)**

1. `scripts/devtools.sh`
2. `make [local] vendor`
3. add unit tests
4. apply changes to go.mod
5. `make [local] build`
7. profit
