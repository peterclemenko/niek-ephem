# Introduction #
The **niek-ephem** tool is a C++ front end to JPL's DE405 ephemeris.  It includes:

  * A conversion utility to change JPL's ASCII interpolation coefficient files to a single binary file.
  * An interactive text-based interface to the ephemeris information.
  * A library interface to the ephemeris information.
  * A unit testing framework.

The correctness of the implementation has been verified using the JPL Horizons web-based ephemeris system.


# Documentation #
Currently, the documentation for this project is minimal.

Inside the v1.0.0 source release, there is a doc/misc directory.  This contains two interesting files:

  * format.txt - description of how the DE405 ASCII datafiles are organized
  * truth.txt  - description of how to configure the JPL Horizons web-based so that its output will match **niek-ephem**'s results.


# Dependencies #
The following tools and library are needed to fully utilize **niek-ephem**.

  * Doxygen
> > (Javadoc-like tool for extracting source code documentation)
  * CppUnit
> > (A unit testing framework for C++)
  * GNU Make
> > (Project uses advanced features from GNU's make program, including autodependency generation).

The ephemeris interpolation coefficients must be downloaded separately from JPL.  They are available here: [ftp://ssd.jpl.nasa.gov/pub/eph/export/ascii/](ftp://ssd.jpl.nasa.gov/pub/eph/export/ascii/)

# Usage #
Here are the instruction for compiling and using **niek-ephem**:

  1. Open test/DE405EphemerisTest and change the "...JPL\_DE405/de405.data" filename to point to where the generated ephemerides will be read from.
  1. In the top-level directory, type "make".
> > Ignore the warnings saying "Makefile:108: src/DE405Ephemeris.d: No such file or directory".  These files will automatically be generated.
  1. Change to the "./util" directory
  1. Edit the "dataDir" line of the code to point at the directory with the DE405 ephemeris inputs.  (Grab all the files prefixed 'ascp' from [ftp://ssd.jpl.nasa.gov/pub/eph/export/ascii/](ftp://ssd.jpl.nasa.gov/pub/eph/export/ascii/))
> > Note that there are some other options provided, such as omitting planets or limiting the covered date-range.
  1. Compile the "convert.cpp" utility.  E.g. _g++ convert.cpp_
  1. Run the convert utility.  E.g. _./a.out_
  1. If everything goes well, the program will display the conversion results.
```
    ....
Wrote 228 records from /dirs/static/JPL_DE405/ascp2140.405
Wrote 228 records from /dirs/static/JPL_DE405/ascp2160.405
Wrote 229 records from /dirs/static/JPL_DE405/ascp2180.405
Wrote 12 records from /dirs/static/JPL_DE405/ascp2200.405

Total of 6862 records written to de405.data
```
  1. You can now run the tester program in "bin/ephem\_tester"
```
prompt% ./ephem_tester
.............................


OK (29 tests)
```
  1. You can now run the interactive program in "bin/ephem\_demo"
```
prompt% ./ephem_demo ../util/de405.data
Interactive interface for niek-ephem
====================================
Query format: [entity] [julian day]

Entities:
    Mercury
    Venus
    EarthMoonBarycenter
    Mars
    JupiterBarycenter
    SaturnBarycenter
    UranusBarycenter
    NeptuneBarycenter
    PlutoBarycenter
    Moon
    Sun
====================================
---> Sun 2500000
Position: 6.895470152806e+05    -1.830651407889e+05     -1.074005965319e+05
Velocity: 8.575065029780e+02    3.819271522623e+02      1.442392540203e+02

---> 
```

There may still be an instance or two where the location of the ephemeris data is hard coded to /dirs/static.  Just edit the code in question and run "make" again.

Note that the output is in the International Celestial Reference Frame.  Positions are given in kilometers while velocities are in kilometers/day.  The reference plane is the "Earth mean equator and equinox of reference epoch".  The coordinate system origin is the solar system barycenter.