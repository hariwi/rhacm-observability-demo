# rhacm-observability-demo

Create the S3 bucket:

Run the following command to login to AWS: aws configure and enter your AWS
keys when prompted.
Then run the following command to create the S3 bucket: 
aws s3 mb s3://grafana-$GUID
Please take note of the bucket name.

oc create namespace open-cluster-management-observability
DOCKER_CONFIG_JSON=`oc extract secret/pull-secret -n openshift-config --to=-`

oc create secret generic multiclusterhub-operator-pull-secret -n open-cluster-management-observability \
--from-literal=.dockerconfigjson="$DOCKER_CONFIG_JSON" \
--type=kubernetes.io/dockerconfigjson

Create a secret for your object storage by running the following command: 
oc create -f thanos-object-storage.yaml -n open-cluster-management-observability

oc apply -f multiclusterobservability_cr.yaml
