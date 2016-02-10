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

The following examples install libraries in the `./test/test_helper`
directory.


## Git submodule

If your project uses Git, the recommended method of installation is via
a [submodule][git-book-submod].

```sh
$ git submodule add https://github.com/ztombol/bats-core test/test_helper
$ git commit -m 'Add bats-core library'
```


## Git clone

If you do not use Git for version control, simply
[clone][git-book-clone] the repository.

```sh
$ git clone https://github.com/ztombol/bats-core test/test_helper/bats-core
```


# Loading

A library is loaded by sourcing the `load.bash` file in its main
directory.

Assuming that libraries are installed in `./test_helper`, adding the
following line to a test file in the current directory loads the
`bats-core` library.

```sh
load 'test_helper/bats-core/load'
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


# Licence

`bats-docs` is licensed under the CC0 licence.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

<!-- BEGIN GENERATED with https://creativecommons.org/choose/zero/ -->

<p xmlns:dct="http://purl.org/dc/terms/" xmlns:vcard="http://www.w3.org/2001/vcard-rdf/3.0#">
  <a rel="license"
     href="http://creativecommons.org/publicdomain/zero/1.0/">
    <img src="http://i.creativecommons.org/p/zero/1.0/88x31.png" style="border-style: none;" alt="CC0" />
  </a>
  <br />
  To the extent possible under law,
  <a rel="dct:publisher"
     href="https://github.com/ztombol">
    <span property="dct:title">Zoltán Tömböl</span></a>
  has waived all copyright and related or neighboring rights to
  <span property="dct:title">bats-docs</span>.
This work is published from:
<span property="vcard:Country" datatype="dct:ISO3166"
      content="HU" about="https://github.com/ztombol">
  Hungary</span>.
</p>

<!-- END GENERATED -->


<!-- REFERENCES -->

[bats]: https://github.com/sstephenson/bats
[bats-pr-110]: https://github.com/sstephenson/bats/pull/110 
[bats-core]: https://github.com/ztombol/bats-core
[bats-assert]: https://github.com/ztombol/bats-assert
[git-book-submod]: https://git-scm.com/book/en/v2/Git-Tools-Submodules
[git-book-clone]: https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository#Cloning-an-Existing-Repository
[bats-load]: https://github.com/sstephenson/bats#load-share-common-code
