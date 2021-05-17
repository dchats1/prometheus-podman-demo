# prometheus-podman-demo


1. Run the node-exporter container (https://github.com/prometheus/node_exporter)
```
podman run -d --net host --name node quay.io/prometheus/node-exporter
```

2. Save the following to a file called prometheus.yml
```
global:
  scrape_interval:     15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']

  - job_name: 'node'
    static_configs:
    - targets: ['localhost:9100']
```

3. Run the Prometheus container, and mount the newly created Prometheus configuration:
```
podman run -d --net host --name prometheus -v ./prometheus.yml:/etc/prometheus/prometheus.yml:z quay.io/prometheus/prometheus
```

4. In your browser, navigate to http://localhost:9090/
Under the Status > Targets page you should be able to see the node and prometheus job.

5. Run Grafana:
```
podman run -d --net host --name grafana grafana/grafana
```

6.  In your browser, navigate to http://localhost:3000/
Login: admin/admin

Import node-exporter dashboard: 13978
