FROM benok/os-ubuntuconsole-base
# FROM amd64=ubuntu:latest arm64=arm64v8/ubuntu:latest

COPY build/sshd_config.append.tpl /etc/ssh/
COPY build/generate-lsb-release /etc/init.d/

RUN apt-get update \
    && apt-get install -y --no-install-recommends fdisk iptables openssh-server rsync locales \
                           sudo vim less curl ca-certificates psmisc htop kmod iproute2 \
                           net-tools bash-completion iputils-ping wget \
                           apparmor \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /etc/ssh/*key* \
    && locale-gen en_US.UTF-8 \
    && addgroup --gid 1100 rancher \
    && addgroup --gid 1101 docker \
    && useradd -u 1100 -g rancher -G docker,sudo -m -s /bin/bash rancher \
    && useradd -u 1101 -g docker -G docker,sudo -m -s /bin/bash docker \
    && echo '## allow password less for rancher user' >> /etc/sudoers \
    && echo 'rancher ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers \
    && echo '## allow password less for docker user' >> /etc/sudoers \
    && echo 'docker ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers \
    && cat /etc/ssh/sshd_config > /etc/ssh/sshd_config.tpl \
    && cat /etc/ssh/sshd_config.append.tpl >> /etc/ssh/sshd_config.tpl \
    && rm -f /etc/ssh/sshd_config.append.tpl /etc/ssh/sshd_config
ENTRYPOINT ["/usr/bin/ros", "entrypoint"]
