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

> ### NetCDF (Network Common Data Form)
>
> This is the only mandatory library for building the WRF. NetCDF is a set of software libraries and machine-independent data formats that support the creation, access and sharing of array-oriented scientific data. It is also a community standard for sharing scientific data. For futher knowledge visit [here.](https://www.unidata.ucar.edu/software/netcdf/)
>
> For compiling WRF it is required to build two NetCDF libraries - ***NetCDF-C*** and ***NetCDF-Fortran***. Both of these libraries are available in [Unidata github page](https://github.com/Unidata).
>
>> #### NetCDF-C
>> To clone the NetCDF-C repository use the following git command inside the `libraries` directory. If `git` is not installed, use `sudo apt-get install git` to install the package in your system.
>>
>> ```bash
>> $ cd libraries
>> $ git clone https://github.com/Unidata/netcdf-c.git
>> ```
>>
>>
>>
>>
>>
>>
>>
>>
>>
