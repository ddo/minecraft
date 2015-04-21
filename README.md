# minecraft
my personal minecraft server

##installation

####install java

```sh
sudo apt-get update
sudo apt-get install default-jdk
```

####create server folder, switch into it

```sh
mkdir server
cd server
```

####download minecart server, change ``1.8.3`` to your version

```sh
wget -O minecraft_server.jar https://s3.amazonaws.com/Minecraft.Download/versions/1.8.3/minecraft_server.1.8.3.jar
```

####1 time starting, to let them generate init files then exit

> change ``350`` to your server RAM

```sh
java -Xms350M -Xmx350M -jar minecraft_server.1.8.3.jar nogui
```

####accept ``eula.txt``

```text
eula=true
```

####edit ``server.properties``, ``whitelist.json``, ``ops.json``

####setup upstart

> for auto start minecraft server after boot/reboot, auto restart in case your server crash as well

```sh
sudo nano /etc/init/minecraft.conf
```

> change ``/root/minecraft.log`` to your log file

> change ``/root/server/`` to your server location

```conf
#!upstart
description "minecraft server"
author      "ddo"

start on runlevel [2345]
stop on shutdown

respawn

chdir /root/server/

script
    java -Xms350M -Xmx350M -server -XX:+UseConcMarkSweepGC -XX:+AggressiveOpts -XX:+UseParNewGC -XX:ParallelGCThreads=2 -XX:+DisableExplicitGC -jar minecraft_server.1.8.3.jar nogui >> /root/minecraft.log
end script

pre-start script
    echo "[`date -u +%Y-%m-%dT%T.%3NZ`] [startup]: starting" >> /root/minecraft.log
end script

pre-stop script
    screen -S minecraft -p 0 -X quit
    echo "[`date -u +%Y-%m-%dT%T.%3NZ`] [startup]: stopping" >> /root/minecraft.log
end script
```

####start server

```sh
sudo start minecraft
```

####enjoy


##note

* minecraft default port: ``25565``

##starting scripts

* simple

```sh
java -Xms350M -Xmx350M -jar minecraft_server.1.8.3.jar nogui
```

* advance

```sh
java -Xms350M -Xmx350M -server -XX:+UseConcMarkSweepGC -XX:+AggressiveOpts -XX:+UseParNewGC -XX:ParallelGCThreads=2 -XX:+DisableExplicitGC -jar minecraft_server.1.8.3.jar nogui
```

* screen

```sh
screen -dmS "minecraft" java -Xms32M -Xmx450M -jar minecraft_server.1.8.3.jar nogui
```

quit

```sh
screen -S minecraft -p 0 -X quit
```


