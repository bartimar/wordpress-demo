# wordpress-demo
Demo on Wordpress deployment @ GKE

Persistency enabled! 
# tldr;
```
$ helm install stable/wordpress
```

# customized deployment 
```
$ HELM_RELEASE_NAME=my-release
$ NAMESPACE=wordpress

$ helm install --name my-release \                                                
  --set wordpressUsername=admin,wordpressPassword=password,mariadb.mariadbRootPassword=secretpassword \
    stable/wordpress

```
for other values check the official helm chart https://github.com/kubernetes/charts/tree/master/stable/wordpress

# test the persistence
upload an image to media gallery on the wordpress page using your broswer
next delete the wordpress pod
when a new one is started and running, check the wordpress site, the data should be there
```
$ kubectl delete pod $(kubectl get pods -l app="$HELM_RELEASE_NAME"-wordpress -o jsonpath="{.items[0].metadata.name}")
pod "my-release-wordpress-57965cd666-tt2v6" deleted
$ kubectl get pods -w                                                                                                         NAME                                    READY     STATUS              RESTARTS   AGE
my-release-mariadb-6b9987fd44-6bddv     1/1       Running             0          1h
my-release-wordpress-57965cd666-279t8   0/1       ContainerCreating   0          2s
my-release-wordpress-57965cd666-tt2v6   1/1       Terminating         0          2m
my-release-wordpress-57965cd666-tt2v6   0/1       Terminating   0         2m
my-release-wordpress-57965cd666-279t8   0/1       Running   0         35s
```

# teardown the deployment
(Warning: This will delete all the data too!)
```
$ helm del --purge $HELM_RELEASE_NAME
```
