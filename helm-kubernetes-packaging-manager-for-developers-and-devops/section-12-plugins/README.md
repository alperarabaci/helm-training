# install & use plugin
```console
helm plugin install https://github.com/sales
```

```console
helm plugin list
Available Commands:
    helm starter fetch GITURL       Install a bare Helm starter from Github (e.g git clone)
    helm starter list               List installed Helm starters
    helm starter update NAME        Refresh an installed Helm starter
    helm starter delete NAME        Delete an installed Helm starter
    helm starter inspect NAME       Print out a starter's readme
    --help                          Display this text
```

# create custom plugin

```console
helm plugin install .

helm plugin list
NAME    	VERSION	DESCRIPTION
starter 	1.0.0  	This plugin fetches, lists, and deletes helm starters from github.
myplugin	1.0.0  	my fist custom plugin

helm myplugin
the plug-in myplugin is located in /Users/jammer/Library/helm/plugins/plug-in
```

we can also add platform dependent commands.
```yaml
name: "last"
version: "0.1.0"
usage: "get the last release name"
description: "get the last release name"
ignoreFlags: false
command: "$HELM_BIN --host $TILLER_HOST list --short --max 1 --date -r"
platformCommand:
  - os: linux
    arch: i386
    command: "$HELM_BIN list --short --max 1 --date -r"
  - os: linux
    arch: amd64
    command: "$HELM_BIN list --short --max 1 --date -r"
  - os: windows
    arch: amd64
    command: "$HELM_BIN list --short --max 1 --date -r"
```


For more info: https://helm.sh/docs/topics/plugins/