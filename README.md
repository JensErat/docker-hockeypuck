# Hockeypuck for Docker

(c) 2015 Jens Erat <email@jenserat.de>

Redistribution and modifications are welcome, see the LICENSE file for details.

[Hockeypuck](http://hockeypuck.github.io/) is an OpenPGP public keyserver.  This keyserver implements the HKP draft protocol specification as well as several extensions to the protocol supported by SKS.

This Dockerfile provides an Hockeypuck image based on the Ubuntu packages provided in the Hockeypuck private package archive.

## Setup

This image exports two volumes at the default locations, `/etc/hockeypuck` for the configuration and `/var/lib/hockeypuck/recon-ptree` for reconciliation data. A separate PostgreSQL database must be linked. Port `11370` is exposed as the default SKS reconcilation port, port `11371`serves the HTTP server.

### Configuration

Follow the default Hockeypuck configuration throughout the [official configuration manual](http://hockeypuck.github.io/doc.html). Make sure to prepoluate Hockeypuck before starting reconciliation with other servers.

## Running a Container

The most basic run command would be:

docker run -d \
        --name hockeypuck \
        --publish 11370:11370 \
        --publish 11371:11371 \
        --link hockeypuck-postgresql:db \
        --volume /srv/hockeypuck/etc:/etc/hockeypuck \
        --volume /srv/hockeypuck/recon-ptree:/var/lib/hockeypuck/recon-ptree \
        jenserat/hockeypuck

## Upgrading and Maintenance

Hockeypuck performs database maintenance on startup and will very likely not require manual upgrade interventions.

To enter the container and perform maintenance, use (given you named the container `hockeypuck`):

    docker exec -ti hockeypuck /bin/bash
