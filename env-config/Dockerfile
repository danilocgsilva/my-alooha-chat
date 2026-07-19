FROM ubuntu:26.04

# user and ssh
ENV LC_ALL="C.UTF-8"

RUN apt update
RUN apt install software-properties-common sudo openssh-server -y
RUN mkdir -p /run/sshd
RUN chmod 755 /run/sshd

RUN ssh-keygen -A

RUN useradd -ms /bin/bash moninmout
RUN echo "moninmout:moninmoutpass" | chpasswd
RUN usermod -aG sudo moninmout
RUN echo "moninmout ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN chown -R moninmout:moninmout /home/moninmout

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
