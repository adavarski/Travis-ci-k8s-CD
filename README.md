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

