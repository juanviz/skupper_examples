# skupper_examples

## Description

Policies networks for apply to https://github.com/skupperproject/skupper-example-grpc

Tested with 3 Openshift clusters (west,east and east-2)

## Execution

*Public west, On Premise East and East-2 Namespace*

$  wget https://raw.githubusercontent.com/skupperproject/skupper/1.0/api/types/crds/skupper_cluster_policy_crd.yaml


$ kubectl apply -f skupper_cluster_policy_crd.yaml
customresourcedefinition.apiextensions.k8s.io/skupperclusterpolicies.skupper.io created
Warning: resource clusterroles/skupper-service-controller is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
clusterrole.rbac.authorization.k8s.io/skupper-service-controller configured




$ kubectl apply -f skupper_cluster_policy_crd.yaml


$ skupper status
Skupper is enabled for namespace "east-2" in interior mode (with policies). 


$ skupper link status

Links created from this site:
-------------------------------
Link link1 not connected (Destination host is not allowed)

Current links from other sites that are connected:
----------------------------------------
There are no connected links


*Public west*
$ oc apply -f allowincominglinks.yaml 


*On Premise East and East-2 Namespace*

$ oc apply -f allowoutgoinglinks.yaml 
skupperclusterpolicy.skupper.io/allowedoutgoinglinkshostnames created


$ skupper link status

Links created from this site:
-------------------------------
Link link1 is connected


*Public west, On Premise East and East-2 Namespace*

$ oc apply -f ../skupper-policies/allowservices.yaml
$ oc apply -f skupper-policies/allowedexposedresources.yaml


*Public west, On Premise East and East-2 Namespace*

$ kubectl exec deploy/skupper-service-controller -- get policies list
Defaulted container "service-controller" out of: service-controller, flow-collector
RULE                          VALUE                            ALLOWED_BY                    
AllowIncomingLinks            true                             allowincominglinks            
AllowedOutgoingLinksHostnames openshiftapps                    allowedoutgoinglinkshostnames 
AllowedExposedResources       deployment/productcatalogservice allowedexposedresources       
                              deployment/recommendationservice allowedexposedresources       
AllowedServices               adservice                        allowedservices               
                              cartservice                      allowedservices               
                              checkoutservice                  allowedservices               
                              currencyservice                  allowedservices               
                              emailservice                     allowedservices               
                              paymentservice                   allowedservices               
                              productcatalogservice            allowedservices               
                              recommendationservice            allowedservices               
                              redis-cart                       allowedservices               
                              shippingservice                  allowedservices    




