# vim:ft=dockerfile

# the base image is ubuntu
FROM ubuntu:20.04

# create basic working environment
RUN ln -snf /usr/share/zoneinfo/UTC /etc/localtime \
 && echo UTC > /etc/timezone \
 && apt-get update \
 && apt-get -y --no-install-recommends install \
    make \
    git \
    ca-certificates \
    gfortran \
    python3 \
    python3-pip \
 && rm -rf /var/lib/apt/lists/* \
 && ln -s /usr/bin/python3 /usr/bin/python

# use pip to install required packages
RUN pip3 install -I numpy scipy netcdf4 six matplotlib rasterio requests

# set up necessary environment variables
ENV PYTHON=python3 \
    FC=gfortran \
    MPLBACKEND=agg \
    PATH=/opt/geoclaw-landspill-cases-new/utilities:$PATH

# clone geoclaw-landspill-cases-new
RUN cd /opt \
 && git config --global http.sslVerify false \
 && git clone https://github.com/ENTRUSTSolutionsDevelopment/geoclaw-landspill-cases-new.git \
 && cd geoclaw-landspill-cases-new \
 && $PYTHON setup.py
 

# add a new non-root user and copy example case folders
RUN useradd -ms /bin/bash landspill \
 && chmod -R 777 /opt/geoclaw-landspill-cases-new \
 && cp -r /opt/geoclaw-landspill-cases-new/cases /home/landspill/landspill-examples \
 && chown landspill:landspill -R /home/landspill/landspill-examples

# change user to landspill
USER landspill
WORKDIR /home/landspill

# default command
CMD ["/bin/bash"]


