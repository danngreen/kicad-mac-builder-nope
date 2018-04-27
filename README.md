KiCad Mac Builder
=================

This is the 2017+ KiCad Mac builder and packager.  It does not yet work.  Do not use it to build or package.

It's close, though.

Setup
=====
When I run on MacOS, I need to do the following:

`brew install cmake swig glew glm cairo boost doxygen gettext wget`

Building by hand
================
To get up and running the absolute fastest, just use `build.sh`.  However, it builds everything and uses "reasonable" settings.  If you want something special, for now at least, run `cmake` and `make` by hand.  Better documentation is definitely welcomed, but for now, you can look at `build.sh` for reference.

`build.sh` create all the DMGs.
`build.sh kicad` just builds KiCad, but packages nothing.  This is the same for any other CMake targets.
`build.sh package-kicad-nightly` creates a DMG of "mostly just KiCad" in `build/dmg`.
`build.sh package-extras` creates a DMG of "extras" in `build/dmg`.
`build.sh package-kicad-unified` creates a DMG of "KiCad and the extras" in `build/dmg`.

Building inside a VM
====================
There can be value in building inside a VM.  This can help increase isolation and repeatability, by reducing the chances that something "sticks around" between builds, and helps reduce the chances of undocumented steps.  However, it can be slower and take more resources.

I do this, however, to make sure that people can build using 10.11, 10.12, and 10.13.  To do this, setup a macOS Vagrant machine.  I use https://github.com/timsutton/osx-vm-templates.  Please note, that as of early 2018, to create a 10.13 VM you must start with a 10.12 VM and upgrade it.

There is an example Vagrantfile and scripts in `vagrant/`.

Testing KiCad Patches
=====================
Any patches inside kicad-mac-builder/patches/kicad/ are applied via git-am, per kicad-mac-builder/kicad.cmake.  This helps make it easy to test patches that may affect KiCad macOS packaging.

Issues
======
In early 2018, I'm noticing that sometimes wxPython doesn't download properly from Sourceforge, so I've included a mirror in this repository.

Linting
=======
To prescreen your changes for style issues, install shellcheck and cmakelint and run the following from the same directory as this README:

`find . -path ./build -prune -o -name \*.sh -exec shellcheck {} \;`

`cmakelint --filter=-linelength,-readability/wonkycase kicad-mac-builder/CMakeLists.txt`

`find . -path ./build -prune -o -name \*.cmake -exec cmakelint --filter=-linelength,-readability/wonkycase {} \;`

Making KiCad Mods
=================
When doing some types of work, it can be helpful to have these scripts build KiCad from a location on your computer, rather than the integrated checkout via git.  This can be easily done by removing the 2 GIT_* lines from kicad.cmake, and replace them SOURCE_DIR.

Test Procedure
==============
Before big releases, we should check to make sure all the component pieces work.

Basics
------
* Open up KiCad, and then open up each of the applications like pcbnew and the calculator.
* Open up each of the apps in standalone mode.

Python
------
* Open up pcbnew, and open up the Python scripting console.  Type `import pcbnew` and press enter.  It shouldn't show an error.

Localization
------------
* Open up KiCad, and change the language via Preferences -> Language.  You should see the text in the menubars change.

Help
----
* Open up KiCad, and open up the help documents via Help -> KiCad Manual and Help -> Getting Started in KiCad.  You should see a browser open with the documentation.
* Open up KiCad, and change the languages to something not English via Preferences -> Language.  Then open up the manual via Help -> KiCad Manual.  You should see a browser open with the documentation in the matching language.
