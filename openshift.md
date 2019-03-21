# Allow hostPath
## Openshift requires special permissions for in order to allow pods to use volumes in nodes.

#### Do the following:
#### Create standard security-context yaml:
#### File namescc-hostpath.yaml
    
    kind: SecurityContextConstraints
    apiVersion: v1
    metadata:
      name: scc-hostpath
    allowPrivilegedContainer: true
    runAsUser:
      type: RunAsAny
    seLinuxContext:
      type: RunAsAny
    fsGroup:
      type: RunAsAny
    supplementalGroups:
      type: RunAsAny
    users:
    - my-admin-user
    groups:
    - my-admin-group
    
#### Run
    oc create -f scc-hostpath.yaml

## Add the "allowHostDirVolumePlugin" privilege to this security-context:
#### Run
    oc patch scc scc-hostpath -p '{"allowHostDirVolumePlugin": true}'

## Associate the pod's service account with the above security context
#### Run
    oc adm policy add-scc-to-user scc-hostpath system:serviceaccount:<service_account_name>
##### or
     oc adm policy add-scc-to-group hostpath system:authenticated
     
--------------------------------------------------------------------------------------------------------------------------------

# Allow container run as privileged.
## Add the service account to the privileged SCC
    oc adm policy add-scc-to-user privileged system:serviceaccount:myproject:mysvcacct
--------------------------------------------------------------------------------------------------------------------------------

# Delete Evicted pod
## oc get pod --all-namespaces | awk '{if ($4=="Evicted") print "oc delete pod " $2 " -n " $1;}' | sh

--------------------------------------------------------------------------------------------------------------------------------
# docker login after oc login
## docker login -p $(oc whoami -t) -u unused docker-registry-default.xxxx.xxxx.com

--------------------------------------------------------------------------------------------------------------------------------

# ETCD 2379: getsockopt: connection refused (Issue: Master service down all)
## Check etcd cluster-health
       export `cat /etc/etcd/etcd.conf |grep ETCD_LISTEN_CLIENT_URLS`
       etcdctl -C ${ETCD_LISTEN_CLIENT_URLS} --ca-file=/etc/etcd/ca.crt --cert-file=/etc/etcd/peer.crt --key-file=/etc/etcd/peer.key cluster-health

## Chcek etcd service staus
       systemctl status etcd.service
       
## Restart services
        systemctl restart etcd.service
        systemctl status etcd.service
        systemctl restart atomic-openshift-master-api
        systemctl restart atomic-openshift-master-controllers
        systemctl restart atomic-openshift-node
