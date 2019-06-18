# the base miniconda3 image
FROM ddebby/ai:v0.1.0

# load in the environment.yml file - this file controls what Python packages we install
ADD environment.yml /

# install the Python packages we specified into the base environment
RUN conda update -n base conda -y && conda env update

# install tensorflow packages we specified into the base environment
RUN pip install tensorflow==2.0.0-alpha0

# download the coder binary, untar it, and allow it to be executed
RUN wget https://github.com/cdr/code-server/releases/download/1.1156-vsc1.33.1/code-server1.1156-vsc1.33.1-linux-x64.tar.gz \
    && tar -xzvf code-server1.1156-vsc1.33.1-linux-x64.tar.gz && chmod +x code-server1.1156-vsc1.33.1-linux-x64/code-server

COPY docker-entrypoint.sh /usr/local/bin/

ADD ./code /code

ENTRYPOINT ["docker-entrypoint.sh"]
