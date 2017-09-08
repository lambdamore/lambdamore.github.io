# WRF Installation Script

[WRF\|Weather Research Forecast](http://www.wrf-model.org) is a popular  meteorological downscaling model. It receives the forcing from GCM\|Global Climate Model or observations, simulates the detailed weather dynamics described by the [primitive equations](https://en.wikipedia.org/wiki/Primitive_equations). The closure of the equation set is guaranteed by a minimum set of physics parameterization schemes including radiation, boundary layer and land-surface parameterization, convective parameterization, subgrid eddy diffusion, and microphysics.

The popularity of WRF is largely due to its open source feature. However, the lack of integrated installation and running circumstance hindered its acceptance for those who are not accustomed to the Unix environment. Given this, a whole installation script for WRF-ARW is provided [here](https://github.com/lambdamore/HPC_Repository/blob/master/WRF/WRF_Install/WRF_Install.sh). To execute it, just run *bash WRF_Install.sh* on terminal. 

The script is consisted of 3 succeeding parts. 

- The first part does the basic environment checking, which includes fortran compiler check and interpreter check. This part is from line 1 to line 52. 

- The second part builds the necessary libraries( netCDF, MPICH, Jasper, Libpng). netCDF is a must-have library as it provides input/output data format for WRF. The compiler used for netCDF must be identical to that used for WRF compilation in the third part. MPICH is a high performance and widely portable implementation of the Message Passing Interface (MPI) standard. It allows WRF to use distribued memory parrallelism\|DMPAR and save the running time. This part is from line 55 to line 180. After the successful execuation of this part, there should be the *netcdf* file, *grib2* file and *mpich* file under the *WRF/LIBRARIES* directory.

- The last part builds the WRF model under the environment provided by the first two parts. The main program(WRF) should be built  before compiling the WPS\|WRF Preprocessing System part. The default mode for WRF compilation here is *gcc DMPAR*. To change it, you need to go to line 191 and change the representative number. The default installed mode is *em_real*, which stands for 3d real case. To change it, got to line 199 and change the number.
The WPS part will be executed after the compilation of WRF main session. After that, the underlying surface dataset will be auto downloaed into the WPS\_GEOG directory. 

The script will delete all the installation package and test files after installation.

Enjoy WRF!