
# nodejs-debian-jessie
# Dockerfile
# using debian:jessie for it's smaller size over ubuntu
FROM debian:jessie

# TODO: Put the maintainer name in the image metadata
# MAINTAINER Nishant Sahay <nsahaytech@gmail.com>

# Replace shell with bash so we can source files
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# Set environment variables
ENV appDir /usr/src/app

# Run updates and install deps
RUN apt-get update

RUN apt-get install -y -q --no-install-recommends \
    apt-transport-https \
    build-essential \
    ca-certificates \
    curl \
    g++ \
    gcc \
    git \
    make \
    nginx \
    sudo \
    wget \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get -y autoclean

ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 7.8.0
ENV STI_SCRIPTS_PATH="/usr/local/s2i"

# Install nvm with node and npm
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.29.0/install.sh | bash \
    && source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# Set up our PATH correctly so we don't have to long-reference npm, node, &c.
ENV NODE_PATH $NVM_DIR/versions/node/v$NODE_VERSION/lib/node_modules
ENV PATH      $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

# Set the work directory
RUN mkdir -p /usr/src/app  
WORKDIR ${appDir}

# Install pm2 so we can run our application
RUN npm i -g pm2

# Add application files
ADD . /usr/src/app

#Expose the port
EXPOSE 8080

LABEL io.openshift.s2i.scripts-url=image:///usr/local/s2i
# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

CMD ["usage"]


