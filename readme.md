kubectl apply -f deployment.yaml
kubectl get pods
kubectl exec -it $(kubectl get pods -l app=citus-coordinator -o jsonpath='{.items[0].metadata.name}') -- psql -U postgres
kubectl apply -f .\citus-coordinator-service.yaml
kubectl get services
kubectl get pods -l app=citus-worker

SELECT citus_set_coordinator_host('citus-coordinator-service', 5432);
SELECT * FROM citus_add_node('citus-worker-service', 5432);
SELECT * FROM citus_get_active_worker_nodes();

kubectl apply -f .\deployment.yaml
SELECT * FROM citus_add_node('citus-worker-statefulset-0.citus-worker-service', 5432);
SELECT * FROM citus_add_node('citus-worker-statefulset-1.citus-worker-service', 5432);
SELECT * FROM citus_get_active_worker_nodes();
