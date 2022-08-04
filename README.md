[![Build Status](https://travis-ci.org/PNNL-Comp-Mass-Spec/bsf-py.svg?branch=master)](https://travis-ci.org/PNNL-Comp-Mass-Spec/bsf-py)
[![Docker Build Status](https://img.shields.io/docker/build/coldfire79/bsf-py.svg)](https://hub.docker.com/r/coldfire79/bsf-py/)

# bsf-jaccard-py: Python wrapper for Blazing Signature Filter
The Blazing Signature Filter (BSF) is a highly efficient pairwise similarity algorithm which enables extensive data mining within a reasonable amount of time.

This version has been modified - along with the bsf-core - to calculate Jaccard similarity instead of AND or XOR

## How to install
1. Install python. Python3+ and conda recommended. 
Please refer to https://conda.io/docs/download.html and https://www.continuum.io/downloads.
2. Clone this repository.
Go to the desired folder and clone it as follows:
```bash
git clone --recursive https://github.com/PNNL-Compbio/bsf-jaccard-py
```
The 'bsf-core' is a submodule for this repository so that this command will recursively clone the 'bsf-core' too.

3. Install BSF package. You can select either -DBSF_XOR or -DBSF_AND as a build option.
  ```bash
  python setup.py build_ext -DDEBUG -DBSF_AND install
  ```
  * NOTE: This python package contains the C extention module for C++ library of BSF. We employ the OpenMP (Open Multi-Processing) 4.0 specification and C++11 standard for BSF. Therefore, if you don't have any C/C++ compiler which supports these, please install GCC 4.9 or newer. 

  Also, you can explicitly set a valid compiler path or name on the command line as below.
  ```bash
  CC=g++-4.9 CXX=g++-4.9 python setup.py build_ext -DDEBUG -DBSF_AND install
  ```
  After installing, it will automatically run the [unit test](tutorial/test_bsf.py). 

4. Run the test via jupyter notebook
```bash
jupyter notebook
```
On browsers, you can access the tree GUI. 
```url
http://localhost:8888/
```

## Tutorial
Please refer to [this tutorial](tutorial/tutorial.ipynb).

## License
[BSD License](LICENSE.txt).

## Install GCC 4.9 or newers
### MAC
Unfortunately, for the MAC users, the project to support the OpenMP 4.0 specification in the Clang C language family front-end for the LLVM compiler is still going on. They don't fully support yet. Please refer to this [link](https://clang-omp.github.io/). However, you can install GCC 4.9 with Homebrew. It's tested on Yosemite and macOS Sierra.
```bash
brew update
brew install gcc49
```
If you have the following trouble to install GCC 4.9 due to no permission, please try to change the ownership and make a link again as follows. 
```bash
sudo chown -R $USER /usr/local/lib /usr/local/include /usr/local/bin /usr/local/Cellar /usr/local/share/
brew link gcc@4.9
```
### Linux/Unix
Basically, the most latest linux kernels (Debian:jessie, Ubuntu, CentOS, ...) support the GCC 4.9 or later. But if you don't have one, please refer to the below. If you have better ideas, please feel free to share that with us.
#### Debian:jessie
```bash
sudo apt-get update
sudo apt-get install gcc-4.9 g++-4.9
```
#### Ubuntu
```bash
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-4.9 g++-4.9
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 60 --slave /usr/bin/g++ g++ /usr/bin/g++-4.9
```
#### CentOS
```bash
sudo yum install libmpc-devel mpfr-devel gmp-devel

cd ~/Downloads
curl ftp://ftp.mirrorservice.org/sites/sourceware.org/pub/gcc/releases/gcc-4.9.2/gcc-4.9.2.tar.bz2 -O
tar xvfj gcc-4.9.2.tar.bz2

cd gcc-4.9.2
./configure --disable-multilib --enable-languages=c,c++
make -j 4

make install
```
### Windows
You can install GCC through [cygwin](https://cygwin.com/) now. Please refer to this [link](http://preshing.com/20141108/how-to-install-the-latest-gcc-on-windows/) to install [cygwin](https://cygwin.com/install.html) on windows.

## Troubleshooting
### 1. Install BSF but cannot import bsf in python prompt.
with some error message as follows:
```bash
ImportError: /path/to/anaconda3/lib/python3.6/site-packages/bsf.cpython-36m-x86_64-linux-gnu.so: undefined symbol: GOMP_parallel
```
That's because you don't have correct gmp or gomp library for supporting OpenMP. In general, you should have these when you install gcc through the stable package installation, such as apt-get, yum, and brew. Otherwise, you need to install these dependencies. In this case, you can just try to install gcc through conda package system. It assumes that you have installed anaconda for your python.

Please refer to the following commands.
```bash
# remove the bsf package first.
pip uninstall bsf
conda install -y gcc
```
And you can see it will install the following new dependent packages. 
```bash
    cloog: 0.18.0-0
    gcc:   4.8.5-7
    gmp:   6.1.0-0
    isl:   0.12.2-0
    mpc:   1.0.3-0
    mpfr:  3.1.5-0
```
