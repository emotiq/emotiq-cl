# Emotiq

This source tree constitutes the public Emotiq source repository from
<https://github.com/emotiq/emotiq>.

We intend to do our work transparently and openly in full public view
to this repository.

We assert an MIT license over this source aggregation; see
<https://github.com/emotiq/emotiq/blob/master/LICENSE.txt>.

## Developer installation instructions to test the basic EMOTIQ-TESTS system

This is a predominantly Common Lisp code base.

We aim to work on as many ANSI implementations as possible.

For the development of `testnet` we are targeting the commercial
LispWorks Pro 7.1 implementation but we also test our code with
`sbcl`, `ccl`, and `abcl`.  Currently not all ASDF systems run cleanly
outside of `lwpro-7.1.0-patches-2`.

### Tell ASDF where to find the Emotiq systems 

First, place a copy of this repository somewhere locally on your
machine.  We refer to that location as `~/work/emotiq/` in the
following instructions, so please adjust if you have your local copy
of the repository in a different location.

Configure ASDF to search `~/work/emotiq/` at Lisp boot by copying
<file:etc/emotiq.conf> to
`~/.config/common-lisp/source-registry.conf.d/`, creating the
destination directory if it doesn't already exist.

The following REPL command issued from the top-level source directory
will both create the directory and copy the file on operating systems
running some version of *NIX:

    (uiop:run-program "mkdir -p ~/.config/common-lisp/source-registry.conf.d/ && cp ~/work/emotiq/etc/emotiq.conf ~/.config/common-lisp/source-registry.conf.d/")

    
If you are under Windows or have placed your copy of this source tree
in a different location on the file-system, you will have to perform
the corresponding actions manually as per your local OS conventions.

Once the ASDF configuration has been edited correctly, one should be
able to verify that things are working via:

    (asdf:system-source-directory :emotiq)

### Ensure Quicklisp dependencies have been satisfied

The installation of Quicklisp dependencies only needs to be performed
once per installation.

If Quicklisp is not locally available to your Lisp implementation,
first download and install it via the instructions available at
<https://www.quicklisp.org/beta/>.

After Quicklisp has been installed, then issuing 

    (ql:quickload :emotiq-test)
    
will download all the dependencies needed by the tests gathered into
the `emotiq-test` ASDF system.

We have many ASDF descriptions within this repository whose
dependencies may need to be satisfied by via `ql:quickload` other than
`emotiq-test`.

Currently we are working many systems simultaneously, most noteworthy
among them being the work in the `cosi` system.

If in exploring the code one finds a missing dependency, say for the
system `cosi`, a simple

    (ql:quickload :cosi)

should satisfy the dependencies.  (TODO: `cl:restart` for missing
dependencies).

### Evaluate form to test, which also loads, the system:

    (asdf:test-system :emotiq)

At end you should see a result like

    Unit Test Summary
     | 12 assertions total
     | 12 passed
     | 0 failed
     | 0 execution errors
     | 0 missing tests

The counts of assertions/passed should go up over time, and should
stay equal, with other counts staying zero.


## Building the native libraries required by CRYPTO-PAIRINGS

Currently, we have a dependency on a C library to do our pair based
curve (PBC) cryptography.

The library currently only builds on Linux/MacOS.  It requires a
development tool-chain to be in place, of which dependencies include:

    gcc make g++ flex bison
    
Once these tools are installed so that they may be invoked from a
shell, the script in `etc/build-crypto-pairings.bash` can be used to drive
the build.  The results of that script are created under a `var/`
subdirectory.

As a convenience, loading the ASDF definition for `crypto-pairings`
will attempt to run the script to create the native libraries.  If one
is updating this tree from a previous version, one may explicitly have
to force the asdf `prepare-op` via so:

    (asdf:make :crypto-pairings)


# Test Coverage

All pushed to the source tree result in "Continuous Integration" build
from Travis CI: <https://travis-ci.org/emotiq/emotiq>.

The description of test coverage is contained in the Travis CI
artifact at <https://github.com/emotiq/emotiq/blob/dev/.travis.yml>.

# Colophon
    
    Copyright (c) 2018 Emotiq AG
    Created: 20-FEB-2018
    Revised: <2018-03-28 Wed 11:57Z>
