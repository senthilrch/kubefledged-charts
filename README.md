# Kube-fledged helm charts

**_kube-fledged_** is a kubernetes add-on for creating and managing a cache of container images directly on the worker nodes of a kubernetes cluster. It allows a user to define a list
of images and onto which worker nodes those images should be cached (i.e. pre-pulled). As a result, application pods start almost instantly, since the images need not be pulled from the registry.

_kube-fledged_ provides CRUD APIs to manage the lifecycle of the image cache, and supports several configurable parameters to customize the functioning as per one's needs.

- Source code repository of kube-fledged:- https://github.com/senthilrch/kube-fledged
- Helm repository of kube-fledged:- https://senthilrch.github.io/kubefledged-charts

