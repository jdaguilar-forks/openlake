# Quickstart: MinIO for Kubernetes

This procedure deploys a Single-Node Single-Drive MinIO server onto Kubernetes for early development and evaluation of MinIO Object Storage and its S3-compatible API layer.

Use the MinIO Operator to deploy and manage production-ready MinIO tenants on Kubernetes.

# Prerequisites

An existing Kubernetes deployment where at least one Worker Node has a locally-attached drive.

A local kubectl installation configured to create and access resources on the target Kubernetes deployment.

Familiarity with Kubernetes environments

Familiarity with using a Terminal or Shell environment

# Procedure

1. Download the MinIO Object

Download the MinIO Kubernetes Object Definition

```
curl https://raw.githubusercontent.com/minio/docs/master/source/extra/examples/minio-dev.yaml -O
```

The file describes two Kubernetes resources:

- A new namespace minio-dev, and

- A MinIO pod using a drive or volume on the Worker Node for serving data

## Apply the MinIO Object Definition

The following command applies the minio-dev.yaml configuration and deploys the objects to Kubernetes:

```
kubectl apply -f minio-dev.yaml
```

The command output should resemble the following:

```
namespace/minio-dev created
pod/minio created
```

You can verify the state of the pod by running `kubectl get pods`:

```
kubectl get pods -n minio-dev
```

The output should resemble the following:

```
NAME    READY   STATUS    RESTARTS   AGE
minio   1/1     Running   0          77s
```

You can also use the following commands to retrieve detailed information on the pod status:

```
kubectl describe pod/minio -n minio-dev

kubectl logs pod/minio -n minio-dev
```

## 3.Temporarily Access the MinIO S3 API and Console

Use the `kubectl port-forward` command to temporarily forward traffic from the MinIO pod to the local machine:

```
kubectl port-forward pod/minio 9000 9090 -n minio-dev
```

The command forwards the pod ports `9000` and `9090` to the matching port on the local machine while active in the shell. The `kubectl port-forward` command only functions while active in the shell session. Terminating the session closes the ports on the local machine.

## 4. Connect your Browser to the MinIO Server

Access the MinIO Console by opening a browser on the local machine and navigating to `http://127.0.0.1:9001`.

Log in to the Console with the credentials `minioadmin | minioadmin`. These are the default root user credentials.

## (Optional) Connect the MinIO Client

If your local machine has mc installed, use the mc alias set command to authenticate and connect to the MinIO deployment:

```
mc alias set k8s-minio-dev http://127.0.0.1:9000 minioadmin minioadmin
mc admin info k8s-minio-dev
```

- The name of the alias

- The hostname or IP address and port of the MinIO server

- The Access Key for a MinIO user

- The Secret Key for a MinIO user
