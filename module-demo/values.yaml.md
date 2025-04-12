# Exploring Cyclops: A Hands-on Tutorial

This document summarizes our hands-on exploration of Cyclops, a Kubernetes management platform.

## Initial Setup

First, we initialized Cyclops in our cluster:
```bash
$ cyctl init
```

## Exploring Available Templates

We checked the available templates in our cluster:
```bash
$ cyctl get templates
NAME                 AGE
app-template         25m37s
cerbos               25m37s
demo                 25m37s
jenkins              25m37s
k6-operator          25m37s
mariadb              25m37s
metabase             25m37s
mysql                25m37s
postgresql           25m37s
prometheus           25m37s
rabbitmq             25m37s
redis                25m37s
valkey               25m37s
```

## Creating Our First Application

1. We created a `values.yaml` file with the following configuration:
```yaml
name: demo-app
replicas: 2
image: nginx
version: 1.14.2
service: true
```

2. Created a module using the demo template:
```bash
$ cyctl create module demo-app --template demo -f values.yaml
demo-app created successfully.
```

3. Verified the module creation:
```bash
$ cyctl describe module demo-app
Name:         demo-app
Namespace:    cyclops
Labels:       <none>
Annotations:  <none>
Creation:     2025-04-12 16:39:01 +0100 CET
Status:       succeeded

Template:            
  Repository:        https://github.com/cyclops-ui/templates
  Relative Path:     demo
  Branch:            main
  Resolved Version:  ab2756886ba4c9e0e3122e3cbb6dc0338fadc617

Values:
  image: nginx
  name: demo-app
  replicas: 2
  service: true
  version: 1.14.2
```

4. Checked the actual Kubernetes resources:
```bash
$ kubectl get all -l app=demo-app
NAME                            READY   STATUS    RESTARTS   AGE
pod/demo-app-74cd5b8666-n8q2v   1/1     Running   0          8m9s
pod/demo-app-74cd5b8666-wz89j   1/1     Running   0          8m9s

NAME               TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/demo-app   ClusterIP   10.96.66.241   <none>        80/TCP    8m9s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/demo-app   2/2     2            2           8m9s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/demo-app-74cd5b8666   2         2         2       8m9s
```

## What We Learned

1. **Template Management**: Cyclops provides a variety of pre-configured templates for common applications.
2. **Easy Deployment**: With a simple values file and one command, we deployed a complete nginx application.
3. **Resource Management**: Cyclops automatically created all necessary Kubernetes resources (Deployment, Service, Pods).
4. **Status Monitoring**: We could easily check the status and configuration of our deployment using Cyclops commands.

## Key Concepts Demonstrated

- Template-based deployment
- Configuration management through values files
- Resource creation and management
- Status monitoring and verification

This demo showcased how Cyclops simplifies Kubernetes application deployment and management through its template-based approach and straightforward CLI commands.