# Prometheus Monitoring / Alerting

Prometheus is an open-source systems monitoring and alerting toolkit.

This docker-compose file creates a simple prometheus environment.

The following containers will be created:


- prometheus : monitoring solution for recording and processing any purely numeric time-series metrics.
- node-exporter : hardware and OS metrics
- alertmanager : handles alerts
- cadvisor : container resource usage and performance 
- grafana : analytics and interactive visualization web application ( make things pretty )
- blackbox_exporter : probing of endpoints over HTTP, HTTPS, DNS, TCP and ICMP.
- traefik : reverse proxy
