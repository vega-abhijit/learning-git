# WRF Complilation and Sample Run

This documentation will provide a brief overview regarding the complilation of the WRF-ARW Model. Later on the documentation will guide through the whole process of running the model.

> ### ***Note: All the scripts provided here are in `bash`.***

---

## System Environment Tests

This section tests whether necessary compliers and scripting languages are present in the system to build WRF properly. Please ensure the following compilers and scripting languages installed in your system.

- gfortran
- cpp
- gcc
- csh
- sh
- perl

To test whether these exist on the system, type the following in your terminal:

```bash
$ which gfortran
$ which cpp
$ which gcc
$ which csh
$ which sh
$ which perl
```

Each of the above commands should give a similar output given below:

```bash
/usr/bin/cpp
```

Showing no output would imply the absense of the compiler in your system. In such case please run the following command to install the package.

```bash
$ sudo apt-get install #name_of_the_package
```
A general command to install all the required packages is given below:

```bash
$ sudo apt-get install gfortran m4 build-essential g++ make tcsh gfortran-multilib
```

List of mandatory UNIX Commands:

||||
| :---: | :---: | :---: |
| ar | head | sed |
| awk | hostname | sleep |
| cat | ln | sort |
| cd | ls | tar |
| cp | make | touch |
| cut | mkdir | tr |
| expr | mv | uname |
| file | nm | wc |
| grep | printf | which |
| gzip | rm | m4 |

