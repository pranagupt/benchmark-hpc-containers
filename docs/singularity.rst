.. _Singularity:

Running Benchmarks in Singularity Containers
============================================
The definition files for singularity containers are .def files.
Container images can be built form these files using rudimentary singularity commands.
The images for these files can be found at in the github repository 
`pranagupt/benchmark-hpc-containers <https://github.com/pranagupt/benchmark-hpc-containers>`_ 
at ``containers/memorySTREAM/singularity``.
for STREAM benchmark and at ``containers/HPL/singularity`` for HPL benchmark.

.. note::

    To know more about singularity, refer the documentation at https://sylabs.io/docs/

Running STREAM in Singularity
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create and run a STREAM image named ``STREAM.simg`` (or another name)::

    $ cd containers/memorySTREAM/singularity
    $ sudo singularity build STREAM.simg xenial_stream.def


.. note::

    Explore ``-remote`` and ``--false-root`` flags if sudo privilege is not available. 
    Refer the documentation at https://sylabs.io/docs/


Run the container using::

    $ ./STREAM.simg

Run the stream benchmark using::
    
    $ ./stream_benchmark

To get the best results, add the flag ``-DSTREAM_ARRAY_SIZE=VAL`` (replace VAL with the required value)in the ``gcc`` line 
in .def file before building the image.
Refer to the optimisation section for details (:ref:`STREAM-optimisation`).
The environment variable OMP_NUM_THREADS allows runtime control of the 
number of threads/cores used when the resulting "stream_omp" program is executed.

Running HPL in Singularity
^^^^^^^^^^^^^^^^^^^^^^^^^^

To create and run an HPL image named ``HPL.simg`` (or another name)::

    $ cd containers/HPL/singularity
    $ sudo singularity build HPL.simg xenial_docker_hpllinpack_simple.def

To run the container::
    
    $ ./HPL.simg

Now you are inside the container. Navigate to the directory with HPL in it::

    $ cd /usr/local/hpl-2.3/bin/linux

Now you will find the ``xhpl`` executable and ``HPL.dat`` configuration file.
Edit the ``HPL.dat`` according to your needs. 
Refer to the optimisation section (:ref:`HPL-optimisation`) to know more about configuring ``HPL.dat``. 
Run the HPL benchmark using::

    $ mpirun -np <NO_OF_PROCCESES> xhpl


