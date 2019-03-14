# Allow hostPath
## Openshift requires special permissions for in order to allow pods to use volumes in nodes.

Do the following:
Create standard security-context yaml:
    
    ################## scc-hostpath.yaml ####################
    
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
    
    ######################################
Run:
  - oc create -f scc-hostpath.yaml

## Add the "allowHostDirVolumePlugin" privilege to this security-context:
Run:
   - oc patch scc scc-hostpath -p '{"allowHostDirVolumePlugin": true}'

## Associate the pod's service account with the above security context
Run:
   - oc adm policy add-scc-to-user scc-hostpath system:serviceaccount:<service_account_name>
or
   - oc adm policy add-scc-to-group hostpath system:authenticated
     
--------------------------------------------------------------------------------------------------------------------------------

# Allow container run as privileged.
## Add the service account to the privileged SCC
   - oc adm policy add-scc-to-user privileged system:serviceaccount:myproject:mysvcacct
  

