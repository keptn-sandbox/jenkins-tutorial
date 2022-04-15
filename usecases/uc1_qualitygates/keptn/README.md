# CREATE PROJECT

keptn create project adidas --shipyard=./shipyard.yaml

keptn onboard service glass --project=adidas --chart=./helm-charts/carts

keptn onboard service carts-db --project=adidas --chart=./helm-charts/carts-db

keptn configure monitoring prometheus --project=adidas --service=glass 

keptn add-resource --project=adidas --stage=dev --service=glass --resource=prometheus/sli-dev.yaml --resourceUri=prometheus/sli-dev.yaml

keptn add-resource --project=adidas --stage=dev --service=glass --resource=./slo.yaml --resourceUri=./slo.yaml

keptn add-resource --project=adidas --stage=staging --service=glass --resource=prometheus/sli.yaml --resourceUri=prometheus/sli.yaml

keptn add-resource --project=adidas --stage=staging --service=glass --resource=./slo.yaml --resourceUri=./slo.yaml

keptn add-resource --project=adidas --stage=production --service=glass --resource=prometheus/sli.yaml --resourceUri=prometheus/sli.yaml

keptn add-resource --project=adidas --stage=production --service=glass --resource=./slo.yaml --resourceUri=./slo.yaml

keptn add-resource --project=adidas --stage=dev --service=glass --resource=jmeter/basiccheck.jmx --resourceUri=jmeter/basiccheck.jmx

keptn add-resource --project=adidas --stage=staging --service=glass --resource=jmeter/load.jmx --resourceUri=jmeter/load.jmx

---

# DELETE PROJECT

keptn delete project adidas
kubectl delete namespace adidas-dev adidas-staging adidas-production loadgen

---
kubectl scale deployment carts-db -n adidas-staging --replicas=0

kubectl apply -f cartsloadgen/deploy/cartsloadgen-fast.yaml

keptn trigger delivery --project=adidas --service=carts-db --image=docker.io/mongo --tag=4.2.2 --sequence=delivery-direct

keptn trigger delivery --project=adidas --service=glass --image=0 --tag=0.13.1

keptn trigger delivery --project=adidas --service=glass --image=docker.io/keptnexamples/carts --tag=0.13.2

keptn trigger delivery --project="adidas" --service=glass --image="ghcr.io/podtato-head/podtatoserver" --tag=v0.1.1

keptn trigger evaluation --project=adidas --stage=staging --service=glass —timeframe=30m

kubectl port-forward svc/prometheus-server 8080:80 -n monitoring

kubectl port-forward svc/kiali 20001:20001 -n istio-system 



