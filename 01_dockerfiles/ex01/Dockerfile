FROM debian

# PORT

EXPOSE 9987/udp
EXPOSE 30033
EXPOSE 10011

# environment

ENV TS3SERVER_LICENSE=accept

# CONFIG TEAMSPEAK

RUN apt-get update -y && apt-get upgrade -y
RUN apt-get install curl -y
RUN apt-get install bzip2 -y

#

RUN curl -O 'https://files.teamspeak-services.com/releases/server/3.9.0/teamspeak3-server_linux_amd64-3.9.0.tar.bz2'
RUN bzip2 -d teamspeak3-server_linux_amd64-3.9.0.tar.bz2
RUN tar -xf teamspeak3-server_linux_amd64-3.9.0.tar

# WORK DIR

WORKDIR teamspeak3-server_linux_amd64

ENTRYPOINT ["sh", "ts3server_minimal_runscript.sh"]
#docker run  -p 9987:9987/udp -p 10011:10011 -p 30033:30033 
