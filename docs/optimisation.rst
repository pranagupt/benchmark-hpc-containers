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
