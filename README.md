## Deploy monitoring stack on my homelab

## REQUIREMENTS:

- I have prometheus.home and grafana.home both configured to same ip (pihole static dns)
- I have all the dependencies helm, go-task, envsubst installed

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


