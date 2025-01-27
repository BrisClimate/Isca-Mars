## Note for Isca-Mars

This is a development branch of the Isca-Mars set up, used for simulations analyzed
in Ball et al. 2021, "The roles of latent heating and dust in the structure and
variability of the northern Martian polar vortex". DOI:

[![DOI](https://zenodo.org/badge/350370198.svg)](https://zenodo.org/badge/latestdoi/350370198)

For the main Isca repository, please visit:
https://github.com/ExeClim/Isca

***
# Isca

Isca is a framework for the idealized modelling of the global circulation of
planetary atmospheres at varying levels of complexity and realism. The
framework is an outgrowth of models from GFDL designed for Earth's atmosphere,
but it may readily be extended into other planetary regimes. Various forcing
and radiation options are available. At the simple end of the spectrum a
Held-Suarez case is available. An idealized grey radiation scheme, a grey
scheme with moisture feedback, a two-band scheme and a multi-band scheme are
also available, all with simple moist effects and astronomically-based solar
forcing. At the complex end of the spectrum the framework provides a direct
connection to comprehensive atmospheric general circulation models.

For Earth modelling, options include an aqua-planet and configurable (idealized
or realistic) continents with idealized or realistic topography. Continents may
be defined by changing albedo, heat capacity and evaporative parameters, and/or
by using a simple bucket hydrology model. Oceanic Q-fluxes may be added to
reproduce specified sea-surface temperatures, with any continents or on an
aquaplanet. Planetary atmospheres may be configured by changing planetary size,
solar forcing, atmospheric mass, radiative, and other parameters.

The underlying model is written in Fortran and may largely be configured with
Python scripts, with internal coding changes required for non-standard cases.
Python scripts are also used to run the model on different architectures, to
archive the output, and for diagnostics, graphics, and post-processing. All of
these features are publicly available on a Git-based repository.

# Getting Started

A python module `isca` (note lowercase) is provided alongside the Fortran source code that should help to do a lot of the heavy-lifting of compiling, configuring and running the model for you.  Isca can be compiled, run and configured without using python, but using the python wrapper is recommended.

## Installing the `isca` python module

To begin you'll need a copy of the source code. Either fork the Isca repository to your own github username, or clone directly from the ExeClim group.

```{bash}
$ git clone https://github.com/ExeClim/Isca
$ cd Isca
```

The python module is found in the `src` directory and can be installed using `pip`.  It's recommended (but not essential) that you use some sort of python environment manager to do this, such as using the Anaconda distribution and creating an environment (in the code below called "`isca_env`"), or using `virtualenv` instead.  This getting started will use Anaconda.

```{bash}
$ conda create -n isca_env python ipython
...
$ source activate isca_env
(isca_env)$ cd src/extra/python
(isca_env)$ pip install -r requirements.txt
...
Successfully installed MarkupSafe-1.0 f90nml jinja2-2.9.6 numpy-1.13.3 pandas-0.21.0 python-dateutil-2.6.1 pytz-2017.3 sh-1.12.14 six-1.11.0 xarray-0.9.6
```

Now install the `isca` python module in "development mode".  This will allow you, if you wish, to edit the `src/extra/python/isca` files and have those changes be used when you next run an Isca script.

```{bash}
(isca_env)$ pip install -e .
...
Successfully installed Isca
```

## Compiling for the first time

At Exeter University, Isca is compiled using:

* Intel Compiler Suite 14.0
* OpenMPI 10.0.1
* NetCDF 4.3.3.1
* git 2.1.2

Different workstations/servers at different institutions will have different compilers and libraries available.  The Isca framework assumes you have something similar to our stack at Exeter, but provides a hook for you to configure the environment in which the model is run.

Before Isca is compiled/run, an environment is first configured which loads the specific compilers and libraries necessary to build the code.  This done by setting the environment variable `GFDL_ENV` in your session.

For example, on the EMPS workstations at Exeter, I have the following in my `~/.bashrc`:

```{bash}
# directory of the Isca source code
export GFDL_BASE=/scratch/jamesp/Isca 
# "environment" configuration for emps-gv4
export GFDL_ENV=emps-gv
# temporary working directory used in running the model
export GFDL_WORK=/scratch/jamesp/gfdl_work
# directory for storing model output
export GFDL_DATA=/scratch/jamesp/gfdl_data
```

The value of `GFDL_ENV` corresponds to a file in `src/extra/env` that is sourced before each run or compilation.  For an example that you could adapt to work on your machine, see `src/extra/env/emps-gv`.

We are not able to provide support in configuring your environment at other institutions other than Exeter University - we suggest that you contact your friendly local sysops technician for guidance in getting the compilers and libraries collated if you are not sure how to proceed.

If you work at another large institution and have successfully compiled and run Isca, we welcome you to commit your own environment config to `/src/extra/env/my-new-env` for future scientists to benefit from and avoid the pain of debugging compilation!

## Running the model

Once you have installed the `isca` python module you will most likely want to try a compilation and run a simple test case.  There are several test cases highlighting features of Isca in the `exp/test_cases` directory.

A good place to start is the famous Held-Suarez dynamical core test case. Take a look at the python file for an idea of how an Isca experiment is constructed and then try to run it.
```
(isca_env)$ cd $GFDL_BASE/exp/test_cases/held_suarez
(isca_env)$ python held_suarez_test_case.py
```
The `held_suarez_test_case.py` experiment script will attempt to compile the source code for the dry dynamical core and then run for several iterations.  

It is likely that the first time you run the script, compilation will fail.  Debug, adjust your environment file as necessary, and then rerun the python script to try again.

Once the code has sucessfully compiled, the script will continue on to run the model distributed over some number of cores.  Once it completes, netCDF diagnostic files will be saved to `$GFDL_DATA/held_suarez_test_case/run####`.

Once you've got an environment file that works for your machine saved in `src/extra/env`, all of the test cases should now compile and run - you're now ready to start running your own experiments!

## Site-specific help

There are some site-specific guides to running Isca on your local system located in the directory [exp/site_specific/](https://github.com/ExeClim/Isca/tree/master/exp/site_specific).

# License

Isca is distributed under a GNU GPLv3 license. See the `LICENSE` file for details. 

RRTM/RRTMG: Copyright © 2002-2010, Atmospheric and Environmental Research, Inc. (AER, Inc.). 
This software may be used, copied, or redistributed as long as it is not sold and this 
copyright notice is reproduced on each copy made. This model is provided as is without 
any express or implied warranties.

Some of the code provided in the `src/atmos_params/socrates/interface` folder were provided by the UK Met Office,
and are therefore covered by British Crown Copyright. The copyright statement at the top of the 
relevant code is provided below. For the `copyright.txt.` refered to in this statement, please see the
Socrates source code itself, which is downloadable from the Met Office, and is not packaged with Isca.

```
! *****************************COPYRIGHT*******************************
! (C) Crown copyright Met Office. All rights reserved.
! For further details please refer to the file COPYRIGHT.txt
! which you should have received as part of this distribution.
! *****************************COPYRIGHT*******************************
```

The `check_disk_space.py` script, which is used as part of the email-alerts functionality
of the `gfdl` module, was written by Giampaolo Rodola and is released under the MIT license.

The parts of Isca provided by GFDL are also released under a GNU GPL license. A copy of the 
relevant GFDL license statement is provided below.

```
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!                                                                   !!
!!                   GNU General Public License                      !!
!!                                                                   !!
!! This file is part of the Flexible Modeling System (FMS).          !!
!!                                                                   !!
!! FMS is free software; you can redistribute it and/or modify it    !!
!! under the terms of the GNU General Public License as published by !!
!! the Free Software Foundation, either version 3 of the License, or !!
!! (at your option) any later version.                               !!
!!                                                                   !!
!! FMS is distributed in the hope that it will be useful,            !!
!! but WITHOUT ANY WARRANTY; without even the implied warranty of    !!
!! MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the      !!
!! GNU General Public License for more details.                      !!
!!                                                                   !!
!! You should have received a copy of the GNU General Public License !!
!! along with FMS. if not, see: http://www.gnu.org/licenses/gpl.txt  !!
!!                                                                   !!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
```
