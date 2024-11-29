FROM ubuntu:noble
LABEL org.opencontainers.image.maintainer="ueno.s <ueno.s@gamestudio.co.jp>"

# P4D configuration
ENV P4D_PORT            "ssl:perforce:1666"
ENV P4D_SUPER           "super"
ENV P4D_SUPER_PASSWD    "HelixDockerBay94"

# Swarm configuration
ENV SWARM_USER          "swarm"
ENV SWARM_PASSWD        "HelixDockerBay94"
ENV SWARM_MAILHOST      "localhost"
ENV SWARM_HOST          "helix.swarm"
ENV SWARM_REDIS         "redis"
ENV P4D_GRACE           "30"
ENV SWARM_FORCE_EXT     "n"

RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime \
    && apt-get update -y \
    && apt-get upgrade -y \
    && apt-get install -y wget gnupg less language-pack-ja-base language-pack-ja tzdata vim cron logrotate \
    && echo 'export LANG=ja_JP.UTF-8' >> ~/.bashrc \
    && echo 'export LANGUAGE="ja_JP:ja"' >> ~/.bashrc \
    && wget -qO - https://package.perforce.com/perforce.pubkey | apt-key add - \
    && echo "deb http://package.perforce.com/apt/ubuntu noble release" > /etc/apt/sources.list.d/perforce.list \
    && apt-get update && apt-get -y install helix-cli helix-swarm \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get autoremove -y


COPY Version /opt/perforce/etc/Docker-Version
COPY setup/swarm-docker-setup.sh /opt/perforce/swarm/sbin/swarm-docker-setup.sh
COPY setup/helix-swarm.p4-extension /opt/perforce/swarm/p4-bin/helix-swarm.p4-extension
RUN  chmod 755 /opt/perforce/swarm/sbin/swarm-docker-setup.sh

# Simple Swarm configuration
RUN echo http://localhost >> /opt/perforce/etc/swarm-cron-hosts.conf
RUN rm -f /etc/apache2/sites-enabled/000*

ENV P4DGRACE=$P4DGRACE

CMD /opt/perforce/swarm/sbin/swarm-docker-setup.sh
