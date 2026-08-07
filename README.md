# MuViCo GitOps

![Argo CD](https://img.shields.io/badge/Argo%20CD-EF7B4D?logo=argo&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white)
![Kustomize](https://img.shields.io/badge/Kustomize-7B42BC?logo=kubernetes&logoColor=white)
![Quay.io](https://img.shields.io/badge/Quay.io-EE0000?logo=redhat&logoColor=white)
![UpCloud](https://img.shields.io/badge/UpCloud-7B00FF?logo=upcloud&logoColor=white)

GitOps repository for the MuViCo Kubernetes deployment on UpCloud. Argo CD
reconciles the cluster with the manifests declared here.

## Structure

```
bootstrap/argocd/    Argo CD install (namespace + upstream base)
bootstrap/root.yaml  App-of-Apps root (watches apps/)
apps/                AppProject + Applications (staging, prod, image-updater)
environments/        kustomize manifests per env (staging, prod)
infra/image-updater/ Argo CD Image Updater install
```

`root` syncs `apps/`, which deploys the Applications, which deploy `environments/*`.

## Bootstrap

```bash
kubectl apply -k bootstrap/argocd     # install Argo CD
kubectl apply -f bootstrap/root.yaml  # register the App-of-Apps
```

Admin password: `kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d`