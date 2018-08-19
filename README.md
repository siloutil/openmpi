# openmpi
A copy of the OpenMPI source, version 3.1.1 for compatibility and reference
## Original Readme
Copyright (c) 2004-2007 The Trustees of Indiana University and Indiana
                        University Research and Technology
                        Corporation.  All rights reserved.
Copyright (c) 2004-2007 The University of Tennessee and The University
                        of Tennessee Research Foundation.  All rights
                        reserved.
Copyright (c) 2004-2008 High Performance Computing Center Stuttgart,
                        University of Stuttgart.  All rights reserved.
Copyright (c) 2004-2007 The Regents of the University of California.
                        All rights reserved.
Copyright (c) 2006-2017 Cisco Systems, Inc.  All rights reserved.
Copyright (c) 2006-2011 Mellanox Technologies. All rights reserved.
Copyright (c) 2006-2012 Oracle and/or its affiliates.  All rights reserved.
Copyright (c) 2007      Myricom, Inc.  All rights reserved.
Copyright (c) 2008-2017 IBM Corporation.  All rights reserved.
Copyright (c) 2010      Oak Ridge National Labs.  All rights reserved.
Copyright (c) 2011      University of Houston. All rights reserved.
Copyright (c) 2013-2017 Intel, Inc. All rights reserved.
Copyright (c) 2015      NVIDIA Corporation.  All rights reserved.
Copyright (c) 2017-2018 Los Alamos National Security, LLC.  All rights
                        reserved.
Copyright (c) 2017      Research Organization for Information Science
                        and Technology (RIST). All rights reserved.

$COPYRIGHT$

Additional copyrights may follow

$HEADER$

===========================================================================

When submitting questions and problems, be sure to include as much
extra information as possible.  This web page details all the
information that we request in order to provide assistance:

     http://www.open-mpi.org/community/help/

The best way to report bugs, send comments, or ask questions is to
sign up on the user's and/or developer's mailing list (for user-level
and developer-level questions; when in doubt, send to the user's
list):

        users@lists.open-mpi.org
        devel@lists.open-mpi.org

Because of spam, only subscribers are allowed to post to these lists
(ensure that you subscribe with and post from exactly the same e-mail
address -- joe@example.com is considered different than
joe@mycomputer.example.com!).  Visit these pages to subscribe to the
lists:

     http://lists.open-mpi.org/mailman/listinfo/users
     http://lists.open-mpi.org/mailman/listinfo/devel

Thanks for your time.

===========================================================================

Much, much more information is also available in the Open MPI FAQ:

    https://www.open-mpi.org/faq/

===========================================================================

The following abbreviated list of release notes applies to this code
base as of this writing (March 2017):

General notes
-------------

- Open MPI now includes two public software layers: MPI and OpenSHMEM.
  Throughout this document, references to Open MPI implicitly include
  both of these layers. When distinction between these two layers is
  necessary, we will reference them as the "MPI" and "OpenSHMEM"
  layers respectively.

- OpenSHMEM is a collaborative effort between academia, industry, and
  the U.S. Government to create a specification for a standardized API
  for parallel programming in the Partitioned Global Address Space
  (PGAS).  For more information about the OpenSHMEM project, including
  access to the current OpenSHMEM specification, please visit:

     http://openshmem.org/

  This OpenSHMEM implementation will only work in Linux environments
  with a restricted set of supported networks.

- Open MPI includes support for a wide variety of supplemental
  hardware and software package.  When configuring Open MPI, you may
  need to supply additional flags to the "configure" script in order
  to tell Open MPI where the header files, libraries, and any other
  required files are located.  As such, running "configure" by itself
  may not include support for all the devices (etc.) that you expect,
  especially if their support headers / libraries are installed in
  non-standard locations.  Network interconnects are an easy example
  to discuss -- Libfabric and OpenFabrics networks, for example, both
  have supplemental headers and libraries that must be found before
  Open MPI can build support for them.  You must specify where these
  files are with the appropriate options to configure.  See the
  listing of configure command-line switches, below, for more details.

