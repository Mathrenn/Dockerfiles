# Run eclipse in a container
#
# docker build -t eclipse \
#  --build-arg USER_ID=$(id -u) \
#  --build-arg GROUP_ID=$(id -g) .
#
# docker run -it \
#	-v /tmp/.X11-unix:/tmp/.X11-unix \ # mount the X11 socket
#	-e DISPLAY=unix$DISPLAY \
#	-v $HOME/eclipse-workspace:/home/eclipse/workspace/\
#	-v $HOME/.eclipse/:/home/eclipse/.eclipse \
#	-v /dev/shm:/dev/shm \
#       -p 8080:8080
#	--name eclipsejee \
#	eclipsejee:0.1

# Base docker image
FROM adoptopenjdk:11-jre-openj9

ARG USER_ID
ARG GROUP_ID

# Install Dependencies
RUN apt-get update && apt-get install -y \
    wget \
    sudo \
    apt-utils \
    libxext-dev libxrender-dev libxtst-dev \
    libgtk2.0-0 libcanberra-gtk-module \
    libswt-gtk-4-java \
    openjdk-8-jre \
    openjdk-11-jre \
    --no-install-recommends

# Install eclipse
RUN wget http://ftp.halifax.rwth-aachen.de/eclipse//technology/epp/downloads/release/2020-06/R/eclipse-jee-2020-06-R-linux-gtk-x86_64.tar.gz -O /opt/eclipse.tar.gz && \
    cd /opt/ && \
    tar -xzf /opt/eclipse.tar.gz && \
    rm /opt/eclipse.tar.gz

# Add eclipse user
RUN addgroup --gid $GROUP_ID eclipse && adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID eclipse && \
    mkdir -p /home/eclipse/.eclipse && chown -R  $USER_ID:$GROUP_ID /home/eclipse/.eclipse && \
    mkdir -p /home/eclipse/workspace && chown -R $USER_ID:$GROUP_ID /home/eclipse/workspace

RUN chmod +x /opt/eclipse/eclipse && \
    mkdir -p /home/eclipse && \
    echo "eclipse:x:1000:1000:eclipse,,,:/home/eclipse:/bin/bash" >> /etc/passwd && \
    echo "eclipse:x:1000:" >> /etc/group && \
    echo "eclipse ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/eclipse && \
    chmod 0440 /etc/sudoers.d/eclipse && \
    chown $USER_ID:$GROUP_ID -R /home/eclipse && \
    chown root:root /usr/bin/sudo && chmod 4755 /usr/bin/sudo

# Run as non privileged user
USER eclipse

ENTRYPOINT [ "/opt/eclipse/eclipse" ]
CMD [ "--user-data-dir=/data" ]
