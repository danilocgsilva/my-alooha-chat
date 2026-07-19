FROM ubuntu:26.04

ENV LC_ALL="C.UTF-8"

ENV NVM_DIR=/home/myaloohachat/.nvm

RUN apt update
RUN apt install software-properties-common sudo openssh-server curl -y
RUN mkdir -p /run/sshd
RUN chmod 755 /run/sshd
RUN ssh-keygen -A
EXPOSE 22

RUN useradd -ms /bin/bash myaloohachat
RUN echo "myaloohachat:macpassw" | chpasswd
RUN usermod -aG sudo myaloohachat
RUN echo "myaloohachat ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN chown -R myaloohachat:myaloohachat /home/myaloohachat

USER myaloohachat
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash && \
    /bin/bash -c "source $NVM_DIR/nvm.sh && nvm install --lts && npm i -g typescript"

USER root

CMD ["/usr/sbin/sshd", "-D"]
