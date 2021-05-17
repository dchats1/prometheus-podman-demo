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
If you navigate to the 'Statu's menu, and select 'Targets' page you should be able to see the node and prometheus job. If everything is working correctly both with show a state of 'UP'. They may take a minute to run the intial scrape. Reload the page if the State shows 'Unknown'.

![Prometheus Targets Page](screenshots/prometheus-targets.png "Prometheus Targets Page")

5. Run Grafana:
```
podman run -d --net host --name grafana grafana/grafana
```

6. Once Grafana has started, navigate to http://localhost:3000/ in your browser. The default login will be admin/admin  
  
To add a new target navigate to the data sources page under 'Configuration' > 'Data Sources':
![Data Sources](screenshots/grafana-datasources.png "Data Sources")

Import node-exporter dashboard: 13978
