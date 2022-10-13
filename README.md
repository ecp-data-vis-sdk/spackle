# spackle

This is a tool aimed at helping develop Spack for E4S purposes by making it
easy to inject a Spack source tree into an E4S container to perform dedicated
testing.

## Configuration files

It is possible to save a configuration using the `-n -s -c path/to/conf` flags
to write the configuration to a file and just print what would be done (which
helps to verify that any intentions are followed through). Once created,
`spackle -c path/to/conf` will reuse any saved settings from that configuration
file.

## Setup

It is recommended to perform the following in the development Spack source tree so that the volume mounts work as intended:

```console
$ ln -s ../opt
$ ln -s ../../../cache var/spack
```

This will ensure that the in-container mounts are always used within
the container. It also makes it possible to reuse a single source
tree for multiple containers while keeping the actually volatile bits
separated from each other. Be aware that all files written in the
container are probably going to be `root`-owned which leads to
problems when managing files from outside the container.

## Example

As an example configuration, for use with the [E4S
testsuite][e4s-testsuite-repo], the following configuration file
additionally injects the E4S testsuite into the container for ease of
testing:

[e4s-testsuite-repo]: https://github.com/E4S-Project/testsuite

```json
{
    "name": "e4s-testsuite",
    "spack-cache": "/path/to/.cache/spack/spackle",
    "spack-opt": "/path/to/opt",
    "temp": true,
    "x11": true,
    "extra-args": [
        "-v",
        "/path/to/e4s-testsuite:/build/testsuite:Z"
    ]
}
```
