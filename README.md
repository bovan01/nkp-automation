# nkp-automation

The objective of this project is to provide guidance on using gitops to manage NKP Management Cluster resources like:
- Workspaces & Projects
- RBAC
- Clusters
- Appdeployments

Simply apply the following manifest to apply this to the cluster.
> Note: Make changes to the workspaces, projects, rbac and clusters to be created as required

```
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: nkp-automation
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
   name: nkp-automation
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
│       ├── batcave
│       │   ├── batcave.yaml
│       │   ├── clusters
│       │   ├── projects
│       │   │   ├── batman
│       │   │   │   ├── batman.yaml
│       │   │   │   └── rbac
│       │   │   └── robin
│       │   │       ├── rbac
│       │   │       └── robin.yaml
│       │   └── rbac
│       └── oscorp
│           ├── clusters
│           ├── oscorp.yaml
│           ├── projects
│           │   ├── green-goblin
│           │   │   ├── green-goblin.yaml
│           │   │   └── rbac
│           │   └── spiderman
│           │       ├── rbac
│           │       └── spiderman.yaml
│           └── rbac
└── workspaces-kustomization.yaml
```
