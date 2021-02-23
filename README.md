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

## Theory of operation

The `kustomize` command reads `kustomizations.yml` and renders on
stdout resources declared in the `resources` key. It will add a
namespace key to the resources using the value declared in the
`namespace` key.

The `generators` section declares resources that are generated
dynamically. The `secret-generator.yml` file instructs `kustomize` to
generate resources from `ceph-admin-secret.yaml` andf
`ceph-client-secret.yaml` using the `viaduct.ai/v1/ksops` plugin.
`kustomize` will look for plugins in subdirectories of
`~/.config/kustomize/plugin` (where it will find the `ksops` plugin
you installed from the [Prerequisities](#prerequisites) section).

The [KSOPS][] plugin uses [sops][] to decrypt the named files. The
`sops` command will decrypt encrypted keys using GPG. When encrypting
files, it will encrypt data to the keys declared in `.sops.yaml`.

To edit an encrypted file:

```
sops <filename>
```

To decrypt a file to stdout:

```
sops -d <filename>
```
