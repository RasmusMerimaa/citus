kubectl apply -f deployment.yaml
kubectl get pods
kubectl exec -it $(kubectl get pods -l app=citus-master -o jsonpath='{.items[0].metadata.name}') -- psql -U postgres
kubectl apply -f .\citus-service.yaml
kubectl get services
kubectl get pods -l app=citus-worker

SELECT citus_set_coordinator_host('citus-master-service', 5432);
SELECT * FROM citus_add_node('citus-worker-service', 5432);
SELECT * FROM citus_get_active_worker_nodes();
