# argocd-training


apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: application-sample-two-repo
  namespace: argocd
spec:
  generators:
    - git:
        repoURL: 'https://github.com/NaveenBalagouni/argocd-training.git'
        revision: main  # Specify the branch, tag, or commit hash
        directories:
          - path: git-single-repo  # Path in the first repo
    - git:
        repoURL: 'https://github.com/OpsMx/argo-training.git'
        targetRevision: HEAD
        path: Naveen/two-git-repo # Path in the second repo
  template:
    metadata:
      name: '{{path.basename}}-app'  # Creates a unique name for each application
    spec:
      project: default
      sources:
        repoURL: 'https://github.com/NaveenBalagouni/argocd-training.git'
        targetRevision: HEAD
        path: git-single-repo
        repoURL: 'https://github.com/OpsMx/argo-training.git'
        targetRevision: HEAD
        path: Naveen/two-git-repo
      destination:
        server: https://kubernetes.default.svc
        namespace: two-repo-sources  # Ensure this namespace exists
