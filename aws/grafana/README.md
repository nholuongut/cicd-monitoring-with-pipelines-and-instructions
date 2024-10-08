# Grafana Installation Guide

## Table of Contents
- [ADD Helm Repo](#add-helm-repo)
- [Creating NAMESPACE & PV & StorageClass](#creating-namespace--pv--storageclass)
- [Installing Grafana](#installing-grafana)
- [In addition](#in-addition)
- [Reference](#reference)

## ADD Helm Repo

```bash
helm repo add grafana https://grafana.github.io/helm-charts
```

## Creating NAMESPACE & PV & StorageClass

Choose PV or Storage Class:

```bash
kubectl create ns monitoring

# Select Disk 
kubectl apply -f efs-csi-pv.yaml -n monitoring
kubectl apply -f efs-csi-sc.yaml -n monitoring

kubectl apply -f ebs-csi-pv.yaml -n monitoring
kubectl apply -f ebs-csi-sc.yaml -n monitoring
```

## Installing Grafana

```bash
helm install grafana grafana/grafana -n monitoring -f values.yaml

helm upgrade grafana grafana/grafana -n monitoring -f values.yaml # Upgrade Method
```

## In addition
Modify the `Domain`, `host` in all yaml files.

## Reference
[Grafana Helmet Charts](https://github.com/grafana/helm-charts)

### [Contact ]
* [Name: nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com) 

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

# License
* Nho Luong (c). All Rights Reserved.
