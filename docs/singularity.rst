Running Benchmarks in Singularity Containers
============================================
.def files are definition files for singularity.
Container images can be built form these files using rudimentary singularity commands.
The images for these files can be found at containers/memorySTREAM/singularity
for STREAM benchmark and at containers/HPL/singularity for HPL benchmark.

Here we create an image STREAM.simg from the definition file for STREAM
benchmark. However the principles are same for both.

cd containers/memorySTREAM/singularity

(explore -remote and --false-root flags if sudo privilege is not available)
sudo singularity build STREAM.simg xenial_stream.def

run the container using 

./STREAM.simg

