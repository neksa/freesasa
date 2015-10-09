FreeSASA
========

[![DOI](https://zenodo.org/badge/18467/mittinatten/freesasa.svg)]
(https://zenodo.org/badge/latestdoi/18467/mittinatten/freesasa)

C-library for calculating Solvent Accessible Surface Areas.

License: GPLv3 (see file COPYING). Copyright: Simon Mitternacht 2013-2015.

The library includes the algorithms by Lee & Richards and Shrake &
Rupley. Verification has been done by comparing the results of the two
calculations and by visual inspection of the surfaces found by them
(and comparing with analytic results in the two-atom case). For high
resolution versions of the algorithms, the calculations give identical
results.

FreeSASA assigns a radius and a class to each atom. The atomic radii
are by default those defined by [Ooi et al.  PNAS 1987, 84:
3086](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC304812/) (OONS) 
for standard protein atoms, and the van der Waals
radius of the element for other atoms (in for example nucleci
acids). Each atom is also assigned to a class. The default classes are
`polar`, `apolar`, `nucleic acid`, and `unknown`. The program outputs
the total SASA and the area of the four classes.

Users can also provide their own atomic radii and classes, either via
configuration files or via the API. The input format for configuration
files is described in the [online
documentation](http://freesasa.github.io/doxygen/Config-file.html),
and the `share/` directory contains two sample configurations, one for
the NACCESS parameters ([Hubbard & Thornton
1993](http://www.bioinf.manchester.ac.uk/naccess/)) and one for OONS.

The library is still a work in progress, but the calculations have
been verified to give correct results for a large number of
proteins. Therefore, the commandline tool can be considered reliable
and stable. Planned changes mainly include refining the API, adding
more options to the commandline tool and possibly expanding the Python
bindings.

Compiling and installing
------------------------

Can be compiled and installed using the following

    ./configure
    make && make install

The program freesasa provides a command-line interface, the command
`freesasa -h` gives an overview of options. If downloaded from the git
repository the configure-script needs to be set up first using
`autoreconf -i`. Users who don't have access to autotools can download
a tarball that includes the autogenerated scripts from
http://freesasa.github.io/ or from the
[GitHub-releases](https://github.com/mittinatten/freesasa/releases).

Profiling has shown that configuring with 

    ./configure CFLAGS='-ffast-math -funroll-loops -O3' 

increases the speed of the Shrake & Rupley algorithm significantly (10
% or so), as compared to the standard "-O2". There seems to be no
measurable effect on Lee & Richards.

The configuration script can be customized with general options:
* `--enable-python-bindings` builds Python bindings, requires Cython
* `--with-python=<python>` specifies which python binary to use
* `--enable-doxygen` activates building of Doxygen documentation

And some options for developers:
* `--enable-check` enables unit-testing using the Check framework
* `--enable-gcov` adds compiler flags for measuring coverage of tests
    using gcov
* `--enable-flex-bison` rebuild parser/lexer source from
    Flex/Bison sources (the autogenerated code is included in the
    repository, so no need to do this if you are not going to change
    the parser).

Documentation
-------------

Enabling Doxygen builds a [full reference
manual](http://freesasa.github.io/doxygen/), documenting both CLI and
API, also available on the web at http://freesasa.github.io/.

After building the package, calling

    freesasa -h
    
explains how the commandline tool can be used.

Compatibility
-------------

The program has been tested successfully with several versions of GNU
C Compiler and Clang/LLVM. Building the library only requires standard
C and GNU libraries. Developers who want to do testing need to install
the Check unit testing framework. Building the full reference manual
requires Doxygen (version > 1.8.8). Building the Python bindings
requires Cython. These build options, which add extra dependencies,
are disabled by default to simplify installation for users only
interested in the command line tool and and/or C Library.

