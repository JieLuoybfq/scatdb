## Synopsis

SCATDB is a lookup table of snow particle scattering properties - scattering, absorption, extinction cross sections, asymmetry parameter and phase functions - for randomly oriented ice particles of various shapes. The properties are all computed using the Discrete Dipole Approximation (DDA).

## Improvements

This version improves upon the initial SCATDB release in several ways.

- In addition to bullet rosettes, sector and dendritic snowflakes and hexagonal columns, this release adds over a thousand aggregate snowflakes.
- More frequencies are considered.
- The library has been rewritten to improve accessibility. It has C and C++ interfaces. The snowflakes may be binned and distribution statistics may be easily obtained...

## Installing

This library is tested on Debian, Fedora and Ubuntu Linux, as well as Microsoft Windows.

Requirements:
--------------

- A C++ 2011-compatible compiler. This includes any recent version of gcc/g++, LLVM/clang or Visual C++ 2015.
- CMake (generates the build scripts and finds library locations)
- Doxygen (generates html documentation; optional, but highly recommended)
- Boost C++ libraries (provides lots of code features)
- Eigen3 libraries (linear algebra library)
- environment-modules (manipulates shell environment variables; optional, but highly recommended)
- HDF5 libraries (storage backend)
- git (needed to checkout the code; required because it provides information to the build scripts)
- ZLIB library (needed for compression of stored data)

On Debian and Ubuntu, the necessary dependencies may be installed using this command:
	sudo apt install cmake doxygen environment-modules libboost-all-dev libeigen3-dev libhdf5-dev hdf5-tools git zlib1g-dev
On Fedora, this command may be used:
	sudo dnf install cmake doxygen environment-modules boost-devel eigen3-devel hdf5-devel hdf5 git zlib-devel

Building:
-------------

- Download (and perhaps extract) the source code package. 
- Create a new build directory. It can be anywhere. Switch into this directory.
- Run cmake to generate the build scripts. e.g.:
	cmake -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX={install path} {Path to source directory}
 - This command, on Linux, typically defaults to generating Makefiles and will use gcc as the compiler. Consult the CMake
   documentation for details regarding how to change the compiler or other settings.
- If cmake is set to generate Makefiles, run:
	make
- If the build is successful, binaries and libraries should be in the ./Debug directory. These can all be copied
to the install directory using:
	sudo make install


## Using

Example programs are in the apps/ subdirectory. SCATDB is primarily written in C++, though it has an extensive C interface. Fortran programs may link to the library using the C interface. C++ header files are denoted with the .hpp extension, and C-only headers use .h.

The applications all should automatically detect the location of the scattering database. By default, it is located in 'scatdb.hdf5', and contains tables for cross sections and phase functions. If the database is not found, then it can be specified by setting the 'scatdb_db' environment variable, or by specifying the '-d {dbfile}' option at the command prompt.

The scatdb_example_cpp application provides several options for reading and subsetting the database. You can subset by frequency, temperature, ice solid sphere-equivalent effective radius, maximum dimension, aspect ratio, and snowflake classification. These options all read a string, and the string can be a comma-separated list (e.g. for flake types, '-y 3,4,5,6' to select small bullet rosettes only) or can be expressed in a range notation (e.g. for frequencies between 13 and 14 GHz: '-f 13/14'). **Because floating-point values are not exact, frequencies and temperatures should always be expressed in range notation to the nearest GHz or degree K**

The selected data can be re-saved at another location, by specifying an output file. HDF5 files are detected by specifying a .hdf5 extension. This file format is recommended because it preserves the phase functions of the particles. Comma-separated-value files can also be written by specifying a .csv extension. These files can export either only 1) the overall single scattering properties or 2) the phase function for a single database record. To re-use the selected data, you can tell the program to load the saved hdf5 file with the -d option.

Once a selection is made, it is possible to calculate the statistics with the --stats option. By combining this option with filtering, it is possible to calculate binned quantites. An example application is binning over particle size bins produced by another instrument. This way, it is possible to efficiently calculate effective radar reflectivity for different PSDs and for different particle populations.

## State of the database

The package supersedes Guosheng Liu's original scatdb release, as well as Holly Nowell's scatdb_ag database.

Flake categories are listed below:
- Ids 0-10 are for the pristine snowflakes of Liu [2004] and Liu [2008]. There are data for 233, 243, 253, 263 and 273 K. Numerous frequencies have been included (3, 5, 9, 10, 13.4, 15, 19, 24.1, 35.6, 50, 60, 70, 80, 85.5, 90, 94, 118, 150, 166, 183, 220 and 340 GHz).
- Ids 20-22 are for bullet rosette aggregates, described in Nowell, Liu and Honeyager [2013] and Honeyager, Liu and Nowell [2016]. Over one thousand aggregates are included. We currently have results only for 263 K, but extrapolation is possible by examining the behavior of the Liu [2004,2008] particles over the appropriate ranges. Ten frequencies are currently available (10.65, 13.6, 18.7, 23.8, 35.6, 36.5, 89, 94, 165.5 and 183.31 GHz).

Flake Category Listing
Id		Description
0		Liu [2004] Long hexagonal column l/d=4
1		Liu [2004] Short hexagonal column l/d=2
2		Liu [2004] Block hexagonal column l/d=1
3		Liu [2004] Thick hexagonal plate l/d=0.2
4		Liu [2004] Thin hexagonal plate l/d=0.05
5		Liu [2008] 3-bullet rosette
6		Liu [2008] 4-bullet rosette
7		Liu [2008] 5-bullet rosette
8		Liu [2008] 6-bullet rosette
9		Liu [2008] sector-like snowflake
10		Liu [2008] dendrite snowflake
20		Nowell, Liu and Honeyager [2013] Rounded
21		Honeyager, Liu and Nowell [2016] Oblate
22		Honeyager, Liu and Nowell [2016] Prolate


## Work in progress / planned work

- Still working on implementing phase function tables in the code.
- Currently re-assimilating the data.
- Adding 220 GHz aggregate snowflake results.
- More work on the C interface is underway. An improved Fortran code is anticipated.
- Support for interpolation / extrapolation.
- Support for sorting
- Support for estimating mean scattering behavior using the Locally-Weighted Scatterplot Smoothing (LOWESS) algorithm.
- Debian / Ubuntu / Fedora / Windows binary packages

## License

The scattering database and the associated code are released under the MIT License.

## Credits

An early implementation of the LOWESS algorithm is provided by Peter Glaus [http://www.cs.man.ac.uk/~glausp/ and https://github.com/BitSeq/BitSeq] (Artistic License 2.0).

The MurmurHash3 algorithm was written by Austin Appleby, who placed it in the public domain [https://github.com/aappleby/smhasher].

The original scatdb and scatdb_ag databases are available at http://cirrus.met.fsu.edu/research/scatdb.html .


## Problems / Suggestions / Contributions

Contact Ryan Honeyager (rhoneyager@fsu.edu).
