FROM openjdk:latest

WORKDIR /minecraft

ADD https://launcher.mojang.com/v1/objects/a16d67e5807f57fc4e550299cf20226194497dc2/server.jar 	server.jar

RUN echo "eula=true" > eula.txt

# ENTRYPOINT [ "bash" ]
ENTRYPOINT [ "java" , "-Xmx1024M" , "-Xms1024M" , "-jar" , "server.jar" , "nogui" ]
