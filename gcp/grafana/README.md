# Grafana Installation and Configuration Guide

<br/>

## Table of Contents

- [ADD Helm Repo](#add-helm-repo)
- [Creating NAMESPACE & PV & StorageClass](#creating-namespace--pv--storageclass)
  - [Select Disk](#select-disk)
  - [If using NFS](#if-using-nfs)
- [Installing Grafana](#installing-grafana)
- [Modifying a Service](#modifying-a-service)
- [ADD Ingress and Certificate](#add-ingress-and-certificate)
- [Notes](#notes)
- [Reference](#reference)

<br/>

## ADD Helm Repo

```bash
helm repo add grafana https://grafana.github.io/helm-charts
```

<br/>

## Creating NAMESPACE & PV & StorageClass

Choose either PV or Storage Class based on your requirements.

```bash
kubectl create ns monitoring


# Select Disk 
kubectl apply -f pd-csi-pv.yaml -n monitoring
kubectl apply -f pd-csi-sc.yaml -n monitoring  

kubectl apply -f fs-csi-pv.yaml -n monitoring
kubectl apply -f fs-csi-sc.yaml -n monitoring

kubectl apply -f fs-csi-pv.yaml -n monitoring
kubectl apply -f fs-csi-sc-shared-vpc.yaml -n monitoring

# If using NFS, read the guidance in nfs-sc-Readme.md
kubectl apply -f nfs-pv.yaml -n monitoring
```

<br/>

## Installing Grafana

```bash
helm install grafana grafana/grafana -n monitoring -f values.yaml

# To upgrade Grafana
helm upgrade grafana grafana/grafana -n monitoring -f values.yaml
```

<br/>

## Modifying a Service

To modify the Grafana service:

```bash
kubectl edit svc -n monitoring grafana
```

Then, update the annotations in the service as shown below:

```
apiVersion: v1
kind: Service
metadata:
  annotations:
    cloud.google.com/backend-config: '{"ports": {"http":"grafana-backend-config"}}'
```

<br/>

## ADD Ingress and Certificate

```bash
kubectl apply -f grafana-ingress.yaml -n monitoring
kubectl apply -f grafana-certificate.yaml -n monitoring
```

<br/>

## Notes:

Make sure to adjust the `Domain`, `host`, and `static-ip` sections in all yaml files as needed.

<br/>

## Reference:

- [Grafana Helm Charts](https://github.com/grafana/helm-charts)

### [Contact ]
* [Name: nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com) 

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

# License
* Nho Luong (c). All Rights Reserved.