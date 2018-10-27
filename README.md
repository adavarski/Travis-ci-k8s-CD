## Example CI/CD k8s Travis CI

Setup Travis CI env variables

CI_ENV_K8S_CA

CI_ENV_K8S_MASTER

CI_ENV_K8S_SA_TOKEN

CI_ENV_REGISTRY_USER

CI_ENV_REGISTRY_PASS


NOTE:
Travis is integrated with GitHub. However, you need to specify which repositories are to be tested. This is done using Webhook which notifies Travis about changes in your repository.

Activating Webhook#

First, go to Travis CI and sign in with your GitHub account. After synchronizing your account, you`ll see all repositories you have access to. Flip switch to ON for all repositories you'd like to enable.

Travis will now add your repository to queue after every pushed commit or created pull-request. After a short while, your repository will be tested.

==========================================================================================================

deploy

Although we can achieve a fully automated pipeline from end to end, we'd often encounter situations to hold up deploying builds due to business reasons. As such, we tell Travis CI to run deployment scripts only when we release a new version.

To manipulate resources in our Kubernetes cluster from Travis CI, we'll need to grant Travis CI sufficient permissions. Our example uses a service account cd-agent under RBAC mode to create and update our deployments on behalf of us. RBAC. The templates for creating the account and permissions are at: k8s folder. The account is created under namespace cd, and it's authorized to create and modify most kinds of resources across namespaces.

Here we use a service account that is able to read and modify most resources across namespaces, including secrets of the whole cluster. Due to security concerns, its always encouraged to restrict permissions of a service account to resources the account actually used, or it could be a potential vulnerability.
Because Travis CI sits outside our cluster, we have to export credentials from Kubernetes so that we can configure our CI job to use them. Here we provide a simple script to help export those credentials:k8s/get-sa-token.sh.

$ ./get-sa-token.sh --namespace cd --account cd-agent

API endpoint:

https://35.184.53.170

ca.crt and sa.token exported

$ cat ca.crt | base64

LS0tLS1C...

$ cat sa.token

eyJhbGci...

Corresponding variables of exported API endpoint, ca.crt, and sa.token are CI_ENV_K8S_MASTER, CI_ENV_K8S_CA, and CI_ENV_K8S_SA_TOKEN respectively. 

The client certificate (ca.crt) is encoded to base64 for portability, and it will be decoded at our deployment script.

