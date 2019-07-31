FROM ubuntu:16.04
WORKDIR /

RUN echo 'root:qwertyui' | chpasswd

RUN apt-get update
RUN apt-get install -y wget openssh-server ca-certificates postfix
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh \
	&& chmod 777 script.deb.sh && ./script.deb.sh
RUN apt-get update && EXTERNAL_URL="http://gitlab.example.com" apt-get install -yq gitlab-ce

RUN apt update && apt install -y tzdata
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /etc/gitlab/ssl
RUN chmod 700 /etc/gitlab/ssl
RUN openssl req -x509 -nodes -days 365 -newkey rsa:4096 \
		-subj /C=BE/ST=bxl/L=bxl/O=bxl/OU=rs1/CN=192.168.99.101/emailAddress=ghamelek@student.s19.be \
		-keyout /etc/gitlab/ssl/selfsigned.key -out /etc/gitlab/ssl/selfsigned.crt


RUN echo "external_url \"https://192.168.99.101\"" >> /etc/gitlab/gitlab.rb
RUN echo "nginx['redirect_http_to_https'] = true " >> /etc/gitlab/gitlab.rb
RUN echo "nginx['ssl_certificate'] = \"/etc/gitlab/ssl/selfsigned.crt\"" >> /etc/gitlab/gitlab.rb
RUN echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/selfsigned.key\"" >> /etc/gitlab/gitlab.rb
RUN echo "gitlab_rails['gitlab_shell_ssh_port'] = 2222" >> /etc/gitlab/gitlab.rb

ENTRYPOINT service ssh restart && (/opt/gitlab/embedded/bin/runsvdir-start &) \
				&& gitlab-ctl reconfigure && gitlab-ctl tail

#docker build -t ex03 .
#docker run --name ex03 -it -p 80:80 -p 2222:22 -p 443:443 --privileged ex03

#for HTTPS acces with ssl 
#GIT_SSL_NO_VERIFY=true  (git clone or git push )

#for ssh acces => 
# pbcopy < ~/.ssh/id_rsa.pub
