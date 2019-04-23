#BASE
FROM ubuntu:18.04
ENV DEBIAN_FRONTEND=noninteractive 
ENV TERM=xterm 
# to make sure we don't need to answer questions
RUN apt-get install -y  
RUN apt-get update
# set up locale
RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/*     && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 
ENV LANG en_US.utf8
# some basic packages
RUN apt-get update -qq \
    && apt-get install -y -qq \
    sudo \
    tmux \
    wget \
    git \
    zip \
    unzip \
    vim
    

ARG USER=ovinogradov 
#ARG HOME=/home/ovinogradov/
RUN useradd -m -d /home/$USER -u 6746 $USER && \
    chown -R $USER home/$USER && \
    adduser $USER sudo && \
    adduser $USER users && \
    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

#RUN echo "user ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/ovinograd
#RUN chmod 0440 /etc/sudoers.d/ovinogradov


USER $USER
WORKDIR /home/$USER

RUN mkdir /home/$USER/share
#Install basic packages
#RUN sudo apt-get install -y -qq  python 
RUN sudo apt-get -y install python3-pip
#RUN sudo apt-get -y install vim
RUN echo 'alias python=python3' >> ~/.bashrc

#RUN sudo pip3 install pip3 --upgrade
RUN sudo python3 -m pip install  jupyter
RUN sudo python3 -m pip install --upgrade pip
RUN sudo python3 -m pip install jupyterlab\
    numpy\
    scipy\
    sortedcontainers\ 
    matplotlib\
    seaborn\
    cython\
    networkx\
    wheel\
    nose\
    numba\
    h5py
#Insall NEST dependencies
RUN sudo apt-get install -y build-essential\
    cmake libltdl7-dev libreadline6-dev \
    libncurses5-dev libgsl0-dev libtool-bin\
    openmpi-bin libopenmpi-dev


RUN mkdir nest-build &&\
    mkdir nest-install
#Install NEST

RUN git clone https://github.com/nest/nest-simulator.git
WORKDIR /home/$USER/nest-build
RUN cmake  -DCMAKE_INSTALL_PREFIX:PATH=/home/$USER/.local \
    /home/$USER/nest-simulator
#carefull with j
RUN make -j 20
RUN make install
RUN sudo echo 'source /home/ovinogradov/.local/bin/nest_vars.sh' >> ~/.bashrc

# Install NESTML
WORKDIR /home/$USER
RUN git clone  https://github.com/nest/nestml.git
WORKDIR /home/$USER/nestml
RUN sudo python3 -m pip install -r requirements.txt 
RUN sudo python3 setup.py install 
WORKDIR /home/$USER
RUN python3 -m pip install --user git+https://github.com/Silmathoron/NNGT.git
# some additional things to make nestml work
RUN sudo apt-get install -y python
RUN echo 'export LD_LIBRARY_PATH=/home/ovinogradov/.local/lib/' >> ~/.bashrc
# TODO NEED TO SET UID,GID and groups correctly {{{{
# Expose ports
EXPOSE 8888
EXPOSE 8080
#ENTRYPOINT ["tini", "-g", "--"]
CMD [bash]
