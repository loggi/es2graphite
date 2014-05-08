### es2graphite

es2graphite gathers elasticsearch stats and status information 
and sends them to a graphite/statsd server.  One or more elasticsearch
hosts can be specified and requests will be round-robined between
each server.  

### metrics

* node stats
* indices stats
* indices status
* cluster health
* segments status

### notes

* each metric starts with a prefix + the cluster name
* metrics will be sent to graphite at the specified interval
* node names will be used in metrics vs. node id's
* if a shard is moved to a new node, a new metric using the new node name is used
* cluster status: 
    * 0 = red
    * 1 = yellow
    * 2 = green
* shard state: 
    * 0 = created
    * 1 = recovering
    * 2 = started
    * 3 = relocated
    * 4 = closed
* metrics are sent to graphite via the pickle protocol

```
usage: es2graphite.py [-h] [-p PREFIX] [-g HOST] [-o PORT] [-i INTERVAL]
                      [-m MODE] [--health-level {cluster,indices,shards}]
                      [--shard-stats] [--segments] [-d] [-v]
                      ES_HOST [ES_HOST ...]

Send elasticsearch metrics to graphite

positional arguments:
  ES_HOST               elasticsearch host:port

optional arguments:
  -h, --help            show this help message and exit
  -p PREFIX, --prefix PREFIX
                        graphite metric prefix. Default: es
  -g HOST, --host HOST  graphite hostname. Default: localhost
  -o PORT, --port PORT  graphite pickle protocol port. Default: 2004
  -i INTERVAL, --interval INTERVAL
                        interval in seconds. Default: 60
  -m MODE, --mode MODE  Which mode to use: carbon-pickle, statsd
  --health-level {cluster,indices,shards}
                        The level of health metrics. Default: indices
  --shard-stats         Collect shard level stats metrics.
  --segments            Collect low-level segment metrics.
  -d, --debug           Print metrics, don't send to graphite
  -v, --verbose         Verbose output
```
