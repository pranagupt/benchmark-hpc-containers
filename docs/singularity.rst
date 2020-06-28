Running Benchmarks in Singularity Containers
============================================
.def files are definition files for singularity.
Container images can be built form these files using rudimentary singularity commands.
The images for these files can be found at containers/memorySTREAM/singularity
for STREAM benchmark and at containers/HPL/singularity for HPL benchmark.

To create and run a STREAM image named STREAM.simg(replace as needed)

cd containers/memorySTREAM/singularity

sudo singularity build STREAM.simg xenial_stream.def
(explore -remote and --false-root flags if sudo privilege is not available)

run the container using 

./STREAM.simg

run the stream benchmark using
stream_benchmark

To create and run an HPL image named HPL.simg(replace as needed)

cd containers/HPL/singularity

sudo singularity build HPL.simg xenial_docker_hpllinpack_simple.def

to run the container
./HPL.simg
now you are inside the container
navigate to the directory with HPL in it

cd /usr/local/hpl-2.3/bin/linux

now you will find th xhpl executable and HPL.dat cconfiguration file

edit the HPL.dat according to your needs and run the HPL benchmark using

mpirun -np <NO_OF_PROCCESES> xhpl


