## Installation Guide

Obtaining packages and handling interdependencies is easiest if you use a Python distribution, such as [Anaconda](https://www.continuum.io/downloads) or [Canopy](https://www.enthought.com/products/canopy).
Although the selection of maintained packages is smaller than if you use the `pip` command to obtain packages from the Python Package Index (PyPi), you can still use `pip` with these distributions (but always prefer to use the distribution's install mechanism over `pip`, since `pip` does not handle interdependencies well).

The other advantage to these distributions is that they easily install without system administrative privileges (i.e., in a user directory) and come with the non-Python libraries upon which many Python modules rely, making them ideal for setup on e.g. clusters.

### Requirements

To install this package, you'll need to have the following non-python requirements.
Note that these are not installed automatically, and you must install them yourself prior to installing PISA.
Also not that both SWIG and HDF5 support come pre-packaged in the Anaconda and Canopy Python distributions.
* [git](https://git-scm.com/)
* [swig](http://www.swig.org/)
* [hdf5](http://www.hdfgroup.org/HDF5/) — install with `--enable-cxx` option

If you are *not* using Anaconda or Canopy, you can install the above via:
* In Ubuntu,
  'sudo apt-get install git swig hdf5`

The Python requirements are as follows; note that all *required* Python modules (i.e., everything besides Python and pip) are installed automatically when you install PISA with the `pip ... -r $PISA/requirements.txt` option detailed later.
* [python](http://www.python.org) — version 2.7.x required (tested with 2.7.11)
* [pip](https://pip.pypa.io/)
* [numpy](http://www.numpy.org/)
* [scipy](http://www.scipy.org/) — version > 0.12 recommended
* [h5py](http://www.h5py.org/)
* [pytables](http://www.pytables.org/)
* [cython](http://cython.org/)
* [uncertainties](https://pythonhosted.org/uncertainties/)
* [pint](https://pint.readthedocs.org/en/0.7.2/)

Optional dependencies, on the other hand, must be installed manually prior to installing PISA.
The features enabled by and the relevant install commands for optional dependencies are listed below.
* To parallelize certain routines on multi-core computers<br>
  [openmp](http://www.openmp.org)<br>
  (available by compiler; gcc supports OpenMP this, while Clang does not)
* For running certain routines on Nvidia CUDA (>= compute 2.0) GPUs<br>
  [PyCUDA](https://mathema.tician.de/software/pycuda)<br>
  `pip install pycuda`
* Just-in-time compilation via LLVM to accelerate certain routines<br>
  [numba](http://numba.pydata.org)
  * Anaconda:<br>
    `conda install numba`
  * pip:<br>
    `pip install numba`
* For detailed profiling output<br>
  [line_profiler](https://pypi.python.org/pypi/line_profiler/)
  * Anaconda:<br>
    `conda install line_profiler`
  * pip:<br>
    `pip install line_profiler`
* For compiling the Sphinx documentation<br>
  * [Sphinx](http://www.sphinx-doc.org/en/stable/) - version > 1.3
    * Anaconda:<br>
      `conda install sphinx`
    * pip:<br>
      `pip install sphinx`
  * [recommonmark](http://recommonmark.readthedocs.io/en/latest/)<br>
    `pip install recommonmark`

### Install Python
There are many ways of obtaining Python and many ways of installing it.
Here we'll present two options, but this is by no means a complete list.

* Install Python 2.7.x from the Python website [https://www.python.org/downloads](https://www.python.org/downloads/)
* Install Python 2.7.x from the Anaconda distribution following instructions [here](https://docs.continuum.io/anaconda/install)

### Set up your environment
* Create a "parent" directory (the directory into which you wish for the PISA sourcecode to live).
Note that subsequent steps will create a directory named `pisa` within the parent directory you've chosen, so you don't need to create the actual `pisa` directory yourself.
```
mkdir -p <parent dir>
```

* To make life easier in the future (and to make these instructons easy to follow), define the environment variable `PISA`.
E.g., for the bash shell, edit your `.bashrc` file and add the line
```
export PISA=<parent dir>/pisa
```
Load this variable into your *current* environment by sourcing your `.bashrc` file:
```bash
. ~/.bashrc
```
(it will be loaded autmatically for all new shells).

### Github setup
1. Create your own [github account](https://github.com/)
1. Obtain access to the `WIPACrepo/pisa` repository by emailing Sebastian Böeser [sboeser@uni-mainz.de](mailto:sboeser@uni-mainz.de?subject=Access to WIPACrepo/pisa github repository)

#### SSH vs. HTTPS access to repository
You can interact with Github repositories either via SSH (which allows password-less operation) or HTTPS (which gets through firewalls that don't allow for SSH).
To choose one or the other just requires a different form of the repsitory's URL (the URL can be modified later to change method of access if you change your mind).

##### Set up password-less access (SSH)
If you use the SSH URL, you can avoid passwords altogether by uploading your public key to Github:

1. [Check for an existing SSH key](https://help.github.com/articles/checking-for-existing-ssh-keys/)
1. [Generate a new SSH key if none already exists](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
1. [Add your SSH key to github account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account)
1. [Test the ssh connection](https://help.github.com/articles/testing-your-ssh-connection)

##### Set up password caching (SSH or HTTPS)
Git 1.7.10 and later allows storing your password for some time in memory so you aren't asked every time you interact with Github via the command line.
Follow instructions [here](https://help.github.com/articles/caching-your-github-password-in-git/).
This is particularly useful for HTTPS or if you use SSH but do not wish to store a key pair on the computer/server you use.

### Obtain PISA sourcecode

#### Developing PISA: Forking
If you wish to modify PISA and contribute your code changes back to the PISA project (*highly recommended!*), fork `WIPACrepo/pisa` from Github.
*(How to work with the `cake` branch of PISA will be detailed below.)*

Forking creates your own version of PISA within your Github account.
You can freely create your own *branch*, modify the code, and then *add* and *commit* changes to that branch within your fork of PISA.
When you want to share your changes with `WIPACrepo/pisa`, you can then submit a *pull request* to `WIPACrepo/pisa` which can be merged by the PISA administrator (after the code is reviewed and tested, of course).

* Navigate to the [PISA github page](https://github.com/wipacrepo/pisa) and fork the repository by clicking on the ![fork](images/ForkButton.png) button.
* From a terminal, change into the "parent" directory.<br>
`cd <parent dir>`
* Clone the repository via one of the following commands (`<github username>` is your Github username):
  * either SSH access to repo:<br>
`git clone git@github.com:<github username>/pisa.git`
  * or HTTPS access to repo:<br>
`git clone https://github.com/<github username>/pisa.git`

#### Using but not developing PISA: Cloning
If you just wish to pull changes from github (and not submit any changes back), you can just clone the sourcecode without creating a fork of the project.

* From a terminal, change into the "parent" directory.<br>
`cd <parent dir>`
* Clone the WIPACrepo/pisa repository via one of the following commands:
  * either SSH access to repo:<br>
`git clone git@github.com:wipacrepo/pisa.git`
  * or HTTPS access to repo:<br>
`git clone https://github.com/wipacrepo/pisa.git`

### Install PISA
```bash
pip install --editable $PISA -r $PISA/requirements.txt
```
Explanation of the above command:
* `--editable <dir>`: Installs from `<dir>` and  allows for changes to the source code within to be immediately propagated to your Python installation.
Basically, within your Python source tree, PISA is just a link to your source directory, so changes within your source tree are seen directly by your Python installation.
* `-r $PISA/requirements.txt`: Specifies the file containing PISA's dependencies for `pip` to install prior to installing PISA.
This file lives at `$PISA/requirements.txt`.

* `--src $PISA`: Installs PISA from the sourcecode you just cloned in the directory pointed to by the environment variable `$PISA`.

__Notes:__

* You can work with your installation using the usual git commands (pull, push, etc.).
However, these won't recompile any of the extension (i.e. _C/C++_) libraries.
If you want to do so, simply run<br>
`cd $PISA && python setup.py build_ext --inplace`

* If your Python installation was done by an administrator, if you have administrative access, preface the `pip install` command with `sudo`:<br>
`sudo pip install ...`<br>
If you do not have administrative access, you can install PISA as a user module via the `--user` flag:<br>
`pip install --user ...`

### Compiling the Docs

To generate a new version of the documentation simply go to `$PISA/docs` and excecute `make html` to compile. (Alternatively also other output formats like pdf documents can be generated)

in case code structure changed, rebuild the apidoc by executing `sphinx-apidoc -f -o docs/source pisa` from the pisa root dir `$PISA`.