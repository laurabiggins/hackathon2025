**MAJIQ**

A doc describing how MAJIQ was installed and ran.

*INSTALLATION*

1. Created a conda environment just for Majiq
`conda create -n MAJIQ python=3.10`. Make sure to have python=3.10 otherwise it won't work. 
Followed steps as outlined by the bitbucket page. 

The `.yml` file of the venv can be found in this repo.

2. After successfully installing MAJIQ, try to run
`majiq` to double-check the installation was correct. If not, try to run
`export LD_LIBRARY_PATH=$HOME/install/htslib-1.13/lib:$LD_LIBRARY_PATH` and then again
`majiq`. You should see appearing the help message.