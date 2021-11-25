# sbfspot2influxdb
Write PV data from sbfspot2 to an InfluxDB

My environment:

- Raspberry Pi 3 Model B Rev 1.2
- Raspbian 32 bit
- Installed sbfspot
  - [Official documenation](https://github.com/SBFspot/SBFspot/wiki/Installation-Linux-SQLite)
  - `curl -s https://raw.githubusercontent.com/sbfspot/sbfspot-config/master/sbfspot-config` piped into bash

sbfspot is called every 5 minutes by cron.

After running some time, the database can be checked running the following commands by user `pi`:

`% sqlite3 smadata/SBFspot.db 'select * from SpotData order by timestamp desc limit 1'`

```
% sqlite3 smadata/SBFspot.db 'select * from vwSpotData order by TimeStamp DESC limit 1'
2021-11-25 17:12:14|2021-11-25 17:10:00|SN: xxx|STP xxx|xxx|0|0|0.0|0.0|0.0|0.0|0|0|0|0.0|...|OK|N/A|0.0
```

You have to create an InfluxDB database first.

Using cron, the sbfspot2influxdb script can be run by cron (`crontab -e` for user pi):

`*/5 6-22 * * * /home/pi/sqlite2influx 1> /home/pi/sqlite2influx.log 2>&1`
