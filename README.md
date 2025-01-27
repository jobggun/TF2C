# Supported tags and respective `Dockerfile` links

- [`latest` (*Dockerfile*)](https://github.com/jobggun/TF2C/blob/master/Dockerfile)

# What is Team Fortress 2 Classic?

A re-imagining of the 2008 era of the original Team Fortress 2, adding in old features that were scrapped and working upon them or adding new content such as weapons and game modes.
This Docker image contains the dedicated server of the game.

> [TF2 Classic](https://tf2classic.com/)

<img src="https://wiki.teamfortress.com/w/images/thumb/8/8e/Team_Fortress_2_Classic.png/1200px-Team_Fortress_2_Classic.png" alt="Team Fortress 2 Classic.png">

# How to use this image

## Hosting a simple game server

Running on the *host* interface (recommended):<br/>
```console
$ docker run -d --net=host --name=tf2-dedicated -e SRCDS_TOKEN={YOURTOKEN} jobggun/tf2c
```

Running using a bind mount for data persistence on container recreation:
```console
$ mkdir -p $(pwd)/tf2-data
$ chmod 777 $(pwd)/tf2-data # Makes sure the directory is writeable by the unprivileged container user
$ docker run -d --net=host -v $(pwd)/tf2-data:/home/steam/tf-dedicated/ --name=tf2-dedicated -e SRCDS_TOKEN={YOURTOKEN} jobggun/tf2c
```

Running multiple instances (increment SRCDS_PORT and SRCDS_TV_PORT):
```console
$ docker run -d --net=host --name=tf2-dedicated2 -e SRCDS_PORT=27016 -e SRCDS_TV_PORT=27021 -e SRCDS_TOKEN={YOURTOKEN} jobggun/tf2c
```

`SRCDS_TOKEN` **is required to be listed & reachable;** [https://steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers)<br/><br/>
`SRCDS_WORKSHOP_AUTHKEY` **is required to use the workshop;** [https://steamcommunity.com/dev/apikey](https://steamcommunity.com/dev/apikey)<br/><br/>

**It's also recommended to use "--cpuset-cpus=" to limit the game server to a specific core & thread.**<br/>
**The container will automatically update the game on startup, so if there is a game update just restart the container.**

# Configuration

## Environment Variables

Feel free to overwrite these environment variables, using -e (--env): 
```dockerfile
SRCDS_TOKEN="changeme" (value is is required to be listed & reachable, retrieve token here: https://steamcommunity.com/dev/managegameservers)
SRCDS_RCONPW="changeme" (value can be overwritten by tf/cfg/server.cfg) 
SRCDS_PW="changeme" (value can be overwritten by tf/cfg/server.cfg) 
SRCDS_PORT=27015
SRCDS_TV_PORT=27020
SRCDS_IP="0" (local ip to bind)
SRCDS_FPSMAX=300
SRCDS_TICKRATE=66
SRCDS_MAXPLAYERS=14
SRCDS_REGION=3
SRCDS_HOSTNAME="New TF Server" (first launch only)
SRCDS_WORKSHOP_AUTHKEY="" (required to load workshop maps)
```
## Config
You can edit the config using this command:
```console
$ docker exec -it tf2-dedicated nano /home/steam/tf-dedicated/tf/cfg/server.cfg
```

If you want to learn more about configuring a TF2 server check this [documentation](https://wiki.teamfortress.com/wiki/Dedicated_server_configuration).

# Image Variants:
The `tf2c` images come in three flavors, each designed for a specific use case.

## `tf2c:latest`
This is the defacto image. If you are unsure about what your needs are, you probably want to use this one. It is a bare-minimum TF2 dedicated server containing no 3rd party plugins.<br/>

# Contributors
[![Contributors Display](https://badges.pufler.dev/contributors/jobggun/tf2c?size=50&padding=5&bots=false)](https://github.com/jobggun/tf2c/graphs/contributors)
