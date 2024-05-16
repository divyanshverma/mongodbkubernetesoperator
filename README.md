# Setup MongoDB with community operator
# Clone the repo https://github.com/mongodb/mongodb-kubernetes-operator

kubectl --kubeconfig=obto-database-kubeconfig.yaml apply -f operator/config/crd/bases/mongodbcommunity.mongodb.com_mongodbcommunity.yaml

kubectl --kubeconfig=obto-database-kubeconfig.yaml apply -k operator/config/rbac/

kubectl --kubeconfig=obto-database-kubeconfig.yaml apply -f operator/config/manager/manager.yaml

##TLS

kubectl --kubeconfig=obto-database-kubeconfig.yaml create configmap ca-config-map --from-file=ca.crt=<path-to->rootCA.pem

kubectl --kubeconfig=obto-database-kubeconfig.yaml create secret tls ca-key-pair  --cert=<path-to->rootCA.pem  --key=<path-to-key>rootCA-key.pem

kubectl --kubeconfig=obto-database-kubeconfig.yaml apply -f operator/config/samples/external_access/cert-manager-issuer.yaml

kubectl --kubeconfig=obto-database-kubeconfig.yaml apply -f operator/config/samples/external_access/cert-manager-certificate.yaml

##CREATE FINALLY

kubectl --kubeconfig=obto-database-kubeconfig.yaml apply -f operator/config/samples/mongodb.com_v1_mongodbcommunity_cr.yaml

##WAIT FOR PODS TO START

kubectl --kubeconfig=obto-database-kubeconfig.yaml apply -f operator/config/samples/external_access/external_services.yaml