- The majority of Open MPI's documentation is here in this file, the
  included man pages, and on the web site FAQ
  (https://www.open-mpi.org/).

- Note that Open MPI documentation uses the word "component"
  frequently; the word "plugin" is probably more familiar to most
  users.  As such, end users can probably completely substitute the
  word "plugin" wherever you see "component" in our documentation.
  For what it's worth, we use the word "component" for historical
  reasons, mainly because it is part of our acronyms and internal API
  function calls.

- The run-time systems that are currently supported are:
  - rsh / ssh
  - PBS Pro, Torque
  - Platform LSF (v7.0.2 and later)
  - SLURM
  - Cray XE, XC, and XK
  - Oracle Grid Engine (OGE) 6.1, 6.2 and open source Grid Engine

- Systems that have been tested are:
  - Linux (various flavors/distros), 64 bit (x86), with gcc, Absoft,
    Intel, and Portland (*)
  - macOS (10.12), 64 bit (x85_64) with XCode compilers

  (*) Be sure to read the Compiler Notes, below.

- Other systems have been lightly (but not fully tested):
  - Linux (various flavors/distros), 32 bit, with gcc
  - Cygwin 32 & 64 bit with gcc
  - ARMv6, ARMv7, ARMv8 (aarch64)
  - Other 64 bit platforms (e.g., Linux on PPC64)
  - Oracle Solaris 10 and 11, 32 and 64 bit (SPARC, i386, x86_64),
    with Oracle Solaris Studio 12.5
  - OpenBSD.  Requires configure options --enable-mca-no-build=patcher
    and --disable-slopen with this release.
  - Problems have been reported when building Open MPI on FreeBSD 11.1
    using the clang-4.0 system compiler. A workaround is to build
    Open MPI using the GNU compiler.

Platform Notes
--------------

- N/A


Compiler Notes
--------------

- Open MPI requires a C99-capable compiler to build.

- Mixing compilers from different vendors when building Open MPI
  (e.g., using the C/C++ compiler from one vendor and the Fortran
  compiler from a different vendor) has been successfully employed by
  some Open MPI users (discussed on the Open MPI user's mailing list),
  but such configurations are not tested and not documented.  For
  example, such configurations may require additional compiler /
  linker flags to make Open MPI build properly.

- In general, the latest versions of compilers of a given vendor's
  series have the least bugs.  We have seen cases where Vendor XYZ's
  compiler version A.B fails to compile Open MPI, but version A.C
  (where C>B) works just fine.  If you run into a compile failure, you
  might want to double check that you have the latest bug fixes and
  patches for your compiler.

- Users have reported issues with older versions of the Fortran PGI
  compiler suite when using Open MPI's (non-default) --enable-debug
  configure option.  Per the above advice of using the most recent
  version of a compiler series, the Open MPI team recommends using the
  latest version of the PGI suite, and/or not using the --enable-debug
  configure option.  If it helps, here's what we have found with some
  (not comprehensive) testing of various versions of the PGI compiler
  suite:

    pgi-8 : NO known good version with --enable-debug
    pgi-9 : 9.0-4 known GOOD
    pgi-10: 10.0-0 known GOOD
    pgi-11: NO known good version with --enable-debug
    pgi-12: 12.10 known BAD with -m32, but known GOOD without -m32
            (and 12.8 and 12.9 both known BAD with --enable-debug)
    pgi-13: 13.9 known BAD with -m32, 13.10 known GOOD without -m32
    pgi-15: 15.10 known BAD with -m32

- Similarly, there is a known Fortran PGI compiler issue with long
  source directory path names that was resolved in 9.0-4 (9.0-3 is
  known to be broken in this regard).

- Open MPI does not support the PGI compiler suite on OS X or MacOS.
  See issues below for more details:
  https://github.com/open-mpi/ompi/issues/2604
  https://github.com/open-mpi/ompi/issues/2605

- OpenSHMEM Fortran bindings do not support the `no underscore` Fortran
  symbol convention. IBM's xlf compilers build in that mode by default.
  As such, IBM's xlf compilers cannot build/link the OpenSHMEM Fortran
  bindings by default. A workaround is to pass FC="xlf -qextname" at
  configure time to force a trailing underscore. See the issue below
  for more details:
  https://github.com/open-mpi/ompi/issues/3612

- MPI applications that use the mpi_f08 module on PowerPC platforms
  (tested ppc64le) will likely experience runtime failures if:
   - they are using a GNU linker (ld) version after v2.25.1 and before v2.28,
     -and-
   - they compiled with PGI (tested 17.5) or XL (tested v15.1.5) compilers.
  This was noticed on Ubuntu 16.04 which uses the 2.26.1 version of ld by
  default. However, this issue impacts any OS using a version of ld noted
  above. This GNU linker regression will be fixed in version 2.28.
  Below is a link to the GNU bug on this issue:
    https://sourceware.org/bugzilla/show_bug.cgi?id=21306
  The XL compiler will include a fix for this issue in a future release.

- On NetBSD-6 (at least AMD64 and i386), and possibly on OpenBSD,
  libtool misidentifies properties of f95/g95, leading to obscure
  compile-time failures if used to build Open MPI.  You can work
  around this issue by ensuring that libtool will not use f95/g95
  (e.g., by specifying FC=<some_other_compiler>, or otherwise ensuring
  a different Fortran compiler will be found earlier in the path than
  f95/g95), or by disabling the Fortran MPI bindings with
  --disable-mpi-fortran.

- On OpenBSD/i386, if you configure with
  --enable-mca-no-build=patcher, you will also need to add
  --disable-dlopen.  Otherwise, odd crashes can occur
  nondeterministically.

- Absoft 11.5.2 plus a service pack from September 2012 (which Absoft
  says is available upon request), or a version later than 11.5.2
  (e.g., 11.5.3), is required to compile the Fortran mpi_f08
  module.

- Open MPI does not support the Sparc v8 CPU target.  However,
  as of Solaris Studio 12.1,  and later compilers, one should not
  specify -xarch=v8plus or -xarch=v9.  The use of the options
  -m32 and -m64 for producing 32 and 64 bit targets, respectively,
  are now preferred by the Solaris Studio compilers.  GCC may
  require either "-m32" or "-mcpu=v9 -m32", depending on GCC version.

- It has been noticed that if one uses CXX=sunCC, in which sunCC
  is a link in the Solaris Studio compiler release, that the OMPI
  build system has issue with sunCC and does not build libmpi_cxx.so.
  Therefore  the make install fails.  So we suggest that one should
  use CXX=CC, which works, instead of CXX=sunCC.

- If one tries to build OMPI on Ubuntu with Solaris Studio using the C++
  compiler and the -m32 option, you might see a warning:

    CC: Warning: failed to detect system linker version, falling back to
    custom linker usage

  And the build will fail.  One can overcome this error by either
  setting LD_LIBRARY_PATH to the location of the 32 bit libraries (most
  likely /lib32), or giving LDFLAGS="-L/lib32 -R/lib32" to the configure
  command.  Officially, Solaris Studio is not supported on Ubuntu Linux
  distributions, so additional problems might be incurred.

- Open MPI does not support the gccfss compiler (GCC For SPARC
  Systems; a now-defunct compiler project from Sun).

- At least some versions of the Intel 8.1 compiler seg fault while
  compiling certain Open MPI source code files.  As such, it is not
  supported.

- The Intel 9.0 v20051201 compiler on IA64 platforms seems to have a
  problem with optimizing the ptmalloc2 memory manager component (the
  generated code will segv).  As such, the ptmalloc2 component will
  automatically disable itself if it detects that it is on this
  platform/compiler combination.  The only effect that this should
  have is that the MCA parameter mpi_leave_pinned will be inoperative.

- It has been reported that the Intel 9.1 and 10.0 compilers fail to
  compile Open MPI on IA64 platforms.  As of 12 Sep 2012, there is
  very little (if any) testing performed on IA64 platforms (with any
  compiler).  Support is "best effort" for these platforms, but it is
  doubtful that any effort will be expended to fix the Intel 9.1 /
  10.0 compiler issuers on this platform.

- Early versions of the Intel 12.1 Linux compiler suite on x86_64 seem
  to have a bug that prevents Open MPI from working.  Symptoms
  including immediate segv of the wrapper compilers (e.g., mpicc) and
  MPI applications.  As of 1 Feb 2012, if you upgrade to the latest
  version of the Intel 12.1 Linux compiler suite, the problem will go
  away.

- Early versions of the Portland Group 6.0 compiler have problems
  creating the C++ MPI bindings as a shared library (e.g., v6.0-1).
  Tests with later versions show that this has been fixed (e.g.,
  v6.0-5).

- The Portland Group compilers prior to version 7.0 require the
  "-Msignextend" compiler flag to extend the sign bit when converting
  from a shorter to longer integer.  This is is different than other
  compilers (such as GNU).  When compiling Open MPI with the Portland
  compiler suite, the following flags should be passed to Open MPI's
  configure script:

  shell$ ./configure CFLAGS=-Msignextend CXXFLAGS=-Msignextend \
         --with-wrapper-cflags=-Msignextend \
         --with-wrapper-cxxflags=-Msignextend ...

  This will both compile Open MPI with the proper compile flags and
  also automatically add "-Msignextend" when the C and C++ MPI wrapper
  compilers are used to compile user MPI applications.

- It has been reported that Pathscale 5.0.5 and 6.0.527 compilers
  give an internal compiler error when trying to Open MPI.

- Using the MPI C++ bindings with older versions of the Pathscale
  compiler on some platforms is an old issue that seems to be a
  problem when Pathscale uses a back-end GCC 3.x compiler. Here's a
  proposed solution from the Pathscale support team (from July 2010):

      The proposed work-around is to install gcc-4.x on the system and
      use the pathCC -gnu4 option. Newer versions of the compiler (4.x
      and beyond) should have this fixed, but we'll have to test to
      confirm it's actually fixed and working correctly.

  We don't anticipate that this will be much of a problem for Open MPI
  users these days (our informal testing shows that not many users are
  still using GCC 3.x).  Contact Pathscale support if you continue to
  have problems with Open MPI's C++ bindings.

  Note the MPI C++ bindings have been deprecated by the MPI Forum and
  may not be supported in future releases.

- As of July 2017, the Pathscale compiler suite apparently has no
  further commercial support, and it does not look like there will be
  further releases.  Any issues discovered regarding building /
  running Open MPI with the Pathscale compiler suite therefore may not
  be able to be resolved.

- Using the Absoft compiler to build the MPI Fortran bindings on Suse
  9.3 is known to fail due to a Libtool compatibility issue.

- MPI Fortran API support has been completely overhauled since the
  Open MPI v1.5/v1.6 series.

  ********************************************************************
  ********************************************************************
  *** There is now only a single Fortran MPI wrapper compiler and a
  *** single Fortran OpenSHMEM wrapper compiler: mpifort and oshfort,
  *** respectively.  mpif77 and mpif90 still exist, but they are
  *** symbolic links to mpifort.
  ********************************************************************
  *** Similarly, Open MPI's configure script only recognizes the FC
  *** and FCFLAGS environment variables (to specify the Fortran
  *** compiler and compiler flags, respectively).  The F77 and FFLAGS
  *** environment variables are IGNORED.
  ********************************************************************
  ********************************************************************

  As a direct result, it is STRONGLY recommended that you specify a
  Fortran compiler that uses file suffixes to determine Fortran code
  layout (e.g., free form vs. fixed).  For example, with some versions
  of the IBM XLF compiler, it is preferable to use FC=xlf instead of
  FC=xlf90, because xlf will automatically determine the difference
  between free form and fixed Fortran source code.

  However, many Fortran compilers allow specifying additional
  command-line arguments to indicate which Fortran dialect to use.
  For example, if FC=xlf90, you may need to use "mpifort --qfixed ..."
  to compile fixed format Fortran source files.

  You can use either ompi_info or oshmem_info to see with which Fortran
  compiler Open MPI was configured and compiled.

  There are up to three sets of Fortran MPI bindings that may be
  provided depending on your Fortran compiler):

  - mpif.h: This is the first MPI Fortran interface that was defined
    in MPI-1.  It is a file that is included in Fortran source code.
    Open MPI's mpif.h does not declare any MPI subroutines; they are
    all implicit.

  - mpi module: The mpi module file was added in MPI-2.  It provides
    strong compile-time parameter type checking for MPI subroutines.

  - mpi_f08 module: The mpi_f08 module was added in MPI-3.  It
    provides many advantages over the mpif.h file and mpi module.  For
    example, MPI handles have distinct types (vs. all being integers).
    See the MPI-3 document for more details.

    *** The mpi_f08 module is STRONGLY is recommended for all new MPI
        Fortran subroutines and applications.  Note that the mpi_f08
        module can be used in conjunction with the other two Fortran
        MPI bindings in the same application (only one binding can be
        used per subroutine/function, however).  Full interoperability
        between mpif.h/mpi module and mpi_f08 module MPI handle types
        is provided, allowing mpi_f08 to be used in new subroutines in
        legacy MPI applications.

  Per the OpenSHMEM specification, there is only one Fortran OpenSHMEM
  binding provided:

  - shmem.fh: All Fortran OpenSHMEM programs **should** include
    'shmem.fh', and Fortran OpenSHMEM programs that use constants
    defined by OpenSHMEM **MUST** include 'shmem.fh'.

  The following notes apply to the above-listed Fortran bindings:

  - All Fortran compilers support the mpif.h/shmem.fh-based bindings,
    with one exception: the MPI_SIZEOF interfaces will only be present
    when Open MPI is built with a Fortran compiler that support the
    INTERFACE keyword and ISO_FORTRAN_ENV.  Most notably, this
    excludes the GNU Fortran compiler suite before version 4.9.

  - The level of support provided by the mpi module is based on your
    Fortran compiler.

    If Open MPI is built with a non-GNU Fortran compiler, or if Open
    MPI is built with the GNU Fortran compiler >= v4.9, all MPI
    subroutines will be prototyped in the mpi module.  All calls to
    MPI subroutines will therefore have their parameter types checked
    at compile time.

    If Open MPI is built with an old gfortran (i.e., < v4.9), a
    limited "mpi" module will be built.  Due to the limitations of
    these compilers, and per guidance from the MPI-3 specification,
    all MPI subroutines with "choice" buffers are specifically *not*
    included in the "mpi" module, and their parameters will not be
    checked at compile time.  Specifically, all MPI subroutines with
    no "choice" buffers are prototyped and will receive strong
    parameter type checking at run-time (e.g., MPI_INIT,
    MPI_COMM_RANK, etc.).

    Similar to the mpif.h interface, MPI_SIZEOF is only supported on
    Fortran compilers that support INTERFACE and ISO_FORTRAN_ENV.

  - The mpi_f08 module has been tested with the Intel Fortran compiler
    and gfortran >= 4.9.  Other modern Fortran compilers likely also
    work.

    Many older Fortran compilers do not provide enough modern Fortran
    features to support the mpi_f08 module.  For example, gfortran <
    v4.9 does provide enough support for the mpi_f08 module.

  You can examine the output of the following command to see all
  the Fortran features that are/are not enabled in your Open MPI
  installation:

  shell$ ompi_info | grep -i fort


General Run-Time Support Notes
------------------------------

- The Open MPI installation must be in your PATH on all nodes (and
  potentially LD_LIBRARY_PATH (or DYLD_LIBRARY_PATH), if libmpi/libshmem
  is a shared library), unless using the --prefix or
  --enable-mpirun-prefix-by-default functionality (see below).

- Open MPI's run-time behavior can be customized via MPI Component
  Architecture (MCA) parameters (see below for more information on how
  to get/set MCA parameter values).  Some MCA parameters can be set in
  a way that renders Open MPI inoperable (see notes about MCA
  parameters later in this file).  In particular, some parameters have
  required options that must be included.

  - If specified, the "btl" parameter must include the "self"
    component, or Open MPI will not be able to deliver messages to the
    same rank as the sender.  For example: "mpirun --mca btl tcp,self
    ..."
  - If specified, the "btl_tcp_if_exclude" parameter must include the
    loopback device ("lo" on many Linux platforms), or Open MPI will
    not be able to route MPI messages using the TCP BTL.  For example:
    "mpirun --mca btl_tcp_if_exclude lo,eth1 ..."

- Running on nodes with different endian and/or different datatype
  sizes within a single parallel job is supported in this release.
  However, Open MPI does not resize data when datatypes differ in size
  (for example, sending a 4 byte MPI_DOUBLE and receiving an 8 byte
  MPI_DOUBLE will fail).


MPI Functionality and Features
------------------------------

- All MPI-3 functionality is supported.

- Rank reordering support is available using the TreeMatch library. It
  is activated for the graph and dist_graph topologies.

- When using MPI deprecated functions, some compilers will emit
  warnings.  For example:

  shell$ cat deprecated_example.c
  #include <mpi.h>
  void foo(void) {
      MPI_Datatype type;
      MPI_Type_struct(1, NULL, NULL, NULL, &type);
  }
  shell$ mpicc -c deprecated_example.c
  deprecated_example.c: In function 'foo':
  deprecated_example.c:4: warning: 'MPI_Type_struct' is deprecated (declared at /opt/openmpi/include/mpi.h:1522)
  shell$

- MPI_THREAD_MULTIPLE is supported with some exceptions.  Note that
  Open MPI must be configured with --enable-mpi-thread-multiple to get
  this level of thread safety support.

  The following PMLs support MPI_THREAD_MULTIPLE:
  - cm (see list (1) of supported MTLs, below)
  - ob1 (see list (2) of supported BTLs, below)
  - ucx
  - yalla

  (1) The cm PML and the following MTLs support MPI_THREAD_MULTIPLE:
      - MXM
      - ofi (Libfabric)
      - portals4

  (2) The ob1 PML and the following BTLs support MPI_THREAD_MULTIPLE:
      - openib (see exception below)
      - self
      - sm
      - smcuda
      - tcp
      - ugni
      - usnic
      - vader (shared memory)

  The openib BTL's RDMACM based connection setup mechanism is also not
  thread safe.  The default UDCM method should be used for
  applications requiring MPI_THREAD_MULTIPLE support.

  Currently, MPI File operations are not thread safe even if MPI is
  initialized for MPI_THREAD_MULTIPLE support.

- MPI_REAL16 and MPI_COMPLEX32 are only supported on platforms where a
  portable C datatype can be found that matches the Fortran type
  REAL*16, both in size and bit representation.

- The "libompitrace" library is bundled in Open MPI and is installed
  by default (it can be disabled via the --disable-libompitrace
  flag).  This library provides a simplistic tracing of select MPI
  function calls via the MPI profiling interface.  Linking it in to
  your application via (e.g., via -lompitrace) will automatically
  output to stderr when some MPI functions are invoked:

  shell$ cd examples/
  shell$ mpicc hello_c.c -o hello_c -lompitrace
  shell$ mpirun -np 1 hello_c
  MPI_INIT: argc 1
  Hello, world, I am 0 of 1
  MPI_BARRIER[0]: comm MPI_COMM_WORLD
  MPI_FINALIZE[0]
  shell$

  Keep in mind that the output from the trace library is going to
  stderr, so it may output in a slightly different order than the
  stdout from your application.

  This library is being offered as a "proof of concept" / convenience
  from Open MPI.  If there is interest, it is trivially easy to extend
  it to printf for other MPI functions.  Pull requests on github.com
  would be greatly appreciated.

OpenSHMEM Functionality and Features
------------------------------------

- All OpenSHMEM-1.3 functionality is supported.


MPI Collectives
---------------

- The "fca" coll component: the Mellanox Fabric Collective Accelerator
  (FCA) is a solution for offloading collective operations from the
  MPI process onto Mellanox QDR InfiniBand switch CPUs and HCAs.

- The "cuda" coll component provides CUDA-aware support for the
  reduction type collectives with GPU buffers. This component is only
  compiled into the library when the library has been configured with
  CUDA-aware support.  It intercepts calls to the reduction
  collectives, copies the data to staging buffers if GPU buffers, then
  calls underlying collectives to do the work.

OpenSHMEM Collectives
---------------------

- The "fca" scoll component: the Mellanox Fabric Collective
  Accelerator (FCA) is a solution for offloading collective operations
  from the MPI process onto Mellanox QDR InfiniBand switch CPUs and
  HCAs.

- The "basic" scoll component: Reference implementation of all
  OpenSHMEM collective operations.


Network Support
---------------

- There are four main MPI network models available: "ob1", "cm",
  "yalla", and "ucx".  "ob1" uses BTL ("Byte Transfer Layer")
  components for each supported network.  "cm" uses MTL ("Matching
  Tranport Layer") components for each supported network.  "yalla"
  uses the Mellanox MXM transport.  "ucx" uses the OpenUCX transport.

  - "ob1" supports a variety of networks that can be used in
    combination with each other:

    - OpenFabrics: InfiniBand, iWARP, and RoCE
    - Loopback (send-to-self)
    - Shared memory
    - TCP
    - Intel Phi SCIF
    - SMCUDA
    - Cisco usNIC
    - uGNI (Cray Gemini, Aries)
    - vader (XPMEM, Linux CMA, Linux KNEM, and copy-in/copy-out shared
      memory)

  - "cm" supports a smaller number of networks (and they cannot be
    used together), but may provide better overall MPI performance:

    - Intel Omni-Path PSM2
    - Intel True Scale PSM (QLogic InfiniPath)
    - OpenFabrics Interfaces ("libfabric" tag matching)
    - Portals 4

    Open MPI will, by default, choose to use "cm" when one of the
    above transports can be used, unless OpenUCX or MXM support is
    detected, in which case the "ucx" or "yalla" PML will be used
    by default.  Otherwise, "ob1" will be used and the corresponding
    BTLs will be selected. Users can force the use of ob1 or cm if
    desired by setting the "pml" MCA parameter at run-time:

      shell$ mpirun --mca pml ob1 ...
      or
      shell$ mpirun --mca pml cm ...

- Similarly, there are two OpenSHMEM network models available: "ucx",
  and "ikrit":
  - "ucx" interfaces directly with UCX;
  - "ikrit" interfaces directly with Mellanox MXM.

- UCX is the Unified Communication X (UCX) communication library
  (http://www.openucx.org/).
  This is an open-source project developed in collaboration between
  industry, laboratories, and academia to create an open-source
  production grade communication framework for data centric and
  high-performance applications.
  UCX currently supports:
  - OFA Verbs;
  - Cray's uGNI;
  - NVIDIA CUDA drivers.

- MXM is the Mellanox Messaging Accelerator library utilizing a full
  range of IB transports to provide the following messaging services
  to the upper level MPI/OpenSHMEM libraries:

  - Usage of all available IB transports
  - Native RDMA support
  - Progress thread
  - Shared memory communication
  - Hardware-assisted reliability

- The usnic BTL is support for Cisco's usNIC device ("userspace NIC")
  on Cisco UCS servers with the Virtualized Interface Card (VIC).
  Although the usNIC is accessed via the OpenFabrics Libfabric API
  stack, this BTL is specific to Cisco usNIC devices.

- uGNI is a Cray library for communicating over the Gemini and Aries
  interconnects.

- The OpenFabrics Enterprise Distribution (OFED) software package v1.0
  will not work properly with Open MPI v1.2 (and later) due to how its
  Mellanox InfiniBand plugin driver is created.  The problem is fixed
  OFED v1.1 (and later).

- Better memory management support is available for OFED-based
  transports using the "ummunotify" Linux kernel module.  OFED memory
  managers are necessary for better bandwidth when re-using the same
  buffers for large messages (e.g., benchmarks and some applications).

  Unfortunately, the ummunotify module was not accepted by the Linux
  kernel community (and is still not distributed by OFED).  But it
  still remains the best memory management solution for MPI
  applications that used the OFED network transports.  If Open MPI is
  able to find the <linux/ummunotify.h> header file, it will build
  support for ummunotify and include it by default.  If MPI processes
  then find the ummunotify kernel module loaded and active, then their
  memory managers (which have been shown to be problematic in some
  cases) will be disabled and ummunotify will be used.  Otherwise, the
  same memory managers from prior versions of Open MPI will be used.
  The ummunotify Linux kernel module can be downloaded from:

    http://lwn.net/Articles/343351/

- The use of fork() with OpenFabrics-based networks (i.e., the openib
  BTL) is only partially supported, and only on Linux kernels >=
  v2.6.15 with libibverbs v1.1 or later (first released as part of
  OFED v1.2), per restrictions imposed by the OFED network stack.

- Linux "knem" support is used when the "vader" or "sm" (shared
  memory) BTLs are compiled with knem support (see the --with-knem
  configure option) and the knem Linux module is loaded in the running
  kernel.  If the knem Linux kernel module is not loaded, the knem
  support is (by default) silently deactivated during Open MPI jobs.

  See http://runtime.bordeaux.inria.fr/knem/ for details on Knem.

- Linux Cross-Memory Attach (CMA) or XPMEM is used by the vader
  shared-memory BTL when the CMA/XPMEM libraries are installedm,
  respectively.  Linux CMA and XPMEM are similar (but different)
  mechanisms for Open MPI to utilize single-copy semantics for shared
  memory.

Open MPI Extensions
-------------------

- An MPI "extensions" framework is included in Open MPI, but is not
  enabled by default.  See the "Open MPI API Extensions" section below
  for more information on compiling and using MPI extensions.

- The following extensions are included in this version of Open MPI:

  - affinity: Provides the OMPI_Affinity_str() routine on retrieving
    a string that contains what resources a process is bound to.  See
    its man page for more details.
  - cr: Provides routines to access to checkpoint restart routines.
    See ompi/mpiext/cr/mpiext_cr_c.h for a listing of available
    functions.
  - cuda: When the library is compiled with CUDA-aware support, it
    provides two things.  First, a macro
    MPIX_CUDA_AWARE_SUPPORT. Secondly, the function
    MPIX_Query_cuda_support that can be used to query for support.
  - example: A non-functional extension; its only purpose is to
    provide an example for how to create other extensions.

===========================================================================

Building Open MPI
-----------------

If you have checked out a DEVELOPER'S COPY of Open MPI (i.e., you
cloned from Git), you really need to read the HACKING file before
attempting to build Open MPI. Really.

If you have downloaded a tarball, then things are much simpler.
Open MPI uses a traditional configure script paired with "make" to
build.  Typical installs can be of the pattern:

shell$ ./configure [...options...]
shell$ make [-j N] all install
    (use an integer value of N for parallel builds)

There are many available configure options (see "./configure --help"
for a full list); a summary of the more commonly used ones is included
below.

Note that for many of Open MPI's --with-<foo> options, Open MPI will,
by default, search for header files and/or libraries for <foo>.  If
the relevant files are found, Open MPI will built support for <foo>;
if they are not found, Open MPI will skip building support for <foo>.
However, if you specify --with-<foo> on the configure command line and
Open MPI is unable to find relevant support for <foo>, configure will
assume that it was unable to provide a feature that was specifically
requested and will abort so that a human can resolve out the issue.

INSTALLATION OPTIONS

--prefix=<directory>
  Install Open MPI into the base directory named <directory>.  Hence,
  Open MPI will place its executables in <directory>/bin, its header
  files in <directory>/include, its libraries in <directory>/lib, etc.

--disable-shared
  By default, Open MPI and OpenSHMEM build shared libraries, and all
  components are built as dynamic shared objects (DSOs). This switch
  disables this default; it is really only useful when used with
  --enable-static.  Specifically, this option does *not* imply
  --enable-static; enabling static libraries and disabling shared
  libraries are two independent options.

--enable-static
  Build MPI and OpenSHMEM as static libraries, and statically link in
  all components.  Note that this option does *not* imply
  --disable-shared; enabling static libraries and disabling shared
  libraries are two independent options.

  Be sure to read the description of --without-memory-manager, below;
  it may have some effect on --enable-static.

--disable-wrapper-rpath
  By default, the wrapper compilers (e.g., mpicc) will enable "rpath"
  support in generated executables on systems that support it.  That
  is, they will include a file reference to the location of Open MPI's
  libraries in the application executable itself.  This means that
  the user does not have to set LD_LIBRARY_PATH to find Open MPI's
  libraries (e.g., if they are installed in a location that the
  run-time linker does not search by default).

  On systems that utilize the GNU ld linker, recent enough versions
  will actually utilize "runpath" functionality, not "rpath".  There
  is an important difference between the two:

  "rpath": the location of the Open MPI libraries is hard-coded into
      the MPI/OpenSHMEM application and cannot be overridden at
      run-time.
  "runpath": the location of the Open MPI libraries is hard-coded into
      the MPI/OpenSHMEM application, but can be overridden at run-time
      by setting the LD_LIBRARY_PATH environment variable.

  For example, consider that you install Open MPI vA.B.0 and
  compile/link your MPI/OpenSHMEM application against it.  Later, you
  install Open MPI vA.B.1 to a different installation prefix (e.g.,
  /opt/openmpi/A.B.1 vs. /opt/openmpi/A.B.0), and you leave the old
  installation intact.

  In the rpath case, your MPI application will always use the
  libraries from your A.B.0 installation.  In the runpath case, you
  can set the LD_LIBRARY_PATH environment variable to point to the
  A.B.1 installation, and then your MPI application will use those
  libraries.

  Note that in both cases, however, if you remove the original A.B.0
  installation and set LD_LIBRARY_PATH to point to the A.B.1
  installation, your application will use the A.B.1 libraries.

  This rpath/runpath behavior can be disabled via
  --disable-wrapper-rpath.

  If you would like to keep the rpath option, but not enable runpath
  a different configure option is avalabile
  --disable-wrapper-runpath.

--enable-dlopen
  Build all of Open MPI's components as standalone Dynamic Shared
  Objects (DSO's) that are loaded at run-time (this is the default).
  The opposite of this option, --disable-dlopen, causes two things:

  1. All of Open MPI's components will be built as part of Open MPI's
     normal libraries (e.g., libmpi).
  2. Open MPI will not attempt to open any DSO's at run-time.

  Note that this option does *not* imply that OMPI's libraries will be
  built as static objects (e.g., libmpi.a).  It only specifies the
  location of OMPI's components: standalone DSOs or folded into the
  Open MPI libraries.  You can control whether Open MPI's libraries
  are build as static or dynamic via --enable|disable-static and
  --enable|disable-shared.

--disable-show-load-errors-by-default
  Set the default value of the mca_base_component_show_load_errors MCA
  variable: the --enable form of this option sets the MCA variable to
  true, the --disable form sets the MCA variable to false.  The MCA
  mca_base_component_show_load_errors variable can still be overridden
  at run time via the usual MCA-variable-setting mechanisms; this
  configure option simply sets the default value.

  The --disable form of this option is intended for Open MPI packagers
  who tend to enable support for many different types of networks and
  systems in their packages.  For example, consider a packager who
  includes support for both the FOO and BAR networks in their Open MPI
  package, both of which require support libraries (libFOO.so and
  libBAR.so).  If an end user only has BAR hardware, they likely only
  have libBAR.so available on their systems -- not libFOO.so.
  Disabling load errors by default will prevent the user from seeing
  potentially confusing warnings about the FOO components failing to
  load because libFOO.so is not available on their systems.

  Conversely, system administrators tend to build an Open MPI that is
  targeted at their specific environment, and contains few (if any)
  components that are not needed.  In such cases, they might want
  their users to be warned that the FOO network components failed to
  load (e.g., if libFOO.so was mistakenly unavailable), because Open
  MPI may otherwise silently failover to a slower network path for MPI
  traffic.

--with-platform=FILE
  Load configure options for the build from FILE.  Options on the
  command line that are not in FILE are also used.  Options on the
  command line and in FILE are replaced by what is in FILE.

--with-libmpi-name=STRING
  Replace libmpi.* and libmpi_FOO.* (where FOO is one of the fortran
  supporting libraries installed in lib) with libSTRING.* and
  libSTRING_FOO.*. This is provided as a convenience mechanism for
  third-party packagers of Open MPI that might want to rename these
  libraries for their own purposes. This option is *not* intended for
  typical users of Open MPI.

--enable-mca-no-build=LIST
  Comma-separated list of <type>-<component> pairs that will not be
  built. For example, "--enable-mca-no-build=btl-portals,oob-ud" will
  disable building the portals BTL and the ud OOB component.

NETWORKING SUPPORT / OPTIONS

--with-fca=<directory>
  Specify the directory where the Mellanox FCA library and
  header files are located.

  FCA is the support library for Mellanox switches and HCAs.

--with-hcoll=<directory>
  Specify the directory where the Mellanox hcoll library and header
  files are located.  This option is generally only necessary if the
  hcoll headers and libraries are not in default compiler/linker
  search paths.

  hcoll is the support library for MPI collective operation offload on
  Mellanox ConnectX-3 HCAs (and later).

--with-knem=<directory>
  Specify the directory where the knem libraries and header files are
  located.  This option is generally only necessary if the knem headers
  and libraries are not in default compiler/linker search paths.

  knem is a Linux kernel module that allows direct process-to-process
  memory copies (optionally using hardware offload), potentially
  increasing bandwidth for large messages sent between messages on the
  same server.  See http://runtime.bordeaux.inria.fr/knem/ for
  details.

--with-libfabric=<directory>
  Specify the directory where the OpenFabrics Interfaces libfabric
  library and header files are located.  This option is generally only
  necessary if the libfabric headers and libraries are not in default
  compiler/linker search paths.

  Libfabric is the support library for OpenFabrics Interfaces-based
  network adapters, such as Cisco usNIC, Intel True Scale PSM, Cray
  uGNI, etc.

--with-libfabric-libdir=<directory>
  Look in directory for the libfabric libraries.  By default, Open MPI
  will look in <libfabric directory>/lib and <libfabric
  directory>/lib64, which covers most cases.  This option is only
  needed for special configurations.

--with-mxm=<directory>
  Specify the directory where the Mellanox MXM library and header
  files are located.  This option is generally only necessary if the
  MXM headers and libraries are not in default compiler/linker search
  paths.

  MXM is the support library for Mellanox Network adapters.

--with-mxm-libdir=<directory>
  Look in directory for the MXM libraries.  By default, Open MPI will
  look in <mxm directory>/lib and <mxm directory>/lib64, which covers
  most cases.  This option is only needed for special configurations.

--with-portals4=<directory>
  Specify the directory where the Portals4 libraries and header files
  are located.  This option is generally only necessary if the Portals4
  headers and libraries are not in default compiler/linker search
  paths.

  Portals is a low-level network API for high-performance networking
  on high-performance computing systems developed by Sandia National
  Laboratories, Intel Corporation, and the University of New Mexico.
  The Portals 4 Reference Implementation is a complete implementation
  of Portals 4, with transport over InfiniBand verbs and UDP.

--with-portals4-libdir=<directory>
  Location of libraries to link with for Portals4 support.

--with-portals4-max-md-size=SIZE
--with-portals4-max-va-size=SIZE
  Set configuration values for Portals 4

--with-psm=<directory>
  Specify the directory where the QLogic InfiniPath / Intel True Scale
  PSM library and header files are located.  This option is generally
  only necessary if the PSM headers and libraries are not in default
  compiler/linker search paths.

  PSM is the support library for QLogic InfiniPath and Intel TrueScale
  network adapters.

--with-psm-libdir=<directory>
  Look in directory for the PSM libraries.  By default, Open MPI will
  look in <psm directory>/lib and <psm directory>/lib64, which covers
  most cases.  This option is only needed for special configurations.

--with-psm2=<directory>
  Specify the directory where the Intel Omni-Path PSM2 library and
  header files are located.  This option is generally only necessary
  if the PSM2 headers and libraries are not in default compiler/linker
  search paths.

  PSM is the support library for Intel Omni-Path network adapters.

--with-psm2-libdir=<directory>
  Look in directory for the PSM2 libraries.  By default, Open MPI will
  look in <psm2 directory>/lib and <psm2 directory>/lib64, which
  covers most cases.  This option is only needed for special
  configurations.

--with-scif=<dir>
  Look in directory for Intel SCIF support libraries

--with-verbs=<directory>
  Specify the directory where the verbs (also known as OpenFabrics
  verbs, or Linux verbs, and previously known as OpenIB) libraries and
  header files are located.  This option is generally only necessary
  if the verbs headers and libraries are not in default
  compiler/linker search paths.

  The Verbs library usually implies operating system bypass networks,
  such as InfiniBand, usNIC, iWARP, and RoCE (aka "IBoIP").

--with-verbs-libdir=<directory>
  Look in directory for the verbs libraries.  By default, Open MPI
  will look in <verbs_directory>/lib and <verbs_ directory>/lib64,
  which covers most cases.  This option is only needed for special
  configurations.

--with-verbs-usnic
  This option will activate support in Open MPI for disabling a
  dire-sounding warning message from libibverbs that Cisco usNIC
  devices are not supported (because Cisco usNIC devices are supported
  through libfabric, not libibverbs).  This libibverbs warning can
  also be suppressed by installing the "no op" libusnic_verbs plugin
  for libibverbs (see https://github.com/cisco/libusnic_verbs, or
  download binaries from cisco.com).  This option is disabled by
  default because it causes libopen-pal.so to depend on libibverbs.so,
  which is undesirable to many downstream packagers.

--with-usnic
  Abort configure if Cisco usNIC support cannot be built.

RUN-TIME SYSTEM SUPPORT

--enable-mpirun-prefix-by-default
  This option forces the "mpirun" command to always behave as if
  "--prefix $prefix" was present on the command line (where $prefix is
  the value given to the --prefix option to configure).  This prevents
  most rsh/ssh-based users from needing to modify their shell startup
  files to set the PATH and/or LD_LIBRARY_PATH for Open MPI on remote
  nodes.  Note, however, that such users may still desire to set PATH
  -- perhaps even in their shell startup files -- so that executables
  such as mpicc and mpirun can be found without needing to type long
  path names.  --enable-orterun-prefix-by-default is a synonym for
  this option.

--enable-orte-static-ports
   Enable orte static ports for tcp oob (default: enabled).

--with-alps
  Force the building of for the Cray Alps run-time environment.  If
  Alps support cannot be found, configure will abort.

--with-lsf=<directory>
  Specify the directory where the LSF libraries and header files are
  located.  This option is generally only necessary if the LSF headers
  and libraries are not in default compiler/linker search paths.

  LSF is a resource manager system, frequently used as a batch
  scheduler in HPC systems.

  NOTE: If you are using LSF version 7.0.5, you will need to add
        "LIBS=-ldl" to the configure command line.  For example:

            ./configure LIBS=-ldl --with-lsf ...

        This workaround should *only* be needed for LSF 7.0.5.

--with-lsf-libdir=<directory>
  Look in directory for the LSF libraries.  By default, Open MPI will
  look in <lsf directory>/lib and <lsf directory>/lib64, which covers
  most cases.  This option is only needed for special configurations.

--with-pmi
  Build PMI support (by default on non-Cray XE/XC systems, it is not
  built).  On Cray XE/XC systems, the location of pmi is detected
  automatically as part of the configure process.  For non-Cray
  systems, if the pmi2.h header is found in addition to pmi.h, then
  support for PMI2 will be built.

--with-slurm
  Force the building of SLURM scheduler support.

--with-sge
  Specify to build support for the Oracle Grid Engine (OGE) resource
  manager and/or the Open Grid Engine.  OGE support is disabled by
  default; this option must be specified to build OMPI's OGE support.

  The Oracle Grid Engine (OGE) and open Grid Engine packages are
  resource manager systems, frequently used as a batch scheduler in
  HPC systems.

--with-tm=<directory>
  Specify the directory where the TM libraries and header files are
  located.  This option is generally only necessary if the TM headers
  and libraries are not in default compiler/linker search paths.

  TM is the support library for the Torque and PBS Pro resource
  manager systems, both of which are frequently used as a batch
  scheduler in HPC systems.

MISCELLANEOUS SUPPORT LIBRARIES

--with-blcr=<directory>
  Specify the directory where the Berkeley Labs Checkpoint / Restart
  (BLCR) libraries and header files are located.  This option is
  generally only necessary if the BLCR headers and libraries are not
  in default compiler/linker search paths.

  This option is only meaningful if the --with-ft option is also used
  to active Open MPI's fault tolerance behavior.

--with-blcr-libdir=<directory>
  Look in directory for the BLCR libraries.  By default, Open MPI will
  look in <blcr directory>/lib and <blcr directory>/lib64, which
  covers most cases.  This option is only needed for special
  configurations.

--with-dmtcp=<directory>
  Specify the directory where the Distributed MultiThreaded
  Checkpointing (DMTCP) libraries and header files are located.  This
  option is generally only necessary if the DMTCP headers and
  libraries are not in default compiler/linker search paths.

  This option is only meaningful if the --with-ft option is also used
  to active Open MPI's fault tolerance behavior.

--with-dmtcp-libdir=<directory>
  Look in directory for the DMTCP libraries.  By default, Open MPI
  will look in <dmtcp directory>/lib and <dmtcp directory>/lib64,
  which covers most cases.  This option is only needed for special
  configurations.

--with-libevent(=value)
  This option specifies where to find the libevent support headers and
  library.  The following VALUEs are permitted:

    internal:    Use Open MPI's internal copy of libevent.
    external:    Use an external libevent installation (rely on default
                 compiler and linker paths to find it)
    <no value>:  Same as "internal".
    <directory>: Specify the location of a specific libevent
                 installation to use

  By default (or if --with-libevent is specified with no VALUE), Open
  MPI will build and use the copy of libevent that it has in its
  source tree.  However, if the VALUE is "external", Open MPI will
  look for the relevant libevent header file and library in default
  compiler / linker locations.  Or, VALUE can be a directory tree
  where the libevent header file and library can be found.  This
  option allows operating systems to include Open MPI and use their
  default libevent installation instead of Open MPI's bundled libevent.

  libevent is a support library that provides event-based processing,
  timers, and signal handlers.  Open MPI requires libevent to build;
  passing --without-libevent will cause configure to abort.

--with-libevent-libdir=<directory>
  Look in directory for the libevent libraries.  This option is only
  usable when building Open MPI against an external libevent
  installation.  Just like other --with-FOO-libdir configure options,
  this option is only needed for special configurations.

--with-hwloc(=value)
  Build hwloc support (default: enabled).  This option specifies where
  to find the hwloc support headers and library.  The following values
  are permitted:

    internal:    Use Open MPI's internal copy of hwloc.
    external:    Use an external hwloc installation (rely on default
                 compiler and linker paths to find it)
    <no value>:  Same as "internal".
    <directory>: Specify the location of a specific hwloc
                 installation to use

  By default (or if --with-hwloc is specified with no VALUE), Open MPI
  will build and use the copy of hwloc that it has in its source tree.
  However, if the VALUE is "external", Open MPI will look for the
  relevant hwloc header files and library in default compiler / linker
  locations.  Or, VALUE can be a directory tree where the hwloc header
  file and library can be found.  This option allows operating systems
  to include Open MPI and use their default hwloc installation instead
  of Open MPI's bundled hwloc.

  hwloc is a support library that provides processor and memory
  affinity information for NUMA platforms.

--with-hwloc-libdir=<directory>
  Look in directory for the hwloc libraries.  This option is only
  usable when building Open MPI against an external hwloc
  installation.  Just like other --with-FOO-libdir configure options,
  this option is only needed for special configurations.

--disable-hwloc-pci
  Disable building hwloc's PCI device-sensing capabilities.  On some
  platforms (e.g., SusE 10 SP1, x86-64), the libpci support library is
  broken.  Open MPI's configure script should usually detect when
  libpci is not usable due to such brokenness and turn off PCI
  support, but there may be cases when configure mistakenly enables
  PCI support in the presence of a broken libpci.  These cases may
  result in "make" failing with warnings about relocation symbols in
  libpci.  The --disable-hwloc-pci switch can be used to force Open
  MPI to not build hwloc's PCI device-sensing capabilities in these
  cases.

  Similarly, if Open MPI incorrectly decides that libpci is broken,
  you can force Open MPI to build hwloc's PCI device-sensing
  capabilities by using --enable-hwloc-pci.

  hwloc can discover PCI devices and locality, which can be useful for
  Open MPI in assigning message passing resources to MPI processes.

--with-libltdl=<directory>
  Specify the directory where the GNU Libtool libltdl libraries and
  header files are located.  This option is generally only necessary
  if the libltdl headers and libraries are not in default
  compiler/linker search paths.

  Note that this option is ignored if --disable-dlopen is specified.

--disable-libompitrace
  Disable building the simple "libompitrace" library (see note above
  about libompitrace)

--with-valgrind(=<directory>)
  Directory where the valgrind software is installed.  If Open MPI
  finds Valgrind's header files, it will include additional support
  for Valgrind's memory-checking debugger.

  Specifically, it will eliminate a lot of false positives from
  running Valgrind on MPI applications.  There is a minor performance
  penalty for enabling this option.

MPI FUNCTIONALITY

--with-mpi-param-check(=value)
  Whether or not to check MPI function parameters for errors at
  runtime.  The following values are permitted:

    always:  MPI function parameters are always checked for errors
    never:   MPI function parameters are never checked for errors
    runtime: Whether MPI function parameters are checked depends on
             the value of the MCA parameter mpi_param_check (default:
             yes).
    yes:     Synonym for "always" (same as --with-mpi-param-check).
    no:      Synonym for "none" (same as --without-mpi-param-check).

  If --with-mpi-param is not specified, "runtime" is the default.

--disable-mpi-thread-multiple
  Disable the MPI thread level MPI_THREAD_MULTIPLE (it is enabled by
  default).

--enable-mpi-cxx
  Enable building the C++ MPI bindings (default: disabled).

  The MPI C++ bindings were deprecated in MPI-2.2, and removed from
  the MPI standard in MPI-3.0.

--enable-mpi-java
  Enable building of an EXPERIMENTAL Java MPI interface (disabled by
  default).  You may also need to specify --with-jdk-dir,
  --with-jdk-bindir, and/or --with-jdk-headers.  See README.JAVA.txt
  for details.

  Note that this Java interface is INCOMPLETE (meaning: it does not
  support all MPI functionality) and LIKELY TO CHANGE.  The Open MPI
  developers would very much like to hear your feedback about this
  interface.  See README.JAVA.txt for more details.

--enable-mpi-fortran(=value)
  By default, Open MPI will attempt to build all 3 Fortran bindings:
  mpif.h, the "mpi" module, and the "mpi_f08" module.  The following
  values are permitted:

    all:        Synonym for "yes".
    yes:        Attempt to build all 3 Fortran bindings; skip
                any binding that cannot be built (same as
                --enable-mpi-fortran).
    mpifh:      Build mpif.h support.
    usempi:     Build mpif.h and "mpi" module support.
    usempif08:  Build mpif.h, "mpi" module, and "mpi_f08"
                module support.
    none:       Synonym for "no".
    no:         Do not build any MPI Fortran support (same as
                --disable-mpi-fortran).  This is mutually exclusive
                with building the OpenSHMEM Fortran interface.

--enable-mpi-ext(=<list>)
  Enable Open MPI's non-portable API extensions.  If no <list> is
  specified, all of the extensions are enabled.

  See "Open MPI API Extensions", below, for more details.

--disable-mpi-io
  Disable built-in support for MPI-2 I/O, likely because an
  externally-provided MPI I/O package will be used. Default is to use
  the internal framework system that uses the ompio component and a
  specially modified version of ROMIO that fits inside the romio
  component

--disable-io-romio
  Disable the ROMIO MPI-IO component

--with-io-romio-flags=flags
  Pass flags to the ROMIO distribution configuration script.  This
  option is usually only necessary to pass
  parallel-filesystem-specific preprocessor/compiler/linker flags back
  to the ROMIO system.

--disable-io-ompio
  Disable the ompio MPI-IO component

--enable-sparse-groups
  Enable the usage of sparse groups. This would save memory
  significantly especially if you are creating large
  communicators. (Disabled by default)

OPENSHMEM FUNCTIONALITY

--disable-oshmem
  Disable building the OpenSHMEM implementation (by default, it is
  enabled).

--disable-oshmem-fortran
  Disable building only the Fortran OpenSHMEM bindings. Please see
  the "Compiler Notes" section herein which contains further
  details on known issues with various Fortran compilers.

MISCELLANEOUS FUNCTIONALITY

--without-memory-manager
  Disable building Open MPI's memory manager.  Open MPI's memory
  manager is usually built on Linux based platforms, and is generally
  only used for optimizations with some OpenFabrics-based networks (it
  is not *necessary* for OpenFabrics networks, but some performance
  loss may be observed without it).

  However, it may be necessary to disable the memory manager in order
  to build Open MPI statically.

--with-ft=TYPE
  Specify the type of fault tolerance to enable.  Options: LAM
  (LAM/MPI-like), cr (Checkpoint/Restart).  Fault tolerance support is
  disabled unless this option is specified.

--enable-peruse
  Enable the PERUSE MPI data analysis interface.

--enable-heterogeneous
  Enable support for running on heterogeneous clusters (e.g., machines
  with different endian representations).  Heterogeneous support is
  disabled by default because it imposes a minor performance penalty.

  *** THIS FUNCTIONALITY IS CURRENTLY BROKEN - DO NOT USE ***

--with-wrapper-cflags=<cflags>
--with-wrapper-cxxflags=<cxxflags>
--with-wrapper-fflags=<fflags>
--with-wrapper-fcflags=<fcflags>
--with-wrapper-ldflags=<ldflags>
--with-wrapper-libs=<libs>
  Add the specified flags to the default flags that used are in Open
  MPI's "wrapper" compilers (e.g., mpicc -- see below for more
  information about Open MPI's wrapper compilers).  By default, Open
  MPI's wrapper compilers use the same compilers used to build Open
  MPI and specify a minimum set of additional flags that are necessary
  to compile/link MPI applications.  These configure options give
  system administrators the ability to embed additional flags in
  OMPI's wrapper compilers (which is a local policy decision).  The
  meanings of the different flags are:

  <cflags>:   Flags passed by the mpicc wrapper to the C compiler
  <cxxflags>: Flags passed by the mpic++ wrapper to the C++ compiler
  <fcflags>:  Flags passed by the mpifort wrapper to the Fortran compiler
  <ldflags>:  Flags passed by all the wrappers to the linker
  <libs>:     Flags passed by all the wrappers to the linker

  There are other ways to configure Open MPI's wrapper compiler
  behavior; see the Open MPI FAQ for more information.

There are many other options available -- see "./configure --help".

Changing the compilers that Open MPI uses to build itself uses the
standard Autoconf mechanism of setting special environment variables
either before invoking configure or on the configure command line.
The following environment variables are recognized by configure:

CC          - C compiler to use
CFLAGS      - Compile flags to pass to the C compiler
CPPFLAGS    - Preprocessor flags to pass to the C compiler

CXX         - C++ compiler to use
CXXFLAGS    - Compile flags to pass to the C++ compiler
CXXCPPFLAGS - Preprocessor flags to pass to the C++ compiler

FC          - Fortran compiler to use
FCFLAGS     - Compile flags to pass to the Fortran compiler

LDFLAGS     - Linker flags to pass to all compilers
LIBS        - Libraries to pass to all compilers (it is rarely
              necessary for users to need to specify additional LIBS)

PKG_CONFIG  - Path to the pkg-config utility

For example:

  shell$ ./configure CC=mycc CXX=myc++ FC=myfortran ...

*** NOTE: We generally suggest using the above command line form for
    setting different compilers (vs. setting environment variables and
    then invoking "./configure").  The above form will save all
    variables and values in the config.log file, which makes
    post-mortem analysis easier if problems occur.

Note that if you intend to compile Open MPI with a "make" other than
the default one in your PATH, then you must either set the $MAKE
environment variable before invoking Open MPI's configure script, or
pass "MAKE=your_make_prog" to configure.  For example:

  shell$ ./configure MAKE=/path/to/my/make ...

This could be the case, for instance, if you have a shell alias for
"make", or you always type "gmake" out of habit.  Failure to tell
configure which non-default "make" you will use to compile Open MPI
can result in undefined behavior (meaning: don't do that).

Note that you may also want to ensure that the value of
LD_LIBRARY_PATH is set appropriately (or not at all) for your build
(or whatever environment variable is relevant for your operating
system).  For example, some users have been tripped up by setting to
use a non-default Fortran compiler via FC, but then failing to set
LD_LIBRARY_PATH to include the directory containing that non-default
Fortran compiler's support libraries.  This causes Open MPI's
configure script to fail when it tries to compile / link / run simple
Fortran programs.

It is required that the compilers specified be compile and link
compatible, meaning that object files created by one compiler must be
able to be linked with object files from the other compilers and
produce correctly functioning executables.

Open MPI supports all the "make" targets that are provided by GNU
Automake, such as:

all       - build the entire Open MPI package
install   - install Open MPI
uninstall - remove all traces of Open MPI from the $prefix
clean     - clean out the build tree

Once Open MPI has been built and installed, it is safe to run "make
clean" and/or remove the entire build tree.

VPATH and parallel builds are fully supported.

Generally speaking, the only thing that users need to do to use Open
MPI is ensure that <prefix>/bin is in their PATH and <prefix>/lib is
in their LD_LIBRARY_PATH.  Users may need to ensure to set the PATH
and LD_LIBRARY_PATH in their shell setup files (e.g., .bashrc, .cshrc)
so that non-interactive rsh/ssh-based logins will be able to find the
Open MPI executables.

===========================================================================

Open MPI Version Numbers and Binary Compatibility
-------------------------------------------------

Open MPI has two sets of version numbers that are likely of interest
to end users / system administrator:

  * Software version number
  * Shared library version numbers

Both are predicated on Open MPI's definition of "backwards
compatibility."

NOTE: The version numbering conventions were changed with the release
      of v1.10.0.  Most notably, Open MPI no longer uses an "odd/even"
      release schedule to indicate feature development vs. stable
      releases.  See the README in releases prior to v1.10.0 for more
      information (e.g.,
      https://github.com/open-mpi/ompi/blob/v1.8/README#L1392-L1475).

Backwards Compatibility
-----------------------

Open MPI version Y is backwards compatible with Open MPI version X
(where Y>X) if users can:

  * Compile an MPI/OpenSHMEM application with version X, mpirun/oshrun
    it with version Y, and get the same user-observable behavior.
  * Invoke ompi_info with the same CLI options in versions X and Y and
    get the same user-observable behavior.

Note that this definition encompasses several things:

  * Application Binary Interface (ABI)
  * MPI / OpenSHMEM run time system
  * mpirun / oshrun command line options
  * MCA parameter names / values / meanings

However, this definition only applies when the same version of Open
MPI is used with all instances of the runtime and MPI / OpenSHMEM
processes in a single MPI job.  If the versions are not exactly the
same everywhere, Open MPI is not guaranteed to work properly in any
scenario.

Backwards compatibility tends to work best when user applications are
dynamically linked to one version of the Open MPI / OSHMEM libraries,
and can be updated at run time to link to a new version of the Open
MPI / OSHMEM libraries.

For example, if an MPI / OSHMEM application links statically against
the libraries from Open MPI vX, then attempting to launch that
application with mpirun / oshrun from Open MPI vY is not guaranteed to
work (because it is mixing vX and vY of Open MPI in a single job).

Similarly, if using a container technology that internally bundles all
the libraries from Open MPI vX, attempting to launch that container
with mpirun / oshrun from Open MPI vY is not guaranteed to work.

Software Version Number
-----------------------

Official Open MPI releases use the common "A.B.C" version identifier
format.  Each of the three numbers has a specific meaning:

  * Major: The major number is the first integer in the version string
    Changes in the major number typically indicate a significant
    change in the code base and/or end-user functionality, and also
    indicate a break from backwards compatibility.  Specifically: Open
    MPI releases with different major version numbers are not
    backwards compatibale with each other.

    CAVEAT: This rule does not extend to versions prior to v1.10.0.
            Specifically: v1.10.x is not guaranteed to be backwards
            compatible with other v1.x releases.

  * Minor: The minor number is the second integer in the version
    string.  Changes in the minor number indicate a user-observable
    change in the code base and/or end-user functionality.  Backwards
    compatibility will still be preserved with prior releases that
    have the same major version number (e.g., v2.5.3 is backwards
    compatible with v2.3.1).

  * Release: The release number is the third integer in the version
    string.  Changes in the release number typically indicate a bug
    fix in the code base and/or end-user functionality.  For example,
    if there is a release that only contains bug fixes and no other
    user-observable changes or new features, only the third integer
    will be increased (e.g., from v4.3.0 to v4.3.1).

The "A.B.C" version number may optionally be followed by a Quantifier:

  * Quantifier: Open MPI version numbers sometimes have an arbitrary
    string affixed to the end of the version number. Common strings
    include:

    o aX: Indicates an alpha release. X is an integer indicating the
      number of the alpha release (e.g., v1.10.3a5 indicates the 5th
      alpha release of version 1.10.3).
    o bX: Indicates a beta release. X is an integer indicating the
      number of the beta release (e.g., v1.10.3b3 indicates the 3rd
      beta release of version 1.10.3).
    o rcX: Indicates a release candidate. X is an integer indicating
      the number of the release candidate (e.g., v1.10.3rc4 indicates
      the 4th release candidate of version 1.10.3).

Nightly development snapshot tarballs use a different version number
scheme; they contain three distinct values:

   * The git branch name from which the tarball was created.
   * The date/timestamp, in YYYYMMDDHHMM format.
   * The hash of the git commit from which the tarball was created.

For example, a snapshot tarball filename of
"openmpi-v2.x-201703070235-e4798fb.tar.gz" indicates that this tarball
was created from the v2.x branch, on March 7, 2017, at 2:35am GMT,
from git hash e4798fb.

Shared Library Version Number
-----------------------------

The GNU Libtool official documentation details how the versioning
scheme works.  The quick version is that the shared library versions
are a triple of integers: (current,revision,age), or "c:r:a".  This
triple is not related to the Open MPI software version number.  There
are six simple rules for updating the values (taken almost verbatim
from the Libtool docs):

 1. Start with version information of "0:0:0" for each shared library.

 2. Update the version information only immediately before a public
    release of your software. More frequent updates are unnecessary,
    and only guarantee that the current interface number gets larger
    faster.

 3. If the library source code has changed at all since the last
    update, then increment revision ("c:r:a" becomes "c:r+1:a").

 4. If any interfaces have been added, removed, or changed since the
    last update, increment current, and set revision to 0.

 5. If any interfaces have been added since the last public release,
    then increment age.

 6. If any interfaces have been removed since the last public release,
    then set age to 0.

Here's how we apply those rules specifically to Open MPI:

 1. The above rules do not apply to MCA components (a.k.a. "plugins");
    MCA component .so versions stay unspecified.

 2. The above rules apply exactly as written to the following
    libraries starting with Open MPI version v1.5 (prior to v1.5,
    libopen-pal and libopen-rte were still at 0:0:0 for reasons
    discussed in bug ticket #2092
    https://svn.open-mpi.org/trac/ompi/ticket/2092):

    * libopen-rte
    * libopen-pal
    * libmca_common_*

 3. The following libraries use a slightly modified version of the
    above rules: rules 4, 5, and 6 only apply to the official MPI and
    OpenSHMEM interfaces (functions, global variables).  The rationale
    for this decision is that the vast majority of our users only care
    about the official/public MPI/OpenSHMEM interfaces; we therefore
    want the .so version number to reflect only changes to the
    official MPI/OpenSHMEM APIs.  Put simply: non-MPI/OpenSHMEM API /
    internal changes to the MPI-application-facing libraries are
    irrelevant to pure MPI/OpenSHMEM applications.

    * libmpi
    * libmpi_mpifh
    * libmpi_usempi_tkr
    * libmpi_usempi_ignore_tkr
    * libmpi_usempif08
    * libmpi_cxx
    * libmpi_java
    * liboshmem

===========================================================================

Checking Your Open MPI Installation
-----------------------------------

The "ompi_info" command can be used to check the status of your Open
MPI installation (located in <prefix>/bin/ompi_info).  Running it with
no arguments provides a summary of information about your Open MPI
installation.

Note that the ompi_info command is extremely helpful in determining
which components are installed as well as listing all the run-time
settable parameters that are available in each component (as well as
their default values).

The following options may be helpful:

--all       Show a *lot* of information about your Open MPI
            installation.
--parsable  Display all the information in an easily
            grep/cut/awk/sed-able format.
--param <framework> <component>
            A <framework> of "all" and a <component> of "all" will
            show all parameters to all components.  Otherwise, the
            parameters of all the components in a specific framework,
            or just the parameters of a specific component can be
            displayed by using an appropriate <framework> and/or
            <component> name.
--level <level>
            By default, ompi_info only shows "Level 1" MCA parameters
            -- parameters that can affect whether MPI processes can
            run successfully or not (e.g., determining which network
            interfaces to use).  The --level option will display all
            MCA parameters from level 1 to <level> (the max <level>
            value is 9).  Use "ompi_info --param <framework>
            <component> --level 9" to see *all* MCA parameters for a
            given component.  See "The Modular Component Architecture
            (MCA)" section, below, for a fuller explanation.

Changing the values of these parameters is explained in the "The
Modular Component Architecture (MCA)" section, below.

When verifying a new Open MPI installation, we recommend running six
tests:

1. Use "mpirun" to launch a non-MPI program (e.g., hostname or uptime)
   across multiple nodes.

2. Use "mpirun" to launch a trivial MPI program that does no MPI
   communication (e.g., the hello_c program in the examples/ directory
   in the Open MPI distribution).

3. Use "mpirun" to launch a trivial MPI program that sends and
   receives a few MPI messages (e.g., the ring_c program in the
   examples/ directory in the Open MPI distribution).

4. Use "oshrun" to launch a non-OpenSHMEM program across multiple
   nodes.

5. Use "oshrun" to launch a trivial MPI program that does no OpenSHMEM
   communication (e.g., hello_shmem.c program in the examples/
   directory in the Open MPI distribution.)

6. Use "oshrun" to launch a trivial OpenSHMEM program that puts and
   gets a few messages. (e.g., the ring_shmem.c in the examples/
   directory in the Open MPI distribution.)

If you can run all six of these tests successfully, that is a good
indication that Open MPI built and installed properly.

===========================================================================

Open MPI API Extensions
-----------------------

Open MPI contains a framework for extending the MPI API that is
available to applications.  Each extension is usually a standalone set
of functionality that is distinct from other extensions (similar to
how Open MPI's plugins are usually unrelated to each other).  These
extensions provide new functions and/or constants that are available
to MPI applications.

WARNING: These extensions are neither standard nor portable to other
MPI implementations!

Compiling the extensions
------------------------

Open MPI extensions are all enabled by default; they can be disabled
via the --disable-mpi-ext command line switch.

Since extensions are meant to be used by advanced users only, this
file does not document which extensions are available or what they
do.  Look in the ompi/mpiext/ directory to see the extensions; each
subdirectory of that directory contains an extension.  Each has a
README file that describes what it does.

Using the extensions
--------------------

To reinforce the fact that these extensions are non-standard, you must
include a separate header file after <mpi.h> to obtain the function
prototypes, constant declarations, etc.  For example:

-----
#include <mpi.h>
#if defined(OPEN_MPI) && OPEN_MPI
#include <mpi-ext.h>
#endif

int main() {
    MPI_Init(NULL, NULL);

#if defined(OPEN_MPI) && OPEN_MPI
    {
        char ompi_bound[OMPI_AFFINITY_STRING_MAX];
        char current_binding[OMPI_AFFINITY_STRING_MAX];
        char exists[OMPI_AFFINITY_STRING_MAX];
        OMPI_Affinity_str(OMPI_AFFINITY_LAYOUT_FMT, ompi_bound,
                          current_bindings, exists);
    }
#endif
    MPI_Finalize();
    return 0;
}
-----

Notice that the Open MPI-specific code is surrounded by the #if
statement to ensure that it is only ever compiled by Open MPI.

The Open MPI wrapper compilers (mpicc and friends) should
automatically insert all relevant compiler and linker flags necessary
to use the extensions.  No special flags or steps should be necessary
compared to "normal" MPI applications.

===========================================================================

Compiling Open MPI Applications
-------------------------------

Open MPI provides "wrapper" compilers that should be used for
compiling MPI and OpenSHMEM applications:

C:          mpicc, oshcc
C++:        mpiCC, oshCC (or mpic++ if your filesystem is case-insensitive)
Fortran:    mpifort, oshfort

For example:

  shell$ mpicc hello_world_mpi.c -o hello_world_mpi -g
  shell$

For OpenSHMEM applications:

  shell$ oshcc hello_shmem.c -o hello_shmem -g
  shell$

All the wrapper compilers do is add a variety of compiler and linker
flags to the command line and then invoke a back-end compiler.  To be
specific: the wrapper compilers do not parse source code at all; they
are solely command-line manipulators, and have nothing to do with the
actual compilation or linking of programs.  The end result is an MPI
executable that is properly linked to all the relevant libraries.

Customizing the behavior of the wrapper compilers is possible (e.g.,
changing the compiler [not recommended] or specifying additional
compiler/linker flags); see the Open MPI FAQ for more information.

Alternatively, Open MPI also installs pkg-config(1) configuration
files under $libdir/pkgconfig.  If pkg-config is configured to find
these files, then compiling / linking Open MPI programs can be
performed like this:

  shell$ gcc hello_world_mpi.c -o hello_world_mpi -g \
              `pkg-config ompi-c --cflags --libs`
  shell$

Open MPI supplies multiple pkg-config(1) configuration files; one for
each different wrapper compiler (language):

------------------------------------------------------------------------
ompi       Synonym for "ompi-c"; Open MPI applications using the C
           MPI bindings
ompi-c     Open MPI applications using the C MPI bindings
ompi-cxx   Open MPI applications using the C or C++ MPI bindings
ompi-fort  Open MPI applications using the Fortran MPI bindings
------------------------------------------------------------------------

The following pkg-config(1) configuration files *may* be installed,
depending on which command line options were specified to Open MPI's
configure script.  They are not necessary for MPI applications, but
may be used by applications that use Open MPI's lower layer support
libraries.

orte:       Open MPI Run-Time Environment applications
opal:       Open Portable Access Layer applications

===========================================================================

Running Open MPI Applications
-----------------------------

Open MPI supports both mpirun and mpiexec (they are exactly
equivalent) to launch MPI applications.  For example:

  shell$ mpirun -np 2 hello_world_mpi
  or
  shell$ mpiexec -np 1 hello_world_mpi : -np 1 hello_world_mpi

are equivalent.  Some of mpiexec's switches (such as -host and -arch)
are not yet functional, although they will not error if you try to use
them.

The rsh launcher (which defaults to using ssh) accepts a -hostfile
parameter (the option "-machinefile" is equivalent); you can specify a
-hostfile parameter indicating an standard mpirun-style hostfile (one
hostname per line):

  shell$ mpirun -hostfile my_hostfile -np 2 hello_world_mpi

If you intend to run more than one process on a node, the hostfile can
use the "slots" attribute.  If "slots" is not specified, a count of 1
is assumed.  For example, using the following hostfile:

---------------------------------------------------------------------------
node1.example.com
node2.example.com
node3.example.com slots=2
node4.example.com slots=4
---------------------------------------------------------------------------

  shell$ mpirun -hostfile my_hostfile -np 8 hello_world_mpi

will launch MPI_COMM_WORLD rank 0 on node1, rank 1 on node2, ranks 2
and 3 on node3, and ranks 4 through 7 on node4.

Other starters, such as the resource manager / batch scheduling
environments, do not require hostfiles (and will ignore the hostfile
if it is supplied).  They will also launch as many processes as slots
have been allocated by the scheduler if no "-np" argument has been
provided.  For example, running a SLURM job with 8 processors:

  shell$ salloc -n 8 mpirun a.out

The above command will reserve 8 processors and run 1 copy of mpirun,
which will, in turn, launch 8 copies of a.out in a single
MPI_COMM_WORLD on the processors that were allocated by SLURM.

Note that the values of component parameters can be changed on the
mpirun / mpiexec command line.  This is explained in the section
below, "The Modular Component Architecture (MCA)".

Open MPI supports oshrun to launch OpenSHMEM applications. For
example:

   shell$ oshrun -np 2 hello_world_oshmem

OpenSHMEM applications may also be launched directly by resource
managers such as SLURM. For example, when OMPI is configured
--with-pmi and --with-slurm, one may launch OpenSHMEM applications via
srun:

   shell$ srun -N 2 hello_world_oshmem

===========================================================================

The Modular Component Architecture (MCA)

The MCA is the backbone of Open MPI -- most services and functionality
are implemented through MCA components.  Here is a list of all the
component frameworks in Open MPI:

---------------------------------------------------------------------------

MPI component frameworks:
-------------------------

bml       - BTL management layer
coll      - MPI collective algorithms
fbtl      - file byte transfer layer: abstraction for individual
            read/write operations for OMPIO
fcoll     - collective read and write operations for MPI I/O
fs        - file system functions for MPI I/O
io        - MPI I/O
mtl       - Matching transport layer, used for MPI point-to-point
            messages on some types of networks
op        - Back end computations for intrinsic MPI_Op operators
osc       - MPI one-sided communications
pml       - MPI point-to-point management layer
rte       - Run-time environment operations
sharedfp  - shared file pointer operations for MPI I/O
topo      - MPI topology routines
vprotocol - Protocols for the "v" PML

OpenSHMEM component frameworks:
-------------------------

atomic    - OpenSHMEM atomic operations
memheap   - OpenSHMEM memory allocators that support the
            PGAS memory model
scoll     - OpenSHMEM collective operations
spml      - OpenSHMEM "pml-like" layer: supports one-sided,
            point-to-point operations
sshmem    - OpenSHMEM shared memory backing facility


Back-end run-time environment (RTE) component frameworks:
---------------------------------------------------------

dfs       - Distributed file system
errmgr    - RTE error manager
ess       - RTE environment-specific services
filem     - Remote file management
grpcomm   - RTE group communications
iof       - I/O forwarding
notifier  - System-level notification support
odls      - OpenRTE daemon local launch subsystem
oob       - Out of band messaging
plm       - Process lifecycle management
ras       - Resource allocation system
rmaps     - Resource mapping system
rml       - RTE message layer
routed    - Routing table for the RML
rtc       - Run-time control framework
schizo    - OpenRTE personality framework
state     - RTE state machine

Miscellaneous frameworks:
-------------------------

allocator   - Memory allocator
backtrace   - Debugging call stack backtrace support
btl         - Point-to-point Byte Transfer Layer
dl          - Dynamic loading library interface
event       - Event library (libevent) versioning support
hwloc       - Hardware locality (hwloc) versioning support
if          - OS IP interface support
installdirs - Installation directory relocation services
memchecker  - Run-time memory checking
memcpy      - Memory copy support
memory      - Memory management hooks
mpool       - Memory pooling
patcher     - Symbol patcher hooks
pmix        - Process management interface (exascale)
pstat       - Process status
rcache      - Memory registration cache
sec         - Security framework
shmem       - Shared memory support (NOT related to OpenSHMEM)
timer       - High-resolution timers

---------------------------------------------------------------------------

Each framework typically has one or more components that are used at
run-time.  For example, the btl framework is used by the MPI layer to
send bytes across different types underlying networks.  The tcp btl,
for example, sends messages across TCP-based networks; the openib btl
sends messages across OpenFabrics-based networks.

Each component typically has some tunable parameters that can be
changed at run-time.  Use the ompi_info command to check a component
to see what its tunable parameters are.  For example:

  shell$ ompi_info --param btl tcp

shows some of the parameters (and default values) for the tcp btl
component (use --level to show *all* the parameters; see below).

Note that ompi_info only shows a small number a component's MCA
parameters by default.  Each MCA parameter has a "level" value from 1
to 9, corresponding to the MPI-3 MPI_T tool interface levels.  In Open
MPI, we have interpreted these nine levels as three groups of three:

 1. End user / basic
 2. End user / detailed
 3. End user / all

 4. Application tuner / basic
 5. Application tuner / detailed
 6. Application tuner / all

 7. MPI/OpenSHMEM developer / basic
 8. MPI/OpenSHMEM developer / detailed
 9. MPI/OpenSHMEM developer / all

Here's how the three sub-groups are defined:

 1. End user: Generally, these are parameters that are required for
    correctness, meaning that someone may need to set these just to
    get their MPI/OpenSHMEM application to run correctly.
 2. Application tuner: Generally, these are parameters that can be
    used to tweak MPI application performance.
 3. MPI/OpenSHMEM developer: Parameters that either don't fit in the
    other two, or are specifically intended for debugging /
    development of Open MPI itself.

Each sub-group is broken down into three classifications:

 1. Basic: For parameters that everyone in this category will want to
    see.
 2. Detailed: Parameters that are useful, but you probably won't need
    to change them often.
 3. All: All other parameters -- probably including some fairly
    esoteric parameters.

To see *all* available parameters for a given component, specify that
ompi_info should use level 9:

  shell$ ompi_info --param btl tcp --level 9

These values can be overridden at run-time in several ways.  At
run-time, the following locations are examined (in order) for new
values of parameters:

1. <prefix>/etc/openmpi-mca-params.conf

   This file is intended to set any system-wide default MCA parameter
   values -- it will apply, by default, to all users who use this Open
   MPI installation.  The default file that is installed contains many
   comments explaining its format.

2. $HOME/.openmpi/mca-params.conf

   If this file exists, it should be in the same format as
   <prefix>/etc/openmpi-mca-params.conf.  It is intended to provide
   per-user default parameter values.

3. environment variables of the form OMPI_MCA_<name> set equal to a
   <value>

   Where <name> is the name of the parameter.  For example, set the
   variable named OMPI_MCA_btl_tcp_frag_size to the value 65536
   (Bourne-style shells):

   shell$ OMPI_MCA_btl_tcp_frag_size=65536
   shell$ export OMPI_MCA_btl_tcp_frag_size

4. the mpirun/oshrun command line: --mca <name> <value>

   Where <name> is the name of the parameter.  For example:

   shell$ mpirun --mca btl_tcp_frag_size 65536 -np 2 hello_world_mpi

These locations are checked in order.  For example, a parameter value
passed on the mpirun command line will override an environment
variable; an environment variable will override the system-wide
defaults.

Each component typically activates itself when relevant.  For example,
the usNIC component will detect that usNIC devices are present and
will automatically be used for MPI communications.  The SLURM
component will automatically detect when running inside a SLURM job
and activate itself.  And so on.

Components can be manually activated or deactivated if necessary, of
course.  The most common components that are manually activated,
deactivated, or tuned are the "BTL" components -- components that are
used for MPI point-to-point communications on many types common
networks.

For example, to *only* activate the TCP and "self" (process loopback)
components are used for MPI communications, specify them in a
comma-delimited list to the "btl" MCA parameter:

   shell$ mpirun --mca btl tcp,self hello_world_mpi

To add shared memory support, add "vader" into the command-delimited
list (list order does not matter):

   shell$ mpirun --mca btl tcp,vader,self hello_world_mpi

(there is an "sm" shared memory BTL, too, but "vader" is a newer
generation of shared memory support; by default, "vader" will be used
instead of "sm")

To specifically deactivate a specific component, the comma-delimited
list can be prepended with a "^" to negate it:

   shell$ mpirun --mca btl ^tcp hello_mpi_world

The above command will use any other BTL component other than the tcp
component.

===========================================================================

Common Questions
----------------

Many common questions about building and using Open MPI are answered
on the FAQ:

    https://www.open-mpi.org/faq/

===========================================================================

Got more questions?
-------------------

Found a bug?  Got a question?  Want to make a suggestion?  Want to
contribute to Open MPI?  Please let us know!

When submitting questions and problems, be sure to include as much
extra information as possible.  This web page details all the
information that we request in order to provide assistance:

     https://www.open-mpi.org/community/help/

User-level questions and comments should generally be sent to the
user's mailing list (users@lists.open-mpi.org).  Because of spam, only
subscribers are allowed to post to this list (ensure that you
subscribe with and post from *exactly* the same e-mail address --
joe@example.com is considered different than
joe@mycomputer.example.com!).  Visit this page to subscribe to the
user's list:

     http://lists.open-mpi.org/mailman/listinfo/users

Developer-level bug reports, questions, and comments should generally
be sent to the developer's mailing list (devel@lists.open-mpi.org).
Please do not post the same question to both lists.  As with the
user's list, only subscribers are allowed to post to the developer's
list.  Visit the following web page to subscribe:

     http://lists.open-mpi.org/mailman/listinfo/devel

Make today an Open MPI day!
