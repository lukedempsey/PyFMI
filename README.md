PyFMI
=====

PyFMI is a package for loading and interacting with Functional Mock-Up Units
(FMUs) both for Model Exchange and Co-Simulation, which are compiled dynamic
models compliant with the Functional Mock-Up Interface (FMI). See
http://jmodelica.org/page/4924 and https://www.fmi-standard.org/ for more
information.

The latest version is available for download at
http://pypi.python.org/pypi/PyFMI/.   
The PyFMI code is hosted at https://svn.jmodelica.org/PyFMI/.
There is a git mirror at https://github.com/modelon/PyFMI.   
Active issues are tracked at http://trac.jmodelica.org.   
Discussion forums for PyFMI can be found at http://jmodelica.org/forum.
Please report issues related to PyFMI at the forums.   

Contributions are accepted under the [JModelica.org Contributor License Agreement](http://www.jmodelica.org/page/14). For information about contributing, see [here](http://jmodelica.org/page/15).

# Installation

Note: Building with ```-y``` suppresses the ```Y/n``` request when installing.

## Essential Packages

```
sudo apt-get update
apt-get install -y \
build-essential \
git \
python \
python-dev\
python-setuptools \
make \
cmake \
gfortran \
python-pip\
python-lxml \
subversion \
python-tk \
libblas-dev \
liblapack-dev \
python-gi-cairo

```

## Python Modules
```
sudo pip install
numpy \
scipy \
nose \
pandas \
matplotlib \
sympy \
jupyter \
pytest \
cython \
cairocffi
```

## Sundials
PyFMI needs least Sundials 2.5/2.6, only 2.4.0 install works due to different build method (CMake over Make)
```
mkdir ~/pyfmi
cd ~/pyfmi
wget -qO- https://computation.llnl.gov/projects/sundials/download/sundials-2.4.0.tar.gz | tar xvz
cd sundials-2.4.0
# only set the below CFLAGS if running x64 system
sudo ./configure CFLAGS="-fPIC"
make
sudo make install
```

## FMILibrary
Needs at least 2.0.1
```
cd ~/pyfmi
svn checkout https://svn.jmodelica.org/FMILibrary/tags/2.0.3b1/ FMILibrary-2.0.3b1
mkdir build
cd build
sudo cmake -Wno-dev -DFMILIB_INSTALL_PREFIX=/home/luke/pyfmi/FMILibrary-2.0.3b1 ../
sudo make install test

# add fmilib.h to C compile path
nano ~/.profile
# add:
export C_INCLUDE_PATH=/home/luke/pyfmi/FMILibrary-2.0.3b1/install/include
# (^X to close, Y to save)

# reload .profile
source ~/.profile

# optional, check if it has loaded properly
echo $C_INCLUDE_PATH
```


## LAPACK
Only need if using a GLIMDA solver
```
wget -qO- http://www.netlib.org/lapack/lapack-3.8.0.tar.gz | tar xvz
mv make.inc.example make
make
```

## Assimulo
```
cd ~/pyfmi
wget https://files.pythonhosted.org/packages/4c/c0/19a54949817204313efff9f83f1e4a247edebed0a1cc5a317a95d3f374ae/Assimulo-2.9.zip

unzip Assimulo-2.9.zip
rm Assimulo-2.9.zip

sudo python setup.py install --sundials-home=/home/luke/pyfmi/sundials-2.6.0 --blas-home=/usr --lapack-home=/usr

```

## PyFMI

```
cd ~/pyfmi
svn checkout https://svn.jmodelica.org/PyFMI/tags/PyFMI-2.3.1/ PyFMI-2.3.1
cd PyFMI-2.3.1
```

## Modelica
```
for deb in deb deb-src; do echo "$deb http://build.openmodelica.org/apt `lsb_release -cs` release"; done | sudo tee /etc/apt/sources.list.d/openmodelica.list

wget -q http://build.openmodelica.org/apt/openmodelica.asc -O- | sudo apt-key add - 

apt-key fingerprint

sudo apt update
sudo apt install openmodelica

# Havent yet done (has a broken dependency, so seeing if needed):
sudo apt install omlib-.* # Installs optional Modelica libraries (most have not been tested with OpenModelica)
```
Select the compiler in Tools> Options > FMI > Platforms - check the compiler there