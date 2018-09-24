Bootstrap: docker
From: nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

%environment

PATH=/opt/conda/bin:$PATH
export PATH
  
%post
  
# Set up the PATH for other commands
export PATH=/opt/conda/bin:$PATH

# Update list of packages then upgrade them
apt-get update
DEBIAN_FRONTEND=noninteractive apt-get -y upgrade

# Workaround for https://github.com/keras-team/keras/issues/9567
apt-get install -y --allow-downgrades --no-install-recommends \
    --allow-change-held-packages \
    libcudnn7=7.0.5.15-1+cuda9.0 libcudnn7-dev=7.0.5.15-1+cuda9.0 
        
# Install dependencies
apt-get install -y --no-install-recommends build-essential cmake git curl vim ca-certificates libjpeg-dev libpng-dev
# Clean up
rm -rf /var/lib/apt/lists/*
    
# Install miniconda
cd /opt
curl -o /opt/miniconda.sh -O  https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod +x /opt/miniconda.sh
/opt/miniconda.sh -b -p /opt/conda 
rm /opt/miniconda.sh

# Install some packages
conda install numpy pyyaml scipy ipython mkl mkl-include
conda install -c soumith magma-cuda90
conda clean -ya
    
# Install the released version of Pytorch.
conda install pytorch torchvision cuda90 -c pytorch

# For integration with JupyterHub
pip install --no-cache-dir ipykernel
    
echo 'export PATH=/opt/conda/bin:$PATH' >>$SINGULARITY_ENVIRONMENT
