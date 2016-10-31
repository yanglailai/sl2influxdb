# Seedlink to InfluxDB

Dump seedlink (seismological) time series into [InfluxDB](https://influxdata.com). Use [Grafana](http://grafana.org) to plot waveforms, real time latency delay, etc. Maps uses the grafana [worldmap-panel plugin](https://github.com/grafana/worldmap-panel)

## Usage
<pre>
Usage: seedlink2influxdb.py [options]

Options:
  -h, --help           show this help message and exit
  --dbserver=DBSERVER  InfluxDB server name
  --dbport=DBPORT      db server port
  --slserver=SLSERVER  seedlink server name
  --slport=SLPORT      seedlink server port
  --db=DBNAME          influxdb name to use
  --dropdb             [WARNING] drop previous database !
  --recover            try to get stream from last run
</pre>

## Screenshot 
Latency & traces for multiple stations:

<img src="https://cloud.githubusercontent.com/assets/4367036/12712706/95c4a38c-c8ca-11e5-8fa7-9c40bbdb8d24.png" width="250">

Waveform, RMS, latency plots for a given station:

<img src="https://cloud.githubusercontent.com/assets/4367036/12712707/95e9f498-c8ca-11e5-8115-cabb66dbf692.png" width="250">

Latency and Delay Map

<img src="https://cloud.githubusercontent.com/assets/4367036/19850077/d1ad4990-9f56-11e6-83ff-0c5de3587deb.png" width="250">


## InfluxDB

InluxDB data representation (measurements, tags, fields, timestamps).

Measurements:

* **queue**: internal messages producer queue (seedlink thread) and consumer queue (influxdb exporter thread)
	* tags  
		* **type**=(consumer|producer)
	* field
		* **size**=queue size
	* timestamp 

* **count** : amplitude in count (waveforms)
	* tags
		* **channel** = channel name (eg. FR.WLS.00.HHZ)
	* field
		* **value** = amplitude	
	* timestamp
	 
* **latency** : seedlink packet propagation time from station to datacenter
	* tags
		* **channel** = channel name 
	* field
		* **value** = latency value	
	* timestamp
		
* **delay** : time since last seedlink packet was received
	* tags
		* **channel** = channel name (eg. FR.WLS.00.HHZ)
		* **geohash** = station coordinates geohash formated
	* field 
 		* **value** = latency value
	* timestamp


## Dependencies:
* [obspy](https://github.com/obspy/obspy/wiki)
* [python InfluxDB](https://github.com/influxdata/influxdb-python)
* [geohash](https://github.com/vinsci/geohash/)
* [grafana](http://grafana.org) (with [worldmap-panel plugin](https://github.com/grafana/worldmap-panel))
