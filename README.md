# polymur

Mirrors Graphite metrics to multiple destinations

### Overview

Receives Graphite plaintext protocol (LF delimited messages) metrics from collectd/statsd/etc. and replicates the output to multiple destinations. Intended to be a high performance replacement for carbon-relay mirroring with additional health checks and failure mode controls. 

Polymur can also be used upstream to a single destination Graphite node; it's efficient at terminating thousands of connections and multiplexing the input over a single TCP connection to a backend Graphite server, in addition to buffering inbound metrics should the backend become unavailable.

### Installation

- `go get github.com/jamiealquiza/polymur`
- `go install github.com/jamiealquiza/polymur`
- Binary will be found at `$GOPATH/bin/polymur`

### Usage

<pre>
Usage of ./polymur:
  -console-out=false: Dump output to console
  -destinations="": Comma-delimited list of ip:port destinations
  -listen-addr="localhost": bind address
  -listen-port="2005": bind port
  -metrics-flush=0: Graphite flush interval for runtime metrics (0 is disabled)
  -queue-cap=4096: In-flight message queue capacity to any single destination
</pre>

### Examples

<pre>
./polymur -destinations="10.0.5.20:2003,10.0.5.25:2003" -listen-port="2003" -listen-addr="0.0.0.0" -metrics-flush=30
2015/05/11 15:13:05 Metrics listener started: 0.0.0.0:2003
2015/05/11 15:13:05 Connected to destination: 10.0.5.25:2003
2015/05/11 15:13:05 Connected to destination: 10.0.5.20:2003
2015/05/11 15:13:05 Runstats started: localhost:2020
2015/05/11 15:13:10 Last 5s: Received 6176 datapoints | Avg: 1235.20/sec. | Inbound queue length: 0
2015/05/11 15:13:15 Last 5s: Received 5685 datapoints | Avg: 1137.00/sec. | Inbound queue length: 0
2015/05/11 15:13:20 Last 5s: Received 8508 datapoints | Avg: 1701.60/sec. | Inbound queue length: 0
2015/05/11 15:13:25 Last 5s: Received 7990 datapoints | Avg: 1598.00/sec. | Inbound queue length: 0
2015/05/11 15:13:30 Last 5s: Received 10784 datapoints | Avg: 2156.80/sec. | Inbound queue length: 0
2015/05/11 15:13:35 Last 5s: Received 15606 datapoints | Avg: 3121.20/sec. | Inbound queue length: 0
2015/05/11 15:13:40 Last 5s: Received 12377 datapoints | Avg: 2475.40/sec. | Inbound queue length: 0
2015/05/11 15:13:45 Last 5s: Received 8390 datapoints | Avg: 1678.00/sec. | Inbound queue length: 0
</pre>
