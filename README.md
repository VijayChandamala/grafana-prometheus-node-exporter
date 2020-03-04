# grafana-prometheus-node-exporter
## System Monitoring using prometheus-node exporter and grafana

containers you're gonna run:

* prometheus
* prometheus-node-exporter
* Grafana


### command to run grafana

```docker run -id --name grafana -p 3000:3000 grafana/grafana```

If container is up and running, you should be able to see the grafana login page on http://localhost:3000

### command to run node-exporter

```docker run -d --pid="host" --name node-exporter -p 9100:9100 -v "/:/host:ro,rslave" quay.io/prometheus/node-exporter --path.rootfs=/host```

### command to run prometheus

Before running the prometheus container, make sure you have the following config file ```/tmp/prometheus.yml```

Here is an example of the config file

```
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:

- job_name: 'sys met'
  scheme: http
  metrics_path: '/metrics'
  static_configs:
    - targets:
         - '172.18.0.2:9100' ### you can get this IP by doing docker inspect node-exporter and port will be the same
```

Once created, run the following command to run prometheus

```docker run -id --name prometheus -p 9090:9090 -v /tmp/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus```

Now you should be able to see prometheus dashboard @ http://localhost:9090 in your browser

If the exporter is running and prometheus is able to pull metrics from the exporter, you can see the target(exporter) is connected and up here http://localhost:9090/targets

### Add prometheus as datasource and import the dashboard

https://grafana.com/grafana/dashboards/11074

Well Done!

