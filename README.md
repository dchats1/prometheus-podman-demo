# prometheus-podman-demo


Note: If you are using docker instead of podman, simply replace `podman` with `docker` when running the containers. All the steps should be the same.  

1. Run the node-exporter container (https://github.com/prometheus/node_exporter)
```
podman run -d --net host --name node quay.io/prometheus/node-exporter
```

2. Copy this Prometheus configuration file:
```
curl -LO https://raw.githubusercontent.com/dchats1/prometheus-podman-demo/main/prometheus.yml
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
  
7. To add a new target navigate to the data sources page under 'Configuration' > 'Data Sources':
![Data Sources](screenshots/grafana-datasource.png "Data Sources")

8. On the Data Sources page, click on 'Add data source'. By default there will be a list of available datasources. The first option will be 'Prometheus. Select the Prometheus data source.

![Data Sources](screenshots/grafana-select-prometheus.png "Data Sources")

9. Under the HTTP section, add the URL of the Prometheus container. In this case it will be http://localhost:9090  
Next, scroll to the bottom and click 'Save & Test'. If Grafana can reach the Prometheus server it will reply with 'Data source is working'.

![Data Sources](screenshots/grafana-set-url.png "Data Sources")
![Data Sources](screenshots/grafana-save-and-test.png "Data Sources")

10. Now, import the 'Node Exporter Quickstart and Dashboard' prebuilt dashboard (ID: 13978)  
https://grafana.com/grafana/dashboards/13978?pg=dashboards&plcmt=featured-sub1  

![Import](screenshots/grafana-import-1.png "Import")
![Import](screenshots/grafana-import-2.png "Import")
![Import](screenshots/grafana-import-3.png "Import")

![Dashboard](screenshots/grafana-dashboard.png "Dashboard")
