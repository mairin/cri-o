# OCID Integration Tests

Integration tests provide end-to-end testing of OCID.

Note that integration tests do **not** replace unit tests.

As a rule of thumb, code should be tested thoroughly with unit tests.
Integration tests on the other hand are meant to test a specific feature end
to end.

Integration tests are written in *bash* using the
[bats](https://github.com/sstephenson/bats) framework.

## Running integration tests

### Containerized tests

The easiest way to run integration tests is with Docker:
```
$ make integration
```

To run a single test bucket:
```
$ make integration TESTFLAGS="runtimeversion.bats"
```

### On your host

To run the integration tests on your host, you will first need to setup a development environment plus
[bats](https://github.com/sstephenson/bats#installing-bats-from-source)
For example:
```
$ cd ~/go/src/github.com
$ git clone https://github.com/sstephenson/bats.git
$ cd bats
$ ./install.sh /usr/local
```

Then you can run the tests on your host:
```
$ sudo make localintegration
```

To run a single test bucket:
```
$ make localintegration TESTFLAGS="runtimeversion.bats"
```

Or you can just run them directly using bats
```
$ sudo bats test
```

#### Runtime selection
Tests on the host will run with `runc` as the default runtime.
However you can select other OCI compatible runtimes by setting
the `RUNTIME` environment variable.

For example one could use the [Clear Containers](https://github.com/01org/cc-oci-runtime/wiki/Installation)
runtime instead of `runc`:

```
make localintegration RUNTIME=cc-oci-runtime
```

## Writing integration tests

[Helper functions]
(https://github.com/kubernetes-incubator/ocid/blob/master/test/helpers.bash)
are provided in order to facilitate writing tests.

```sh
#!/usr/bin/env bats

# This will load the helpers.
load helpers

# setup is called at the beginning of every test.
function setup() {
}

# teardown is called at the end of every test.
function teardown() {
	cleanup_test
}

@test "ocic runtimeversion" {
	start_ocid
	ocic runtimeversion
	[ "$status" -eq 0 ]
}

```
