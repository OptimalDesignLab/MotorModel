# User Guide for SCOREC Motor Model

This guide explains in a step-by-step approach the recommended way to install the MotorModel software package.

The simulations within the MotorModel package use MISO, which is a C++ software library built on MFEM. Before continuing, MISO must be completely built, as well as MFEM and other dependencies.

Go to [MISO Installation](https://miso-installation.readthedocs.io/en/latest/) to find a guide on setting up MISO. It is strongly recommended that all downloads and builds are completed in one folder so that the eventual Python environment will be able to find all of the installed dependencies. This guide assumes that the formatting is the same as what's recommended in the MISO guide. If you use this guide, make sure that you follow the instructions to incorporate Python functionality.

Some of the tests contained within MotorModel require the use of SNOPT, which, unlike the other dependencies for this package, is not open-source, and must be purchased. See the [SNOPT Website](http://www.sbsi-sol-optimize.com/asp/sol_product_snopt.htm) for more information.

## Python

While the supporting MFEM and MISO libraries are made in C++, the code that is used to run an analysis is written in Python. Any work with Python must come after all of the above dependencies are installed.

Python must be installed as a series of systems packages. Another package, patch, is also required to install one of the dependencies. Patch is included with WSL, but may need to be manually installed if you are using another platform.

```
sudo apt install python3
sudo apt install python3-pip
sudo apt install python3-mpi4py
sudo apt install python3.10-venv
sudo apt install patch
sudo apt install swig
sudo apt install libblas-dev liblapack-dev libopenblas-dev
```

## Environment Setup

A virtual environment must be made to be able to access Python libraries while in the terminal. All Python files must be kept in the same file location as the C++ dependencies.

If a Python environment has already been created, skip this step.

Enter the terminal and navigate to the motor folder. Use the following lines of code to create and activate a virtual Python environment.

```
python -m venv source /[path_to_motor_folder]/python
source /[path_to_motor_folder]/python/bin/activate
```

The source command must be repeated on each PC reboot, when Python is being used.

## Dependency Downloads and Installations

MotorModel, along with dependencies mphys and omESP, must be downloaded and installed within the Python environment, and MISO must have its Python layer installed as well.

Enter the terminal and navigate to the motor folder. Use the following lines of code to download and install the various Python-based dependencies.

```
cd MISO
pip install -e .

cd ../Dependencies

git clone https://github.com/tuckerbabcock/mphys.git
cd mphys
git checkout c67212caca8ecb4a934b4b4bb05c1989b7041f03
pip install -e .
cd ..

git clone https://github.com/tuckerbabcock/omESP.git
cd omESP
git checkout b421974a6932780f5b80096228cb2bcf0d63c931
pip install -e .
cd ..

git clone https://github.com/tuckerbabcock/MotorModel.git
cd MotorModel
git checkout 12d953921a4dc926ac6ef31018de61f35b664014
pip install -e .
cd ../..
```

## Building PyOptSparse

PyOptSparse is the final installation for Python. This installation requires SNOPT. If SNOPT is not available, then the next step can not be completed. SNOPT is used for large-scale optimization problems, so it may be possible to use another package that has similar capabilities. The MotorModel tests and the below code assume that SNOPT is used, and they are not guaranteed to function properly with an alternative package.

When SNOPT is acquired, the files must be placed in the motor folder so as to be used by the Python environment.

Enter the terminal and navigate to the motor folder. Use the following lines of code to download and install PyOptSparse using SNOPT.

```
git clone https://github.com/OpenMDAO/build_pyoptsparse.git
cd build_pyoptsparse
pip install -e .
python build_pyoptsparse.py -s '/[path_to_motor_folder]/SNOPT'
export LD_LIBRARY_PATH=$VIRTUAL_ENV/lib
cd ..
```

## Running an Analysis

At this time, there is an error preventing the running of an analysis. Further instructions will be added when the issue has been corrected.
