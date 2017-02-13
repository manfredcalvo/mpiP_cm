# mpiP_cm
This repository contains a modify version of the application mpiP that allow you get the communication matrix of a mpi application.

Follow the next steps to run an MPI application with mpiP-3.4.1-modified.

1) Compile and build mpiP-3.4.1-modified:

a) In the directory mpiP-3.4.1-modified execute the following command: 

	'make install'

b) Once the command above has finished execute the following command:

	'cp wrappers.c.in wrappers.c'
	
c) Finally build the shared object necessarily to link the library with your MPI application. To do it execute the following command:

	'make shared'
	
2) Installing library dependencies:

a) To install libunwind in ubuntu 16.04 execute the following command:

	'sudo apt-get install libunwind-dev'

b) To install liberty in ubuntu 16.04 execute the following command:

	'sudo apt-get install libiberty-dev'

c) To install 'binutils' in ubuntu 16.04 execute the following command:

	'sudo apt-get install binutils*'

3) Running an example application with mpiP-3.4.1-modified:

a) Go to the folder examples/AMG2013.

b) Edit the file 'Makefile.include' to compile AMG2013 using the flags that will link mpiP-3.4.1-modified with AMG2013.

	Replace the following line:
	
	INCLUDE_CFLAGS = -O2 -DTIMER_USE_MPI -DHYPRE_LONG_LONG -DHYPRE_NO_GLOBAL_PARTITION
	
	with:
	
	MPIP_LIB_PATH=../../mpiP-3.4.1-modified/lib
	INCLUDE_CFLAGS = -O2 -DTIMER_USE_MPI -DHYPRE_LONG_LONG -DHYPRE_NO_GLOBAL_PARTITION -L$MPIP_LIB_PATH -lmpiP -lbfd -liberty

	The first line is the path of the shared object in the library mpiP-3.4.1-modified.
	The second line link the libraries of mpiP-3.4.1-modified with AMG2013 adding the flags '-L$MPIP_LIB_PATH -lmpiP -lbfd -liberty'.

Note: You have to do this step over the makefile (or compilation file) of the application that you want to profile it.
	
c) Build the application AMG2013 executing the following command:

	'make'

d) Once you have build the application AMG2013 go to the folder examples/AMG2013/test and execute the following command:

	'../../../mpiP-3.4.1-modified/bin/mpirun-mpip -n 8 amg2013 -P 2 2 2'


4) Look the results of the run:



	
