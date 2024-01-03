# Antennas on Docker

[Antennas](https://github.com/thejf/antennas) is a Crystal port of tvhProxy which is a program that translates the Tvheadend API to emulate a HDHomeRun API. This is particularly useful to connect Plex's DVR feature to Tvheadend.

## Usage
```
docker create \
  --name=antennas \
  -v <path/to/config>:/opt/antennas/config \
  -p 5004:5004 \
  thejf/antennas
```

This will mount a volume, set by yourself in path/to/config, that will need a config.yml to work. Example of a config.yml is [available here](https://github.com/TheJF/antennas/blob/master/config/config.yml), or below:
```
tvheadend_url: http://replace:me@x.x.x.x:9981
tvheadend_weight: 300
tuner_count: 6
```

Note, I'll eventually make this work with environment variables, Crystal is just not cooperating with me on this at the moment.

Example of a docker compose file
```
version: "1"
services:
  antennas:
    image: thejf/antennas:latest
    container_name: antennas
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Australia/Brisbane #replace with local timezone info
      - ANTENNAS_URL=192.168.0.1:5004 #replace 192.168.0.1 with IP address of docker server hosting antennas
      - TVHEADEND_URL=http://username:password@192.168.1.2:9981/ #replace 192.168.1.2 with IP/hostname of tvheadend server
      - TUNER_COUNT=1 #enter number of tuners
    volumes:
      - /path/to/docker/config:/opt/antennas/config #replace /path/to/docker/config with your path to the config.yml file
    ports:
      - 5004:5004
    restart: unless-stopped
```

