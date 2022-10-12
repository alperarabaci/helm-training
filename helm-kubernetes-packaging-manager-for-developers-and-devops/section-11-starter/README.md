# create project using starter
First we replace created sample project name with <CHARTNAME>. (Explained in section-05-create-chart)

Check out helm home directory. Then move starter folder to here.

```console
helm env HELM_DATA_HOME
/Users/jammer/Library/helm
```

Run create command.

```console
helm create --starter springwebapp  demoapp 
```
