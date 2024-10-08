# Prometheus Setup Guide

## Table of Contents

- [Adding Helm Repository](#adding-helm-repository)
- [Creating Namespace, PV & StorageClass](#creating-namespace-pv--storageclass)
- [Installing Prometheus](#installing-prometheus)
- [Add Application Monitoring](#add-application-monitoring)
- [Additional Information](#additional-information)
- [Reference](#reference)

## Adding Helm Repository

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

## Creating Namespace, PV & StorageClass

Choose between PV or StorageClass:
```bash
kubectl create ns monitoring

# EFS Disk 
kubectl apply -f efs-csi-pv-prometheus-server.yaml -n monitoring
kubectl apply -f efs-csi-pv-prometheus-alertmanager.yaml -n monitoring
kubectl apply -f efs-csi-sc.yaml -n monitoring

# EBS Disk
kubectl apply -f ebs-csi-pv-prometheus-server.yaml -n monitoring
kubectl apply -f ebs-csi-pv-prometheus-alertmanager.yaml -n monitoring
kubectl apply -f ebs-csi-sc.yaml -n monitoring
```

## Installing Prometheus
```bash
helm install prometheus prometheus-community/prometheus -f values.yaml -n prometheus
helm upgrade prometheus prometheus-community/prometheus -f values.yaml -n prometheus # For Upgrades
```

## Add Application Monitoring
```bash
helm upgrade prometheus prometheus-community/prometheus -f extra-scrape-configs-values.yaml -f values.yaml -n prometheus
```

## Additional Information

Modify the `Domain`, `host` part in all yaml files.

## Reference

[Prometheus Helm Charts](https://github.com/prometheus-community/helm-charts)

### [Contact ]
* [Name: nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com) 

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

# License
* Nho Luong (c). All Rights Reserved.