To tests all the compilers working properly, follow the steps from ***[This Section.](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compilation_tutorial.php#STEP1)***

---

## Building Libraries

This section focuses on building the following three types of libraries based on their functions in compliling the model. These libraries need to be built properly in order to compile the model successfully.

- [NetCDF](#netcdf-network-common-data-form)
- [MPI]()
- [GRIB2 Libraries]()

To start building the libraries make one directory for the model and another directory for the libraries inside the model directory on your system using the following commands:

```bash
$ mkdir WRF
$ cd WRF
$ mkdir libraries
```

Now the following commands need to be run to set the environment variables.

```bash
$ export wrf=/path_to_the_created_WRF_directory
$ export DIR=$wrf/libraries
$ export CC=gcc
$ export CXX=g++
$ export FC=gfortran
$ export FCFLAGS=-m64
$ export F77=gfortran
$ export FFLAGS=-m64
$ export JASPERLIB=$DIR/grib2/lib
$ export JASPERINC=$DIR/grib2/include
$ export LDFLAGS=-L$DIR/grib2/lib
$ export CPPFLAGS=-I$DIR/grib2/include
```

You can run these commands individually or copy them to a text file with `.sh` extension (For example: ***file_name.sh***) and run the following command to execute the file:

```bash
$ source #file_name.sh
```


> ### NetCDF (Network Common Data Form)
>
> This is the only mandatory library for building the WRF. NetCDF is a set of software libraries and machine-independent data formats that support the creation, access and sharing of array-oriented scientific data. It is also a community standard for sharing scientific data. For futher knowledge visit [here.](https://www.unidata.ucar.edu/software/netcdf/)
>
> For compiling WRF it is required to build two NetCDF libraries - ***NetCDF-C*** and ***NetCDF-Fortran***. Both of these libraries are available in [Unidata github page](https://github.com/Unidata). There are several releases that can be found in their release pages ([netcdf-c](https://github.com/Unidata/netcdf-c/releases), [netcdf-fortran](https://github.com/Unidata/netcdf-fortran/releases)). In this documentation, the following versions will used to build the libraries. The later releases show currently unresolved errors while configuring, so you may try to use the following or older releases. Download the files with `.tar.gz` by clicking on the versions below.
>
> | Library | Version |
> | :--- | ---: |
> | NetCDF-C | [v4.9.0](https://github.com/Unidata/netcdf-c/archive/refs/tags/v4.9.0.tar.gz) |
> | NetCDF-Fortran | [v4.5.4](https://github.com/Unidata/netcdf-fortran/archive/refs/tags/v4.5.4.tar.gz) |
>
>
>
>> #### NetCDF-C
>> Untar `netcdf-c-4.9.0.tar.gz` and build the library using the following commands:
>>
>> ```bash
>> $ tar xvfz netcdf-c-4.9.0.tar.gz
>> $ cd netcdf-c-4.9.0
>> $ ./configure --prefix=$DIR/netcdf --disable-dap --disable-netcdf-4 --disable-shared
>> $ make
>> $ make install
>> $ cd ..
>> ```
>> 
>> A successful built should give an output like below in the end. If you get otherwise please check for errors.
>> 
>> ```bash
>> +-------------------------------------------------------------+
>> | Congratulations! You have successfully installed netCDF!    |
>> |                                                             |
>> | You can use script "nc-config" to find out the relevant     |
>> | compiler options to build your application. Enter           |
>> |                                                             |
>> |     nc-config --help                                        |
>> |                                                             |
>> | for additional information.                                 |
>> |                                                             |
>> | CAUTION:                                                    |
>> |                                                             |
>> | If you have not already run "make check", then we strongly  |
>> | recommend you do so. It does not take very long.            |
>> |                                                             |
>> | Before using netCDF to store important data, test your      |
>> | build with "make check".                                    |
>> |                                                             |
>> | NetCDF is tested nightly on many platforms at Unidata       |
>> | but your platform is probably different in some ways.       |
>> |                                                             |
>> | If any tests fail, please see the netCDF web site:          |
>> | https://www.unidata.ucar.edu/software/netcdf/                |
>> |                                                             |
>> | NetCDF is developed and maintained at the Unidata Program   |
>> | Center. Unidata provides a broad array of data and software |
>> | tools for use in geoscience education and research.         |
>> | https://www.unidata.ucar.edu                                 |
>> +-------------------------------------------------------------+
>> ```
>>
> Set the following environment variables:
> 
> ```bash
> $ export LIBS="-lnetcdf -lz"
> $ export LDFLAGS=-L$NETCDF/lib
> $ export CPPFLAGS=-I$NETCDF/include
> $ export NETCDF=$DIR/netcdf
> $ export PATH=$NETCDF/bin:$PATH
> ```
>
>> #### NetCDF-Fortran
>> Untar `netcdf-fortran-4.5.4.tar.gz` and build the library using the following commands:
>>
>> ```bash
>> $ tar xvfz netcdf-fortran-4.5.4.tar.gz
>> $ cd netcdf-fortran-4.5.4
>> $ ./configure --prefix=$DIR/netcdf --disable-dap --disable-netcdf-4 --disable-shared
>> $ make
>> $ make install
>> $ cd ..
>> ```
>> 
>> A successful built should give an output with a successfull message similar to the NetCDF-C.
>
>
>
> ### MPI (Message Passing Interface)
> 
> This is an optional library unline NetCDF. MPI is a standardized means of exchanging messages between multiple computers running in a parallel program across a distributed memory or even between multiple processor cores within the same computer. So, if you don't have multiple processor cores or don't plan to run WRF with multiple processor cores in your system you can skip building this library.
>
> To build WRF, MPICH will be built as a MPI library. You can find the latest release [here](https://www.mpich.org/downloads/). This documentation will use [mpich-4.1.2.tar.gz](https://www.mpich.org/static/downloads/4.1.2/mpich-4.1.2.tar.gz) as an example. Untar `mpich-4.1.2.tar.gz` and build the library using following commands:
>
> ```bash
> $ tar xvfz mpich-4.1.2.tar.gz
> $ cd mpich-4.1.2
> $ ./configure --prefix=$DIR/mpich
> $ make
> $ make install
> $ cd ..
> $ export PATH=$DIR/mpich/bin:$PATH    #SET THE ENVIRONMENT VARIABLE
> ```
>
>
>
> ### GRIB2 Libraries
>
> GRIB is a consise data format to store and share meteorological data. This format is standardized by WMO (World Meteorological Organization).
> 
> [zlib](https://www.zlib.net/), [libpng](http://www.libpng.org/pub/png/libpng.html) and [Jasper](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/jasper-1.900.1.tar.gz) will be used as compression libraries necessary for compiling WPS with GRIB2 capability. Download the following versions:
> 
>  Library | Version |
> | :--- | ---: |
> | zlib | [v1.2.13](https://www.zlib.net/zlib-1.2.13.tar.gz) |
> | libpng | [v1.6.40](http://prdownloads.sourceforge.net/libpng/libpng-1.6.40.tar.gz?download) |
> | Jasper | [v1.900.1](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/jasper-1.900.1.tar.gz) |
> 
>> ***Note that it is required that all the environment variables given before are updated in your system***
>
>>
>> #### zlib
>>
>> Untar `zlib-1.2.13.tar.gz` and use following commands to build the library.
>>
>>```bash
>>$ tar xvfz zlib-1.2.13.tar.gz
>>$ cd zlib-1.2.13
>>$ ./configure --prefix=$DIR/grib2
>>$ make
>>$ make install
>>$ cd ..
>>```
>>
>>#### libpng
>>
>>Untar `libpng-1.6.40.tar.gz` and use following commands to build the library.
>>
>>```bash
>>$ tar xvfz libpng-1.6.40.tar.gz
>>$ cd libpng-1.6.40
>>$ ./configure --prefix=$DIR/grib2
>>$ make
>>$ make install
>>$ cd ..
>>```
>>
>>#### Jasper
>>
>>Untar `jasper-1.900.1.tar.gz` and use following commands to build the library.
>>
>>```bash
>>$ tar xvfz jasper-1.900.1.tar.gz
>>$ cd jasper-1.900.1
>>$ ./configure --prefix=$DIR/grib2
>>$ make
>>$ make install
>>$ cd ..
>>```
