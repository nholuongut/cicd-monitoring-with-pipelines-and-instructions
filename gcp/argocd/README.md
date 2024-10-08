# ArgoCD Installation and Configuration Guide

This guide will walk you through the installation of ArgoCD and several configuration steps that are essential for its functionality.

<br/>

## Table of Contents

- [Installation](#installation)
  - [Namespace Creation](#namespace-creation)
  - [Standard Installation](#standard-installation)
  - [High Availability Mode Installation](#high-availability-mode-installation)
  
- [Configurations](#configurations)
  - [Disabling Internal TLS](#disabling-internal-tls)
  - [Modifying a Service](#modifying-a-service)
  - [Adding Ingress and Certificate](#adding-ingress-and-certificate)
  - [Registering a Git Source Repository](#registering-a-git-source-repository)

- [Additional Notes](#additional-notes)
- [Reference](#reference)

<br/>

## Installation

1. **Namespace Creation**:
    ```bash
    kubectl create ns argocd
    ```

2. **Standard Installation**:
    ```bash
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

3. **High Availability Mode Installation**:
    ```bash
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/ha/install.yaml
    ```

<br/>

## Configurations

1. **Disabling Internal TLS**:
    Edit the config map and add `server.insecure`:

    ```bash
    kubectl edit cm -n argocd argocd-cmd-params-cm
    ```
    
    Then, under `data`, add:
    ```yaml
    server.insecure: "true"
    ```
    
    After the modification, restart the pod:
    ```bash
    kubectl delete po -n argocd argocd-server-xxxxxxxx-xxxxxx-xxxxxxx
    ```

2. **Modifying a Service**:
    ```bash
    k edit svc -n argocd argocd-server 
    ```

    Add the `backend-config` annotation:
    ```yaml
    cloud.google.com/backend-config: '{"ports": {"http":"argocd-backend-config"}}'
    ```

3. **Adding Ingress and Certificate**:

    ```bash
    kubectl apply -f argocd-ingress.yaml -n argocd
    kubectl apply -f argocd-certificate.yaml -n argocd
    ```

4. **Registering a Git Source Repository**:
    
    Generate an SSH key, and register it with the repo:

    ```bash
    ssh-keygen -t rsa -f ~/.ssh/[KEY_FILENAME] -C [USERNAME]         # for rsa format
    ssh-keygen -m PEM -t rsa -f ~/.ssh/[KEY_FILENAME] -C [USERNAME]  # for pem format

    kubectl apply -f repo-secret.yaml -n argocd
    ```

5. **Confirm Initial Password**:
    ```bash
    kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
    ```

6. **Except Cilium Resource Argocd(Options)**
    Here's how to add the resource.exclusions to the argocd-cm ConfigMap in your ArgoCD installation guide:
    ```yaml
    k edit cm -n argocd argocd-cm 

    apiVersion: v1
    ...
      resource.exclusions: |
        - apiGroups:
          - cilium.io
          kinds:
          - CiliumIdentity
          clusters:
          - "*"
    ```

7. **Argocd SSO Configuration(Options)**
    ```yaml
    k edit cm -n argocd argocd-rbac-cm 

    apiVersion: v1
    data:
      dex.config: |
        connectors:
          - type: oidc
            id: google
            name: Google
            config:
              issuer: https://accounts.google.com
              clientID: 7xxxxxxxxx-fxxxxxxxxxxxxxxxxxxxxxxxxxx.apps.googleusercontent.com
              clientSecret: Gxxxxx-xxxxxxxxxxxxxxxxxxxxxxxxxxv3
              redirectURI: https://{argocd domain}/api/dex/callback
      url: https://{argocd domain}
    ```

8. **Argocd User Rbac Setting(Options)**
    ```yaml
    k edit cm -n argocd argocd-rbac-cm 
    apiVersion: v1
    data:
      policy.csv: |
        p, role:org-admin, applications, *, */*, allow
        p, role:org-admin, applicationsets, *, */*, allow
        p, role:org-admin, clusters, get, *, allow
        p, role:org-admin, repositories, get, *, allow
        p, role:org-admin, repositories, create, *, allow
        p, role:org-admin, repositories, update, *, allow
        p, role:org-admin, repositories, delete, *, allow
        p, role:org-admin, projects, get, *, allow
        p, role:org-admin, projects, create, *, allow
        p, role:org-admin, projects, update, *, allow
        p, role:org-admin, projects, delete, *, allow
        p, role:org-admin, logs, get, *, allow
        p, role:org-admin, exec, create, */*, allow
        g, somaz@example.com, role:org-admin
      policy.default: role:readonly
      scopes: '[groups, email]'
    ```       

<br/>

## Additional Notes

- Ensure that you modify the `Domain`, `host`, and `static-ip` portions in all the yaml files provided.
- Also, adjust the key details within the `repo-secret.yaml` file as necessary.

<br/>

## Reference

- [ArgoCD Ingress Operator Manual](https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/#google-cloud-load-balancers-with-kubernetes-ingress)


### [Contact ]
* [Name: nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com) 

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

# License
* Nho Luong (c). All Rights Reserved.