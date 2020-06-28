Running Benchmarks in Docker Containers
=======================================

dockerfiles are definitiion files for docker. 
Container images can be built from these files using rudimentary docker
commands.
he images for these files can be found at containers/HPL/docker and
containers/memeorySTREAM/singularity for HPL and STREAM benchmark respectively

to create and run a STREAM image named streamdocker

cd containers/memorySTREAM/docker

sudo docker build --tag streamdocker .

Run the image using

sudo docker run -it streamdocker /bin/bash

now run the stream benchmark using stream_benchmark

To build and run a HPL image named hpldocker

cd containers/HPL/docker

sudo docker build --tag hpldocker .

sudo docker run -it hpldocker /bin/bash

now you are inside the docker image

you will see the xhpl executable and HPL.dat

configure the HPL.dat as needed

run the HPL benchmark using

mpirun -np <NO_OF_PROCCES> xhpl
