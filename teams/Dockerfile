# Run teams in a container
#
# docker run -v /tmp/.X11-unix:/tmp/.X11-unix \
#	-e DISPLAY \
#	-v $HOME/.config/Microsoft:/home/teams/.config/Microsoft \
#	--device /dev/video0 \
#       --device /dev/snd \
#	--name teams \
#	teams

FROM debian:bullseye-slim

# Tell debconf to run in non-interactive mode
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y \
  apt-transport-https \
  ca-certificates \
  sudo \
  wget \
  gnupg \
  procps \
# libasound2 libatk-bridge2.0-0 libatk1.0-0 libatspi2.0-0 libc6 libcairo2 libcups2 libexpat1 libgcc1 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libnss3 libpango-1.0-0 libpangocairo-1.0-0 libsecret-1-0 libuuid1 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 gnome-keyring apt-transport-https libfontconfig1 libdbus-1-3 libstdc++6
  libsecret-1-0 \
  --no-install-recommends

# Download the teams deb package
RUN wget https://packages.microsoft.com/repos/ms-teams/pool/main/t/teams/teams_1.3.00.16851_amd64.deb -O teams.deb
# Install teams
RUN apt install -yf ./teams.deb --no-install-recommends
# Remove old package
RUN rm ./teams.deb

COPY run-teams-and-wait-for-exit /usr/local/bin

# Make a user
ENV HOME /home/teams
RUN useradd --create-home --home-dir /home/teams teams \
  && mkdir -p "/home/teams/.config/Microsoft/Microsoft Teams/logs" && chown -R teams:teams /home/teams/ \
  && usermod -a -G audio,video teams

WORKDIR $HOME
USER teams

# Start teams
ENTRYPOINT ["run-teams-and-wait-for-exit"]
