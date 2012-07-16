---
layout: post
title: "Downgrade MacPorts Boost from version 1.50.0 to 1.49.0"
date: 2012-07-16 15:27
comments: true
categories: macports osx
---

If you have updated [Boost](http://www.boost.org/) to version 1.50.0 via MacPorts, you may have encountered something like the following:

    $ port installed | grep mongodb
      mongodb @2.0.6_0 (active)
    $ port installed | grep boost
      boost @1.50.0_0 (active)
    $ mongo
    dyld: Symbol not found: __ZNK5boost15program_options16validation_error4whatEv
      Referenced from: /opt/local/bin/mongo
      Expected in: /opt/local/lib/libboost_program_options-mt.dylib
     in /opt/local/bin/mongo

There are numerous reports on the MacPorts Trac with various ports that are affected. Here is the solution that worked for me:

    $ cd /tmp
    $ svn co  -r 93341 'http://svn.macports.org/repository/macports/trunk/dports/devel/boost/'
    A    boost/files
    A    boost/files/patch-boost-foreach.diff
    A    boost/files/patch-thread_visibility.diff
    A    boost/files/patch-libs-mpi-build-Jamfile.v2.diff
    A    boost/files/patch-tools-build-v2-tools-python-2.jam.diff
    A    boost/files/patch-tools-build-v2-tools-python.jam.diff
    A    boost/files/patch-bootstrap.sh.diff
    A    boost/files/patch-tools_build_v2_engine_src_build.jam.diff
    A    boost/files/patch-libs-python-src-converter-builtin_converters.cpp
    A    boost/files/patch-tools_build_v2_engine_src_build.sh.diff
    A    boost/Portfile
    Checked out revision 93341.

    $ cd boost
    $ sudo port install
    --->  Computing dependencies for boost
    --->  Fetching archive for boost
    --->  Attempting to fetch boost-1.49.0_0.darwin_10.x86_64.tbz2 from http://packages.macports.org/boost
    --->  Attempting to fetch boost-1.49.0_0.darwin_10.x86_64.tbz2.rmd160 from http://packages.macports.org/boost
    --->  Installing boost @1.49.0_0
    --->  Deactivating boost @1.50.0_0
    --->  Cleaning boost
    --->  Activating boost @1.49.0_0
    --->  Cleaning boost

    $ sudo port activate boost @1.49.0_0
    --->  Computing dependencies for boost
    --->  Cleaning boost
