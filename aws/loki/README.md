# Loki Installation Guide

## Table of Contents
- [Add Helm Repo](#add-helm-repo)
- [Creating NAMESPACE & PV & StorageClass](#creating-namespace--pv--storageclass)
- [Installing Loki](#installing-loki)
- [Additional Information](#additional-information)
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

## Installing Loki

```bash
helm install loki  grafana/loki --version 2.16.0 -n monitoring -f values.yaml

helm upgrade loki  grafana/loki --version 2.16.0 -n monitoring -f values.yaml # Upgrade Method
```

## Additional Information
Modify the `Domain`, `host` part in all yaml files.

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

