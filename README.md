# owncloud-openshift
## How to deploy owncloud on OpenShift 4.x

Login to OpenShift and determine the public application domain for your cluster. You will need it to set the `OWNCLOUD_PUBLIC_DOMAIN` parameter.
The `oc whoami --show-console` should help.

Create an OpenShift project.
```
oc new-project owncloud
```

### Ephemeral 
Example
```
oc create -f https://raw.githubusercontent.com/owncloud-docker/openshift/master/ephemeral.json
oc new-app -p OWNCLOUD_PUBLIC_DOMAIN=owncloud.apps.shared-rhpds.rhpds.openshift.opentlc.com -p OWNCLOUD_ADMIN_PASSWORD=mypassword owncloud-ephemeral
```

### Persistent

Pod storage requirements: 
- mariadb (10Gi)
- redis (1Gi)
- owncloud (10Gi)   

Example

```
oc create -f https://raw.githubusercontent.com/owncloud-docker/openshift/master/persistent.json
oc new-app -p OWNCLOUD_PUBLIC_DOMAIN=owncloud.apps.shared-rhpds.rhpds.openshift.opentlc.com -p OWNCLOUD_ADMIN_PASSWORD=mypassword -p OWNCLOUD_VOLUME_CAPACITY=10Gi owncloud-persistent
```

Check the pod status.
```
oc get pods
```

Once the `owncloud`, `redis` and `mariadb` pods are running and ready, visit the `OWNCLOUD_PUBLIC_DOMAIN` url and login as `admin`.
