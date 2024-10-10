# Summary

This repository provided as a working example of additions to
the Adobe OpenType Feature File Specification currently [under
review](https://github.com/adobe-type-tools/feature_file_change_review).

It contains glyph outline sources for the Roman face of Source
Serif 4 Variable (in the form of 9 UFOs) together with a single
set of feature files, a build script, and other related files.

This example is intended to serve multiple purposes:

*  To demonstrate how a variable font can now be built using
   a single feature file hierarchy (rather than one hierarchy
   per-UFO).
*  To provide a context for experimenting with the grammar by
   adding to or changing one or more feature files, building
   the altered font, and examining the result.
*  To introduce a beta version of AFDKO that supports the
   feature file syntax additions.

# Building the sources

The font can be built by following these steps:

1. Clone (or otherwise download) a copy of this repository to a
   directory on a computer with an installation of Python 3.9,
   3.10, 3.11, or 3.12.
2. If the operating system is Windows or MacOS X, you should be
   able to install the needed version of AFDKO (5.0.0b15) using
   `pip` or another Python package installer. For example, you
   could run
   
   `pip install afdko==5.0.0b15`  

   If the operating system is Linux you *may* be able to install
   from the PyPI wheel, but more likely you will need to compile
   AFDKO from source. [Tag 5.0.0b15](https://github.com/adobe-type-tools/afdko/tree/5.0.0b15)
   corresponds to the intended release; more recent updates to
   this version of the code may be available in the [addfeatures](https://github.com/adobe-type-tools/afdko/tree/addfeatures)
   branch. The repository code contains advice on installation.
   We do not recommend attempting to build against Python 3.13
   at this time.
3. Execute the `buildVF.py` build script. It should run without
   any arguments. The optional `-v` argument will make the script
   show the output of the commands it runs. The optional `--hinted`
   argument will use `otfautohint` to add PostScript-style hinting
   to the built font.
4. After a successful build output will be in the `target/VAR`
   subdirectory

# About AFDKO 5.0.0b15

The AFDKO release that can build these sources is fully-featured
but experimental. The current (at the time of writing) 4.0.1 release
of AFDKO is built from C-language sources for separate programs
packaged in a somewhat idiosyncratic way. Virtually all of the code
for the 5.0.0b15 release has been ported to at least low-level C++
and much of it is fully ported to modern, object-based C++. It is
intended to do all of what 4.0.1 does, and passes our automated test
suite, but there may still be bugs. We do not recommend building
fonts intended for publication with the new code until it is included
in a "standard" release.

# Help or Advice

If you have questions about this repository or are having trouble
building from the sources, open the Discussions tab and look to see
if there is relevant help. If not, start a new discussion and we will
try to assist you or provide you with more information.

# Kerning and Mark Data Processing

The files `kern.fea` and `locations.fea` in the `features` directory
were produced by running 

```
bin/kernFeatureWriter.py -s Roman/SourceSerif4Variable-Roman.designspace
```

and the files `mark.fea` and `mkmk.fea` in the same directory were
produced by running

```
bin/markFeatureWriter.py -t -m Roman/SourceSerif4Variable-Roman.designspace
```

It is not necessary to re-run these scripts to build the font.
If you wish to run them the Python packages `fonttools` and
`defcon` must be installed.

The scripts are included to demonstrate how UFO kerning and mark
data from separate UFOs can be translated into variable values in
a feature file.

Note that the location names in `locations.fea` (which is required
by the output of `markFeatureWriter.py`, but only produced by
`kernFeatureWriter.py`) are taken from the `com.adobe.shortInstanceName`
entry in each UFO's `lib.plist` file. If this key is not present the
script will automaticaly generate a (not very clever) location name
for the instance corresponding to that UFO.
