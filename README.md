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

a) After execute the command in the step 3d you will find a file with the pattern 'amg2013.8.<random_number>.1.mpiP' in the folder examples/AMG2013/test.

b) Execute the following command over the file found in the previous step:

	'cat amg2013.8.<random_number>.1.mpiP | grep "Communication" -A 200'

c) The previous command will show you the following communication matrix:

------- Start Communication Matrix -------------

Statistics of rank: 0

Source	Target	Total Amount	Total Count
0	0	539857	3407
0	1	154039	571
0	2	152211	566
0	3	58291	514
0	4	103063	567
0	5	28079	420
0	6	24679	395
0	7	19495	374

Statistics of rank: 1

Source	Target	Total Amount	Total Count
1	0	143528	565
1	1	473584	3262
1	2	44228	510
1	3	143584	523
1	4	18632	408
1	5	97320	522
1	6	12860	344
1	7	13432	390

Statistics of rank: 2

Source	Target	Total Amount	Total Count
2	0	143112	559
2	1	41468	503
2	2	466324	3298
2	3	134844	524
2	4	18596	408
2	5	13764	392
2	6	98092	493
2	7	16448	419

Statistics of rank: 3

Source	Target	Total Amount	Total Count
3	0	43320	512
3	1	140236	523
3	2	134304	523
3	3	457784	3200
3	4	11304	371
3	5	17236	409
3	6	18052	379
3	7	93332	483

Statistics of rank: 4

Source	Target	Total Amount	Total Count
4	0	93968	562
4	1	17880	405
4	2	16584	405
4	3	10492	363
4	4	479348	3365
4	5	146532	564
4	6	143468	558
4	7	50424	508

Statistics of rank: 5

Source	Target	Total Amount	Total Count
5	0	16276	417
5	1	96548	522
5	2	12508	396
5	3	18660	406
5	4	143624	567
5	5	473408	3356
5	6	40624	526
5	7	145168	522

Statistics of rank: 6

Source	Target	Total Amount	Total Count
6	0	16716	393
6	1	12036	341
6	2	96260	496
6	3	13264	379
6	4	143152	560
6	5	37804	525
6	6	459188	3249
6	7	139956	555

Statistics of rank: 7

Source	Target	Total Amount	Total Count
7	0	12200	380
7	1	17616	398
7	2	19084	430
7	3	94056	494
7	4	47484	522
7	5	143616	533
7	6	133224	562
7	7	467280	3319

------- End Communication Matrix -------------

---------------------------------------------------------------------------
@--- End of Report --------------------------------------------------------
--------------------------------------------------------------------------
	
d) The previous communication matrix show for each rank what is the amount of bytes sent to other ranks in the application.

e) If your communication matrix is similar to the one showed here you can be sure that the application is working correctly.

