## Prerequisites

- Install [kustomize][]:

  ```
  mkdir -p ~/bin
  curl -L https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2Fv4.0.1/kustomize_v4.0.1_linux_amd64.tar.gz |
    tar -C ~/bin -xzf -
  ```

- Install [sops][]:

  ```
  mkdir -p ~/bin
  curl -L -o ~/bin/sops https://github.com/mozilla/sops/releases/download/v3.6.1/sops-v3.6.1.linux
  chmod 755 ~/bin/sops
  ```

- Install [KSOPS][]:

  ```
  plugindir=~/.config/kustomize/plugin/viaduct.ai/v1/ksops
  mkdir -p $plugindir
  curl -L https://github.com/viaduct-ai/kustomize-sops/releases/download/v2.4.0/ksops_2.4.0_Linux_x86_64.tar.gz |
    tar -C $plugindir -xzf -
  ```

[kustomize]: https://kustomize.io/
[ksops]: https://github.com/viaduct-ai/kustomize-sops
[sops]: https://github.com/mozilla/sops

## Usage

To render YAML to stdout:

```
kustomize build --enable-alpha-plugins
```

To create (or update) resources in OpenShift:

```
kustomize build --enable-alpha-plugins | oc apply -f-
```
