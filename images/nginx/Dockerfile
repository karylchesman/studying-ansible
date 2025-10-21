FROM ubuntu:22.04

RUN apt update && \
    apt install -y openssh-server nginx sudo python3 && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

COPY html /var/www/html

RUN useradd -m -s /bin/bash ansible && \
    usermod -aG sudo ansible && \
    usermod -aG www-data ansible && \
    echo "ansible ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/ansible && \
    chmod 440 /etc/sudoers.d/ansible

RUN mkdir /var/run/sshd && \
    mkdir -p /home/ansible/.ssh && \
    chmod 700 /home/ansible/.ssh

RUN echo "alias python='/usr/bin/python3'" >> /home/ansible/.bashrc

COPY id_rsa.pub /home/ansible/.ssh/authorized_keys
RUN chmod 600 /home/ansible/.ssh/authorized_keys && \
    chown -R ansible:ansible /home/ansible/.ssh

RUN chown -R www-data:www-data /var/www/html && \
    chmod -R 775 /var/www/html

EXPOSE 22 80

CMD service nginx start && /usr/sbin/sshd -d
