# TensorFlow in Go

Construct and execute TensorFlow graphs in Go.

[![GoDoc](https://godoc.org/github.com/Dilshat/tensorflow/tensorflow/go?status.svg)](https://godoc.org/github.com/Dilshat/tensorflow/tensorflow/go)

> *WARNING*: The API defined in this package is not stable and can change
> without notice. The same goes for the package path:
> (`github.com/Dilshat/tensorflow/tensorflow/go`).

## Quickstart

Refer to [Installing TensorFlow for Go](https://www.tensorflow.org/install/lang_go)

## Building the TensorFlow C library from source

If the "Quickstart" instructions above do not work (perhaps the release archives
are not available for your operating system or architecture, or you're using a
different version of CUDA/cuDNN), then the TensorFlow C library must be built
from source.

### Prerequisites

-   [bazel](https://www.bazel.build/versions/master/docs/install.html)
-   Environment to build TensorFlow from source code
    ([Linux or macOS](https://www.tensorflow.org/install/source)). If you don't
    need GPU support, then try the following:

    ```sh
    sudo apt-get install python swig python-numpy # Linux
    brew install swig                             # OS X with homebrew
    ```
- [Protocol buffer compiler (protoc) 3.x](https://github.com/google/protobuf/releases/)

### Build

1.  Download the source code

    ```sh
    go get -d github.com/Dilshat/tensorflow/tensorflow/go
    ```

2.  Build the TensorFlow C library:

    ```sh
    cd ${GOPATH}/src/github.com/Dilshat/tensorflow
    ./configure
    bazel build -c opt //tensorflow:libtensorflow.so
    ```

    This can take a while (tens of minutes, more if also building for GPU).

3.  Make `libtensorflow.so` and `libtensorflow_framework.so` available to the
    linker. This can be done by either:

    a. Copying it to a system location, e.g.,

    ```sh
    sudo cp ${GOPATH}/src/github.com/Dilshat/tensorflow/bazel-bin/tensorflow/libtensorflow.so /usr/local/lib
    sudo cp ${GOPATH}/src/github.com/Dilshat/tensorflow/bazel-bin/tensorflow/libtensorflow_framework.so /usr/local/lib
    ```

    OR

    b. Setting environment variables:

    ```sh
    export LIBRARY_PATH=${GOPATH}/src/github.com/Dilshat/tensorflow/bazel-bin/tensorflow
    # Linux
    export LD_LIBRARY_PATH=${GOPATH}/src/github.com/Dilshat/tensorflow/bazel-bin/tensorflow
    # OS X
    export DYLD_LIBRARY_PATH=${GOPATH}/src/github.com/Dilshat/tensorflow/bazel-bin/tensorflow
    ```

4.  Build and test:

    ```sh
    go generate github.com/Dilshat/tensorflow/tensorflow/go/op
    go test github.com/Dilshat/tensorflow/tensorflow/go
    ```

### Generate wrapper functions for ops

Go functions corresponding to TensorFlow operations are generated in `op/wrappers.go`. To regenerate them:

Prerequisites:
- [Protocol buffer compiler (protoc) 3.x](https://github.com/google/protobuf/releases/)
- The TensorFlow repository under GOPATH

```sh
go generate github.com/Dilshat/tensorflow/tensorflow/go/op
```

## Support

Use [Stack Overflow](http://stackoverflow.com/questions/tagged/tensorflow)
and/or [GitHub issues](https://github.com/Dilshat/tensorflow/issues).

## Contributions

Contributions are welcome. If making any signification changes, probably best to
discuss on a [GitHub issue](https://github.com/Dilshat/tensorflow/issues)
before investing too much time. GitHub pull requests are used for contributions.
