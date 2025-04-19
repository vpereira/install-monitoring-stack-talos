## Deploy monitoring stack on my homelab

## REQUIREMENTS:

- I have prometheus.home and grafana.home both configured to same ip (pihole static dns)
- I have all the dependencies helm, go-task, envsubst installed
- Proxmox Prometheus exporter is deployed and configured

## LIMITATIONS: 

- Code isn't idempotent
- I tested it on talos 
- It is taylored for my environment

## HOW TO USE IT:

```
export INGRESS_IP=192.168.0.172
export PROMETHEUS_HOST=prometheus.home
export GRAFANA_HOST=grafana.home
```

To be able to integrate ENV vars with Helm charts I used the tool `envsubst` as suggested here: https://stackoverflow.com/a/57865696/18642

Then run:

`task deploy` # monitor your cluster, it needs some time to download everything and sync all components

When you are done, you should be able to access $PROMETHEUS_HOST and $GRAFANA_HOST both on port 80


## Monitoring proxmox

Because CoreDNS wasn't able to resolve .home, I had to create a Service + Endpoint and use the internal name created by that in my additional scrape configuration. Please run

```kubectl apply -f charts/pve-exporter-svc```


## Tips and Tricks:

You cannot apply the charts automatically, because it needs the environment variable expansion
So to be able to play/debug with the configuration I normally have to:

```
envsubst < kube-prometheus-stack-values.yaml > /tmp/monitoring-values.yaml
helm upgrade --install monitoring prometheus-community/kube-prometheus-stack  --namespace monitoring --create-namespace  --reset-values   -f /tmp/monitoring-values.yaml
 ```

 which kind of sucks
