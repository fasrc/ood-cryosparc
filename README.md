# ood-cryosparc
Open OnDemand CryoSPARC app

## Prerequisites

* [Slurm cluster](https://osc.github.io/ood-documentation/latest/installation/resource-manager/slurm.html)
* [Apptainer](https://apptainer.org/) or [SingularityCE](https://sylabs.io/singularity/)
* CryoSPARC license
  - https://cryosparc.com/download

## Install

1. After obtaining a [CryoSPARC license](https://cryosparc.com/download), download the master and worker tarballs:

    version=4.7.1
    license_id=...CRYOSPARC_LICENSE_ID...
    curl -o cryosparc-master_v${version}.tar.gz -L https://get.cryosparc.com/download/master-v${version}/${license_id} 
    curl -o cryosparc-worker_v${version}.tar.gz -L https://get.cryosparc.com/download/worker-v${version}/${license_id} 

2. Build the SIF image (from the same working directory as the CryoSPARC master/worker tarballs):

    singularity build --fakeroot cryosparc_v${version}.sif Singularity.def

## Site-specific modifications

* [form.yml.erb](form.yml.erb) - replace `__CLUSTER__` with a valid 
* [template/script.sh.erb](template/script.sh.erb) - replace `__CRYOSPARC_SIF_PATH__` with the path to the cryosparc_vXXX.sif built in Install step 2


## Testing

1. Download the test data set to a filesystem accessible from all compute nodes:

    singularity exec --env CRYOSPARC_DB_PATH=/ --env CRYOSPARC_BASE_PORT=1 cryosparc_v4.7.1.sif cryosparcm downloadtest
    tar -xf empiar_10025_subset.tar

2. Launch the CryoSPARC OOD batch app
    * Allocate at least 1 GPU

3. [Verify the CryoSPARC Installation with the Extensive Validation Job](https://guide.cryosparc.com/setup-configuration-and-management/software-system-guides/tutorial-verify-cryosparc-installation-with-the-extensive-workflow-sysadmin-guide)
   - In the "Path to Dataset Data" textbox, enter the absolute path to the empiar_10025_subset directory extracted in step 1
