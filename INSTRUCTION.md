## 4. DaemonSet and CronJob

### Deploying Background Tasks
To deploy the monitoring DaemonSet and the scheduled CronJob:

```
# Apply DaemonSet
kubectl apply -f .infrastructure/daemonset.yml

# Apply CronJob
kubectl apply -f .infrastructure/cronjob.yml
```
### Validation
1. Validate DaemonSet: The DaemonSet should be running on every available node.

```
# Check if pods are running
kubectl get ds -n mateapp
kubectl get pods -n mateapp -l app=curl-logger

# Check logs to see the curl output (should appear every 5 seconds)
kubectl logs -l app=curl-logger -n mateapp --tail=10 -f
```
2. Validate CronJob: The CronJob should create a generic Job every 4 minutes.

```
# Check the schedule and history limits
kubectl get cronjob -n mateapp

# Manually trigger a job to test immediately (optional)
kubectl create job --from=cronjob/health-check-cronjob manual-test-job -n mateapp

# Check created jobs and pods
kubectl get jobs -n mateapp
kubectl get pods -n mateapp | grep health-check

# Check logs of a completed job pod
# (Replace <pod-name> with actual name from previous command)
kubectl logs <pod-name> -n mateapp
```