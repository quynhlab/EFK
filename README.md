# Elastic, FluentBit, Kibana for auto pushing desired logs to index
-------------------------------------------------------------------

#Pre-requisites: NA


#Configurations in fluentbit.yaml
#In Path below, mention path of container logs from desired pods(only) that need to be indexed.
#For indexing every container  log, set Path as  "/var/log/containers/*.log". This is an hard disk expensive setting.
```
  input-kubernetes.conf: |
    [INPUT]
        Name              tail
        Path              /var/log/containers/*.log
```

#create the namespaces
```
kubectl apply -f ns.yaml
```
# apply the elasticsearch yamls to bring up elastic pods
```
 kubectl apply -f elastic-service.yaml
 kubectl apply -f elastic-statefulset.yaml
```
#apply the curator yamls to setup cronjobs for cleanup of elastic indices every '30' days set in "unit_count: 30" at 00:01 Hrs.(both configurable)  
```
 kubectl apply -f es-curator-config.yaml
 kubectl apply -f es-curator.yaml
```
#apply the fluentbit and kibana yamls to deploy the respective pods
```
 kubectl apply -f fluentbit.yaml 
 kubectl apply -f kibana.yaml
```
