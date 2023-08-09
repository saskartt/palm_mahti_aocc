# AMD Optimizing Fortran Compiler (AOCC) configurations for PALM on CSC's Mahti supercomputer

This repository contains necessary scripts to compile the [PALM model system](https://palm.muk.uni-hannover.de/) on the [Mahti supercomputer](https://docs.csc.fi/computing/systems-mahti/) using [AMD Optimizing Fortran Compilers (AOCC)](https://www.amd.com/en/developer/aocc.html).

## Installation
1. Copy the environment script `env_palm_mahti_aocc` from this repository into your home directory.
2. Open the script, on the first line replace `project_XXXXXXX` with your project identifier and save.
3. Source the environment using `source $HOME/env_palm_mahti_aocc`.
4. Download the PALM model system from the official [Gitlab server](https://gitlab.palm-model.org/releases/palm_model_system/-/releases).
5. Extract the contents of the .zip or .tar.gz archive to `$PALM_ROOT/sources/`.
6. Move to the source directory `cd $PALM_ROOT/sources/`.
7. Replace the `install` script in the source directory with the one from this repository. This adds `srun` as an alternative dependency for `mpirun`.
8. Compile auxillary programs by running `bash install -p ../inst`. Compilation of the actual PALM sources fails at this stage due to incorrect compiler configuration, but you may ignore this.
9. Create a run directory `mkdir $PALM_ROOT/run/` and `cd $PALM_ROOT/run/` into it.
10. Copy the configuration file `palm.config.mahti-aocc` from this repository into the run directory as `$PALM_ROOT/run/.palm.config.mahti-aocc`. Notice the dot in the front, i.e. this should be a hidden file.
11. Test building the PALM sources by running  `palmbuild -c mahti-aocc` at `$PALM_ROOT/run/`.
12. If everything OK, you can continue to running PALM jobs.

## Running jobs

1. Move necessary inputs to a respective job directory (`$PALM_ROOT/run/JOBS/$run_identifier/`)
2. Source the environment using `source $HOME/env_palm_mahti_aocc`
4. `cd` into the run directory `$PALM_ROOT/run/`
5. Test compilation and batch job file creation using `palmrun` with the option `-F`, e.g. `palmrun -b -r example_cbl -a "d3#" -c "mahti-aocc" -X "4" -T "4" -q test -m 1000 -t 500 -F`
5. If everything OK, run `palmrun` again without the option `-F`

## Author
Sasu Karttunen \
<sasu.karttunen@helsinki.fi> \
University of Helsinki
