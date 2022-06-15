# gitops-config

https://developers.redhat.com/articles/2021/08/03/managing-gitops-control-planes-secure-gitops-practices#application_delivery_with_openshift_gitops
https://github.com/redhat-developer/openshift-gitops-getting-started
https://github.com/wpernath/person-service-config


### machineconfig creation gives a below error 

"machineconfigs.machineconfiguration.openshift.io is forbidden: User "system:serviceaccount:openshift-gitops:openshift-gitops-argocd-application-controller" cannot create resource "machineconfigs" in API group "machineconfiguration.openshift.io" at the cluster scope"

Need to create policies for the controller to manage resources. By default, it has minimal privileges

Hence ClusterRoleBinding similar to https://access.redhat.com/solutions/6012601

skhobrag@Sunils-MacBook-Pro machineconfig % oc create -f ClusterRoleBinding.yaml
clusterrolebinding.rbac.authorization.k8s.io/argocd-workaround993-rolebinding created
skhobrag@Sunils-MacBook-Pro machineconfig % oc get ClusterRoleBinding | grep argo
argocd-workaround993-rolebinding                                            ClusterRole/cluster-admin                                                               111s
openshift-gitops-openshift-gitops-argocd-application-controller             ClusterRole/openshift-gitops-openshift-gitops-argocd-application-controller             47h
openshift-gitops-openshift-gitops-argocd-server                             ClusterRole/openshift-gitops-openshift-gitops-argocd-server                             47h
skhobrag@Sunils-MacBook-Pro machineconfig % 

### The Machine Config are applied 

skhobrag@Sunils-MacBook-Pro argo % oc get nodes --watch
NAME                                              STATUS                     ROLES    AGE    VERSION
ip-10-0-145-223.ap-southeast-1.compute.internal   Ready                      master   2d5h   v1.20.0+bafe72f
ip-10-0-156-171.ap-southeast-1.compute.internal   Ready,SchedulingDisabled   worker   2d5h   v1.20.0+bafe72f
ip-10-0-169-203.ap-southeast-1.compute.internal   Ready                      master   2d5h   v1.20.0+bafe72f
ip-10-0-173-17.ap-southeast-1.compute.internal    Ready                      worker   2d5h   v1.20.0+bafe72f
ip-10-0-230-47.ap-southeast-1.compute.internal    Ready                      worker   2d5h   v1.20.0+bafe72f
ip-10-0-242-106.ap-southeast-1.compute.internal   Ready,SchedulingDisabled   master   2d5h   v1.20.0+bafe72f

