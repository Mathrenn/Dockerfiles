# PSP emulator in a container
#
# docker run --rm -d \
# 	--device /dev/snd \
# 	-v /tmp/.X11-unix:/tmp/.X11-unix \
# 	-v $PWD:/games \
# 	-e DISPLAY=unix$DISPLAY \
# 	--device /dev/dri \
# 	ppsspp /games/<game.iso>

FROM debian:sid-slim

RUN apt-get update && apt-get install -y \
	binutils \
	ca-certificates \
        libsdl2-dev \
        libgl1-mesa-dev \
        libgl1-mesa-dri \
        mesa-utils \
        unzip \
	--no-install-recommends \
	&& rm -rf /var/lib/apt/lists/* \
	&& ldconfig

ADD https://build.ppsspp.org/builds/Linux/amd64/ppssppbuildbot-org.ppsspp.ppsspp-dev-latest-linux-amd64.zip /opt/ppsspp.zip

RUN unzip /opt/ppsspp.zip -d /opt/ppsspp && rm /opt/ppsspp.zip && chmod +x /opt/ppsspp/PPSSPPSDL

ENTRYPOINT [ "/opt/ppsspp/PPSSPPSDL" ]
