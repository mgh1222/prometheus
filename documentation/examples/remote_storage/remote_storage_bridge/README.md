# Remote storage bridge

This is a bridge that receives samples in Prometheus's remote storage
format and forwards them to Graphite, InfluxDB, or OpenTSDB. It is meant
as a replacement for the built-in specific remote storage implementations
that will be removed from Prometheus soon.

## Building

```
go build
```

## Running

Example:

```
./remote_storage_bridge -graphite-address=localhost:8080 -opentsdb-url=http://localhost:8081/
```

To show all flags:

```
./remote_storage_bridge -h
Usage of ./remote_storage_bridge:
  -graphite-address string
    	The host:port of the Graphite server to send samples to. None, if empty.
  -graphite-prefix string
    	The prefix to prepend to all metrics exported to Graphite. None, if empty.
  -graphite-transport string
    	Transport protocol to use to communicate with Graphite. 'tcp', if empty. (default "tcp")
  -influxdb-url string
    	The URL of the remote InfluxDB server to send samples to. None, if empty.
  -influxdb.database string
    	The name of the database to use for storing samples in InfluxDB. (default "prometheus")
  -influxdb.retention-policy string
    	The InfluxDB retention policy to use. (default "default")
  -influxdb.username string
    	The username to use when sending samples to InfluxDB. The corresponding password must be provided via the INFLUXDB_PW environment variable.
  -log.format value
    	Set the log target and format. Example: "logger:syslog?appname=bob&local=7" or "logger:stdout?json=true" (default "logger:stderr")
  -log.level value
    	Only log messages with the given severity or above. Valid levels: [debug, info, warn, error, fatal] (default "info")
  -opentsdb-url string
    	The URL of the remote OpenTSDB server to send samples to. None, if empty.
  -send-timeout duration
    	The timeout to use when sending samples to the remote storage. (default 30s)
  -web.listen-address string
    	Address to listen on for web endpoints. (default ":9094")
  -web.telemetry-path string
    	Address to listen on for web endpoints. (default "/metrics")
```

## Configuring Prometheus

To configure Prometheus to send samples to this bridge, add the following to your `prometheus.yml`:

```yaml
remote_write:
  url: "http://localhost:9094/receive"
```