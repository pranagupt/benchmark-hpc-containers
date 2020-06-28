.. _Baremetal:

Baremetal
=========

This section will help you install HPL and STREAM benchmarks directly
on your system.

Baremetal STREAM
----------------
Running STREAM is as simple as taking the stream.c file from its website, compiling it and running the binary.

Obtain stream.c
^^^^^^^^^^^^^^^
You can obtain the file from the source code directory currently hosted at: https://www.cs.virginia.edu/stream/FTP/Code/

Save the file as stream.c (or any other name).

Compile stream.c
^^^^^^^^^^^^^^^^
Make sure that you are using the gcc compiler to compile stream.c. The gcc compiler comes with openMP,
and can be enabled with the ``-fopenmp`` flag while compiling. You can thus compile stream.c as follows::

    $ gcc -O -DSTREAM_ARRAY_SIZE=10000000 stream.c -o stream_bin

Now, the ``-DSTREAM_ARRAY_SIZE`` is set to ``10000000``. However, you must set it as per your system's 
cache size. Look at the optimisation section (:ref:`STREAM-optimisation`) for details.

Running the STREAM binary
^^^^^^^^^^^^^^^^^^^^^^^^^
If the compilation was succesful, then you must have stream_bin on your system, in the same folder. 
Running it is as simple as::

    $ ./stream_bin

Baremetal HPL
-------------
Installing and configuring HPL is much more difficult than STREAM. You must have an implementation of MPI and 
a BLAS library on your system for HPL to work. We are going to use OpenMPI and Intel's MKL (Math Kernel Library).
You can also use OpenBLAS or AMD Blis as your math library instead of MKL but as per our experiments, MKL is the fastest.
AMD Blis might work faster on AMD processors.

Installing OpenMPI
^^^^^^^^^^^^^^^^^^

First, download the latest version of OpenMPI source code from www.open-mpi.org::

    $ wget https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.4.tar.gz

Extract the files::
    
    $ tar -xzvf openmpi-4.0.4.tar.gz

Now configure the build tree. The prefix should be set to your installation directory. 
It is recommended that your installation folder for openMPI is in ``/opt`` or in your user's root directory::

    $ cd openmpi-4.0.4
    $ ./configure --prefix=/opt/openmpi

Now run the makefiles to install it::

    $ make all install

You should now have openmpi installed in the chosen folder, which for us is ``/opt/openmpi``.
You should have the ``bin``, ``include`` and ``lib`` folders in it.

Installing Intel's MKL
^^^^^^^^^^^^^^^^^^^^^^

You can use a package manager to manage the installation of MKL. This makes things extremely easy and efficient.
The latest instructions for using apt can be found at https://software.intel.com/content/www/us/en/develop/articles/installing-intel-free-libs-and-python-apt-repo.html.

Downloading and Configuring HPL
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Go into the directory where you want to download and configure HPL::

    $ cd ~

Get the latest version of HPL from https://www.netlib.org/benchmark/hpl/software.html::

    $ wget https://www.netlib.org/benchmark/hpl/hpl-2.3.tar.gz

Extract the tar-gzipped file::

    $ tar -xzvf hpl-2.3.tar.gz

The resulting folder will contain many files. Feel free to go through them for a better understanding of HPL.
The setup folder has many makefiles. You can choose to configure any of them if you feel comfortable doing so.
You can also download our makefile from our github repository::

    $ wget https://raw.githubusercontent.com/pranagupt/benchmark-hpc-containers/master/Make.Linux_Intel

Make sure you change all the folder paths and link the MPI and BLAS libraries properly.
Every parameter in the makefile should be correctly configured. Any mistakes here can cause performance issues and failed compilations.

Installing and Running HPL
^^^^^^^^^^^^^^^^^^^^^^^^^^
Once the Makefile has been configured , you can proceed to installation.

Build the HPL executable as follows::

    $ make arch=Intel_Linux64

This might take several minutes, at the end of which a ``bin`` folder containing the binary will be created::

    $ cd bin/Intel_Linux64
    $ ls
    HPL.dat    xhpl

The ``HPL.dat`` file contains all the inputs which need to be changed according to your system's architecture.
Refer to the optimisation section (:ref:`HPL-optimisation`) for more details.
You can then run the executable as follows::

    $ /opt/openmpi-4.0.4/bin/mpirun -np NUM_OF_CORES xhpl

If everything works, then you should have the results after a few minutes to a few hours of calculation according
to your system specifications and ``HPL.dat`` parameters.