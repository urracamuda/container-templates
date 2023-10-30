### COURTESY OF ANGEL'S WISDOWS AND 

### BASE IMAGE ###
# FIRST: check cuda version with: nvidia-smi 
# sudo ubuntu-drivers devices
# sudo ubuntu-drivers install (el driver más actual)
FROM nvidia/cuda:12.2.2-devel-ubuntu22.04

# Add non-root user (call this with the specific UID and GID of the host, to share permissions)
ARG UID=$UID
ARG GID=$GID
ARG USER=$USER

RUN echo $UID
RUN echo $GID
RUN echo $USER


### SYSTEM UPDATE AND PYTHON INSTALL ###

# sometimes it works with root...somteimes, not
# USER root

RUN apt-get update
RUN apt-get install -y python3
RUN apt-get install -y python3-pip
RUN python3 -m pip install --upgrade pip
# RUN apt-get install -y git

RUN ln -s /usr/bin/python3 /usr/bin/python


# Install locales
#RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* \
#	&& localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
#ENV LANG en_US.utf8



### Change to ROOT user (don't know if it's needed) and set some permissions. Check if needed
RUN addgroup --gid $GID $USER
RUN adduser --disabled-password --shell /usr/bin/bash   --gecos '' --uid $UID --gid $GID $USER

# Giving root permissions to $USER

# RUN adduser $USER sudo
# RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers


# Change the ownership of the editable installs within the lib folder
# RUN mkdir /home/$USER/lib
# RUN chown -R $USER:$USER /home/$USER/lib


### INSTALL JUPYTER ###
RUN pip3 install jupyter
RUN pip3 install jupyterlab>4.0.6
RUN pip3 install jupyter-collaboration
RUN pip3 install jupyter_server>2.0.0


### OTHER STUFF ###
RUN pip3 install -U jedi-language-server
RUN pip3 install -U python-language-server
RUN pip3 install -U python-lsp-server

### INSTALL PYTORCH ### 
# https://pytorch.org/get-started/previous-versions/
RUN pip3 install torch torchvision torchaudio



USER $USER

### Setting the working directory ###
WORKDIR /home/$USER

RUN echo 'export PATH=$PATH:~/.local/bin' >> /home/$USER/.bashrc


# COPY requirements.txt /home/$USER
# RUN pip3 install -r requirements.txt

### LIFT-OFF!! ###
CMD ["bash", "-c", "source ~/.bashrc && jupyter lab --ip 0.0.0.0 --no-browser --collaborative --allow-root --ContentsManager.allow_hidden=True"]
