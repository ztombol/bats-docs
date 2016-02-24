# Overview

This is a proposal describing how a [Bats][bats] helper library should
look and behave. These guidelines and the libraries following them
resulted from the discussion about [introducing a Standard Library of
Test Helpers for Bats][bats-pr-110].

Topics:
- [Installation](#installation)
- [Loading](#loading)
- [Testing](#testing)

Libraries:
- [`bats-core`][bats-core] - supporting library for test helpers
- [`bats-assert`][bats-assert] - common assertions


# Installation

There are multiple supported installation methods. One may be better
than the others depending on your case. Make sure to install the
required dependencies as well (e.g. install `bats-core` when installing
`bats-assert`).

## npm

If your project is managed by npm, the recommended installation method is
also via npm.

```sh
$ npm install --save-dev https://github.com/ztombol/bats-core
$ npm install --save-dev https://github.com/ztombol/bats-assert
```


## Git submodule

If your project uses Git, the recommended method of installation is via
a [submodule][git-book-submod].

**note:** The following example installs libraries in the
`./test/test_helper` directory.

```sh
$ git submodule add https://github.com/ztombol/bats-core test/test_helper/bats-core
$ git commit -m 'Add bats-core library'
```


## Git clone

If you do not use Git for version control, simply
[clone][git-book-clone] the repository.

**note:** The following example installs libraries in the
`./test/test_helper` directory.

```sh
$ git clone https://github.com/ztombol/bats-core test/test_helper/bats-core
```


# Loading

A library is loaded by sourcing the `load.bash` file in its main
directory.

Assuming that libraries are installed in `test/test_helper`, adding the
following line to a file in the `test` directory loads the
`bats-core` library.

```sh
load 'test_helper/bats-core/load'
```

For npm installations, load the libraries from `node_modules`:

```sh
load '../node_modules/bats-core/load'
load '../node_modules/bats-assert/load'
```

***Note:*** *The [`load` function][bats-load] sources a file (with
`.bash` extension automatically appended) relative to the location of
the current test file.*

If a library depends on other libraries, they must be loaded as well.


## Simple loading helper

You can create a shorthand to simplify library loading.

```sh
# Load a library from the `${BATS_TEST_DIRNAME}/test_helper' directory.
#
# Globals:
#   none
# Arguments:
#   $1 - name of library to load
# Returns:
#   0 - on success
#   1 - otherwise
load_lib() {
  local name="$1"
  load "test_helper/${name}/load"
}
```

This function lets you load a library with only its name, instead of
having to type the entire installation path.

```sh
load_lib bats-core
load_lib bats-assert
```


# Testing

A library's test suite is located in its `test` subdirectory. Library
dependencies are loaded from `$TEST_DEPS_DIR`, which defaults to the
directory containing the library.

For example, testing the `bats-assert` library requires the `bats-core`
to be present in the same directory by default.

```
$ tree -L 1
.
├── bats-assert
└── bats-core

2 directories, 0 files
$ bats bats-assert/test
```

This simplifies testing in most cases, e.g. testing in development and
before running the project's test suite, but also allows easily
selecting dependencies, e.g. testing compatibility with different
versions.


<!-- REFERENCES -->

[bats]: https://github.com/sstephenson/bats
[bats-pr-110]: https://github.com/sstephenson/bats/pull/110 
[bats-core]: https://github.com/ztombol/bats-core
[bats-assert]: https://github.com/ztombol/bats-assert
[git-book-submod]: https://git-scm.com/book/en/v2/Git-Tools-Submodules
[git-book-clone]: https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository#Cloning-an-Existing-Repository
[bats-load]: https://github.com/sstephenson/bats#load-share-common-code
