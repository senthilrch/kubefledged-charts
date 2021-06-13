# Kube-fledged helm charts

**_kube-fledged_** is a kubernetes add-on for creating and managing a cache of container images directly on the worker nodes of a kubernetes cluster. It allows a user to define a list
of images and onto which worker nodes those images should be cached (i.e. pre-pulled). As a result, application pods start almost instantly, since the images need not be pulled from the registry.

_kube-fledged_ provides CRUD APIs to manage the lifecycle of the image cache, and supports several configurable parameters to customize the functioning as per one's needs.

- Source code repository of kube-fledged:- https://github.com/senthilrch/kube-fledged
- Helm repository of kube-fledged:- https://senthilrch.github.io/kubefledged-charts

## Steps to install kube-fledged using helm chart

- Create the namespace where kube-fledged will be installed

  ```
  $ export KUBEFLEDGED_NAMESPACE=kube-fledged
  $ kubectl create namespace ${KUBEFLEDGED_NAMESPACE}
  ```

- Create secret containing cert/key for kubefledged-webhook-server

  ```
  $ curl -fsSL https://raw.githubusercontent.com/senthilrch/kube-fledged/master/deploy/webhook-create-signed-cert.sh | bash -s -- --namespace ${KUBEFLEDGED_NAMESPACE}
  ```

- Retrieve the certificate-authoity-data of the kubernetes cluster

  ```
  $ CLUSTER=$(kubectl config view --raw --flatten -o json | jq -r '.contexts[] | select(.name == "'$(kubectl config current-context)'") | .context.cluster')
  $ export CA_BUNDLE=$(kubectl config view --raw --flatten -o json | jq -r '.clusters[] | select(.name == "'${CLUSTER}'") | .cluster."certificate-authority-data"')
  ```

- Verify and install latest version of kube-fledged helm chart

  ```
  $ helm repo add kubefledged-charts https://senthilrch.github.io/kubefledged-charts/
  $ gpg --keyserver keyserver.ubuntu.com --recv-keys 92D793FA3A6460ED (or) gpg --keyserver pgp.mit.edu --recv-keys 92D793FA3A6460ED
  $ gpg --export >~/.gnupg/pubring.gpg
  $ helm install --verify kube-fledged kubefledged-charts/kube-fledged -n ${KUBEFLEDGED_NAMESPACE} --set validatingWebhookCABundle=${CA_BUNDLE} --wait
  ```

