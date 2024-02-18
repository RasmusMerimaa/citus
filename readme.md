- kubectl apply -f deployment.yaml
- kubectl get pods
  kubectl exec -it $(kubectl get pods -l app=citus-master -o jsonpath='{.items[0].metadata.name}') -- psql -U postgres
