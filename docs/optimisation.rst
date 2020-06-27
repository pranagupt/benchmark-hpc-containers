.. _STREAM-optimisation:

Optimising STREAM Benchmark
===========================

STREAM measures memory transfer rates in MB/s for simple computational kernels coded in C.
These kernels work on vectors implemented as *arrays* in C.
There are 3 such arrays used. 

To prevent any interference from cache memory in the measurement of memory speed, the array sizes
must be at least 4 times the cache memory size. 

To get to know your computer's cache memory size, run the following command::

    $ lscpu
    Architecture:          x86_64
    CPU op-mode(s):        32-bit, 64-bit
    Byte Order:            Little Endian
    CPU(s):                56
    On-line CPU(s) list:   0-55
    Thread(s) per core:    2
    Core(s) per socket:    14
    Socket(s):             2
    NUMA node(s):          2
    Vendor ID:             GenuineIntel
    CPU family:            6
    Model:                 79
    Model name:            Intel(R) Xeon(R) CPU E5-2690 v4 @ 2.60GHz
    Stepping:              1
    CPU MHz:               2600.000
    CPU max MHz:           2600.0000
    CPU min MHz:           1200.0000
    BogoMIPS:              5201.37
    Virtualization:        VT-x
    Hypervisor vendor:     vertical
    Virtualization type:   full
    L1d cache:             32K
    L1i cache:             32K
    L2 cache:              256K
    L3 cache:              35840K
    NUMA node0 CPU(s):     0-13,28-41
    NUMA node1 CPU(s):     14-27,42-55

The L3 cache size is what you are looking for. Take the sum of all L3 cache sizes over all the nodes. 
Now triple this value (for the three arrays used in STREAM) and divide by the size of a ``double`` in your system, 
which is generally ``8``. The value of ``STREAM_ARRAY_SIZE`` should be set to be greater than the result obtained.

Examples: 

    Example 1: One Xeon E3 with 8 MB L3 cache ``STREAM_ARRAY_SIZE`` should be >= 4 million, giving 
    an array size of 30.5 MB and a total memory requirement of 91.5 MB.  
    
    Example 2: Two Xeon E5's with 20 MB L3 cache each (using OpenMP) ``STREAM_ARRAY_SIZE`` should be >= 20 million, 
    giving an array size of 153 MB and a total memory requirement of 458 MB.  


.. _HPL-optimisation:

Optimising HPL Benchmark
========================

HPL measures the processing power of your system. It should be configured for maximum
hardware and core utilisation. 

Choosing the best parameters
----------------------------
There are four main parameters that must be properly chosen in the ``HPL.dat`` file:

Problem size ``N``:
^^^^^^^^^^^^^^^^^^^
In order to find out the best performance of your system, 
the largest problem size fitting in memory is what you should aim for. 
The amount of memory used by HPL is essentially the size of the coefficient matrix. 
So for example, if you have 4 nodes with 256 Mb of memory on each, 
this corresponds to 1 Gb total, i.e., 125 M double precision (8 bytes) elements. 
The square root of that number is 11585. One definitely needs to leave some memory for 
the OS as well as for other things, so a problem size of 10000 is likely to fit. 
As a rule of thumb, 80 % of the total amount of memory is a good guess. 
If the problem size you pick is too large, swapping will occur, and the 
performance will drop. If multiple processes are spawn on each node 
(say you have 2 processors per node), what counts is the available amount 
of memory to each process.

Block size ``NB``:
^^^^^^^^^^^^^^^^^^
HPL uses the block size ``NB`` for the data distribution as well as for the 
computational granularity. From a data distribution point of view, the smallest 
NB, the better the load balance. You definitely want to stay away from very 
large values of NB. From a computation point of view, a too small value of ``NB`` 
may limit the computational performance by a large factor because almost no 
data reuse will occur in the highest level of the memory hierarchy. The number 
of messages will also increase. Efficient matrix-multiply routines are often 
internally blocked. Small multiples of this blocking factor are likely to be 
good block sizes for HPL. The bottom line is that "good" block sizes are almost 
always in the [32 .. 256] interval. The best values depend on the computation / 
communication performance ratio of your system. To a much less extent, the 
problem size matters as well. Say for example, you emperically found that 44 
was a good block size with respect to performance. 88 or 132 are likely to give 
slightly better results for large problem sizes because of a slighlty higher 
flop rate.

Process grid ratio ``P`` x ``Q``:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This depends on the physical interconnection network you have.\ 
Assuming a mesh or a switch HPL "likes" a 1:k ratio with k in [1..3]. In other 
words, ``P`` and ``Q`` should be approximately equal, with Q slightly larger than ``P``. 
Examples: 2 x 2, 2 x 4, 2 x 5, 3 x 4, 4 x 4, 4 x 6, 5 x 6, 4 x 8 ... If you 
are running on a simple Ethernet network, there is only one wire through 
which all the messages are exchanged. On such a network, the performance and 
scalability of HPL is strongly limited and very flat process grids are likely 
to be the best choices: 1 x 4, 1 x 8, 2 x 4 ...

All these parameters can only be correctly chosen after multiple runs of HPL with 
varying parameters. You can run ``htop`` in another terminal at the same time to 
keep track of CPU core utilisation, which should be in a cycle of variation between
100% and 85%, approximately.

.. note::

    You can read more about optimising HPL on the official website: https://www.netlib.org/benchmark/hpl/

Efficiency Achieved
-------------------
The theoretical maximum performance of your computer can be calculated as follows:

R\ :sub:`peak` \ = Number of Cores x Frequency x FLOP per cycle

You can find the FLOP per cycle for your processor on the official website of the
manufacturer. You can refer this stackoverflow answer as well: 
https://stackoverflow.com/questions/15655835/flops-per-cycle-for-sandy-bridge-and-haswell-sse2-avx-avx2

The efficiency achieved by HPL on your system for a particular set of parameters is given by:

Efficiency = R\ :sub:`actual` \ / R\ :sub:`peak` \

You should aim for at least 80% efficiency.