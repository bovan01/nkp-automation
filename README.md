# nkp-automation

The objective of this github repo is to provide an example on using gitops & nkp's fluxCD to automate management of NKP Management Cluster resources such as:
- Workspaces & Projects
- RBAC
- Clusters
- Appdeployments

In this example, NKP automation will occur & be limited to the 'wolves' workspace & its projects, 'red' & 'gray'.

Apply the following manifest to the NKP Management Cluster.
**NOTE: Make changes to the resources (workspaces, project, rbac, clusters & appdeployments) as necessary to meet desired needs. Any secrets with PC or registry credentials will be applied directly in the given workspace namespace of the NKP Management Cluster.**

```
kubectl apply -f - <<EOF
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: nkp-wolves-automation
  namespace: kommander
spec:
  interval:  5s
  ref:
    branch: main
  timeout: 20s
  url: https://github.com/bovan01/nkp-automation.git
---
kubectl apply -f -  <<EOF
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: nkp-kustomization
  namespace: kommander
spec:
  interval: 5s
  path: ./
  prune: true
  sourceRef:
   kind: GitRepository
   name: nkp-wolves-automation
   namespace: kommander
EOF
```
Here is the structure of the folders and files. 
```
├── kustomization.yaml
├── kustomizations
│   ├── clusters
│   ├── project-rbac
│   ├── projects
│   │   └── kustomization.yaml
│   ├── workspace-rbac
│   └── workspaces
│       └── kustomization.yaml
├── projects-kustomization.yaml
├── resources
│   └── workspaces
│       └── wolves
│           ├── wolves.yaml
│           ├── clusters
│           ├── projects
│           │   ├── gray
│           │   │   ├── gray.yaml
│           │   │   └── rbac
│           │   └── red
│           │       ├── rbac
│           │       └── red.yaml
│           └── rbac
└── workspaces-kustomization.yaml
```
