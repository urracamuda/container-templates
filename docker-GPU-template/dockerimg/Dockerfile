### COURTESY OF ANGEL'S WISDOWS AND 

### BASE IMAGE ###
# FIRST: check cuda version with: nvidia-smi 
# sudo ubuntu-drivers devices
# sudo ubuntu-drivers install (el driver más actual)
FROM nvidia/cuda:12.2.2-devel-ubuntu22.04

### SYSTEM UPDATE, CONFIGURATIONS AND PYTHON INSTALL ###

RUN apt-get update

# Install locales
# RUN apt-get update && apt-get install -y locales &&  \
#	&& localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
# ENV LANG en_US.utf8

# Use zsh in docker console
# RUN apt-get update
RUN apt-get install -y zsh wget curl
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.3/zsh-in-docker.sh)"
ENV SHELL=/bin/zsh
RUN sed -i '/^ZSH_THEME/c\ZSH_THEME="agnoster"' ~/.zshrc

# Giving root permissions to $USER
# Add non-root user (call this with the specific UID and GID of the host, to share permissions)
ARG UID=$UID
ARG GID=$GID
ARG USER=$USER

RUN apt-get install sudo
RUN addgroup --gid $GID $USER
RUN adduser --disabled-password --shell /usr/bin/bash   --gecos '' --uid $UID --gid $GID $USER

RUN adduser $USER sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers


# git global configuration
RUN apt-get install -y git
RUN git config --global pull.rebase true
RUN git config --global rebase.autoStash true

# Python general stuff

RUN apt-get install -y python3
RUN apt-get install -y python3-pip
RUN apt-get install -y python3-dev
RUN python3 -m pip install --upgrade pip

RUN ln -s /usr/bin/python3 /usr/bin/python




### INSTALL JUPYTER ###
RUN pip3 install jupyter
RUN pip3 install -U jupyterlab>4.0.6
RUN pip3 install jupyter-collaboration
RUN pip3 install -U jupyter_server>2.0.0


### OTHER JUPYTER STUFF ###
RUN pip3 install -U jedi-language-server
RUN pip3 install -U python-language-server
RUN pip3 install -U python-lsp-server


### PACKAGES ###
# General pip packages
RUN pip install --upgrade twine keyrings.alt pynvml fastgpu


### INSTALL PYTORCH ### 
# INSTRUCTIONS: https://pytorch.org/get-started/previous-versions/
RUN pip3 install torch torchvision torchaudio

### Place to install other python packages in the requirements.txt file###
COPY requirements.txt /home/$USER
RUN pip3 install -r requirements.txt


### Cleaning libraries
# RUN rm -rf /var/lib/apt/lists/*

USER $USER

### Setting the working directory ###
WORKDIR /home/$USER
RUN echo 'export PATH=$PATH:~/.local/bin' >> /home/$USER/.bashrc


### LIFT-OFF!! ###
CMD ["bash", "-c", "source ~/.bashrc && jupyter lab --ip 0.0.0.0 --no-browser --collaborative --allow-root --ContentsManager.allow_hidden=True"]