# TileDrawer setup

## What is it?

A setup script for serving OpenStreetMap tiles (= square-shaped pieces of geographic maps) on your own Amazon EC2 Ubuntu instance.

## How to use it?

* Go to http://tiledrawer.com/ and configure your map (including style and location)
* Open the text file, presented under `3. Start Your Instance`, in a new tab. It should look similar to this:

```bash
(
apt-get -y install git curl htop
git clone -b config http://tiledrawer.com/.git/ /usr/local/tiledrawer
/usr/local/tiledrawer/setup.sh
/usr/local/tiledrawer/populate.py -b NUMBER_LAT_1 NUMBER_LONG_1 NUMBER_LAT_2 NUMBER_LONG_2 -s https://path.to/your/style.cfg http://download.geofabrik.de/osm/continent/country/state/region.osm.pbf http://download.geofabrik.de/osm/continent/country/state/region.osm.pbf http://download.geofabrik.de/osm/continent/country/state/region.osm.pbf
/usr/local/tiledrawer/draw.sh
) >> /var/log/tiledrawer.log 2>&1
```

* Copy the whole script to a text editor
* Add `#!/bin/sh -ex` as the first line
* Change `http://tiledrawer.com/.git/` (line 3) to `https://github.com/mikeschaekermann/tiledrawer-setup.git`
* Change all occurences of `http://download.geofabrik.de/osm/continent/country/state/region.osm.pbf` (3 times in the example above) to `http://download.geofabrik.de/continent/country/state/region-latest.osm.pbf` (i.e. remove the `/osm` part and add a `-latest` after the `region` and before the `.osm.pbf`)
* Use the resulting script as the user-data of the launching EC2 instance (these instructions were tested for a [ami-6fa27506](https://console.aws.amazon.com/ec2/home?region=us-east-1#launchAmi=ami-6fa27506) m1.large instance)
* Launch your instance
* Go to your instance's root URL and wait!

## How to monitor the log files?

* Log into your instance
* `tail -fn 30 /var/log/tiledrawer.log` for the setup log file
* `tail -fn 30 /var/log/tiledrawer-gunicorn.log` for the server log file
