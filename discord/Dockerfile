# Run discord in a container
#
# docker run -v /tmp/.X11-unix:/tmp/.X11-unix \
#	-v $HOME/.Skype:/home/skype/.Skype \
#	-e DISPLAY \
#	--device /dev/video0 \
#       --device /dev/snd \
#	--name discord \
#	discord

FROM debian:bullseye-slim

# Tell debconf to run in non-interactive mode
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y \
  apt-transport-https \
  ca-certificates \
  wget \
  gnupg \
  procps \
  libgconf-2-4 libappindicator1 \
  libgtk-3-0 libx11-xcb1 \
  --no-install-recommends

# Download the discord deb package
RUN wget https://dl.discordapp.net/apps/linux/0.0.11/discord-0.0.11.deb -O discord.deb
# Install discord
RUN apt install -yf ./discord.deb --no-install-recommends
# Remove old package
RUN rm ./discord.deb

COPY run-discord-and-wait-for-exit /usr/local/bin

# Make a user
ENV HOME /home/discord
RUN useradd --create-home --home-dir $HOME discord \
  && chown -R discord:discord $HOME \
  && usermod -a -G audio,video discord

WORKDIR $HOME
USER discord

# Start Discord
ENTRYPOINT ["run-discord-and-wait-for-exit"]
