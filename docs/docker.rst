.. _Docker:

Running Benchmarks in Docker Containers
=======================================

Dockerfiles are definition files for building docker containers. 
Container images can be built from these files using rudimentary docker
commands. These definition files can be found in 
`pranagupt/benchmark-hpc-containers <https://github.com/pranagupt/benchmark-hpc-containers>`_ github repository 
at ``containers/HPL/docker`` and ``containers/memorySTREAM/docker`` 
for HPL and STREAM benchmark respectively.

.. note::

    To know more about docker, refer the documentation at https://docs.docker.com/.

Running STREAM in Docker
^^^^^^^^^^^^^^^^^^^^^^^^

To create and run a STREAM image named ``streamdocker``::

    $ cd containers/memorySTREAM/docker
    $ sudo docker build --tag streamdocker .

Run the image using::

    $ sudo docker run -it streamdocker /bin/bash

Now run the stream benchmark using::
    
    $ ./stream_benchmark

To get the best results, modify the flag ``-DSTREAM_ARRAY_SIZE=VAL`` (replace VAL with the required value) in the ``gcc`` line 
in the ``dockerfile`` file before building the image. 
Refer to the optimisation section for details (:ref:`HPL-optimisation`).
The environment variable OMP_NUM_THREADS allows runtime control of the 
number of threads/cores used when the resulting "stream_omp" program is executed.

Running HPL in Docker
^^^^^^^^^^^^^^^^^^^^^

To build and run a HPL image named ``hpldocker``::

    $ cd containers/HPL/docker
    $ sudo docker build --tag hpldocker .
    $ sudo docker run -it hpldocker /bin/bash

Now you are inside the docker image. You will see the ``xhpl`` executable and ``HPL.dat``
Configure the ``HPL.dat`` as needed. Refer to the optimisation section (:ref:`HPL-optimisation`) to know more about configuring ``HPL.dat``.
Run the HPL benchmark using::

    $ mpirun -np <NO_OF_PROCESSES> xhpl
