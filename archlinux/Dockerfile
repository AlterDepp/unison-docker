FROM archlinux:base
MAINTAINER stefan

ARG USER
ARG PASSWD
ARG GROUP=${USER}
ARG USER_UID=1000
ARG USER_GID=1000
ARG USERS_GID=100
ARG SUDO_GID=27

# Install and configure sudo
RUN pacman -Sy --noconfirm sudo
RUN groupadd sudo --gid ${SUDO_GID}
RUN echo "%sudo ALL=(ALL:ALL) ALL" > /etc/sudoers.d/sudo

# Change gid for group users from 984 to 100
RUN groupmod -n users_orig users
RUN groupadd --gid ${USERS_GID} users

# Add user with sudo privileg
RUN groupadd ${GROUP} --gid ${USER_GID}
RUN useradd --system --create-home --home-dir /home/${USER} --shell /bin/bash --gid ${USER_GID} --groups sudo,users --uid ${USER_UID} --password "`openssl passwd -1 ${PASSWD}`" ${USER}

# Install, configure and start sshd
RUN pacman -S --noconfirm openssh
RUN ssh-keygen -A

# Install unison
RUN pacman -S --noconfirm unison

# Start sshd
ENTRYPOINT ["sh", "/bin/entrypoint.sh"]

# Stay here forever
CMD tail -f /dev/null
