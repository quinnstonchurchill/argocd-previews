# Sample Code For Preview Environments With Argo CD

See [blog post](https://codefresh.io/blog/creating-temporary-preview-environments-based-pull-requests-argo-cd-codefresh/) for rationale behind (apps|preview|project).yaml

- [Environments Based On Pull Requests (PRs): Using Argo CD To Apply GitOps Principles On Previews](https://youtu.be/cpAaI8p4R60)

# Getting started

Install Argo CD

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

To access the Argo CD GUI, you need the admin password (username: `admin`)

```
kubectl get secret/argocd-initial-admin-secret -n argocd -o jsonpath='{.data.password}' | base64 -d ; echo
```

In another terminal window, set up port forwarding

```
kubectl -n argocd port-forward service/argocd-server 8080:443
```

You can now see the Argo CD GUI at http://localhost:8080

# Development

Deploy the Argo CD pull request generator application set

```
kubectl apply -n argocd -f pull-request-generator.yaml
```

Verify that the app set was created

```
kubectl get appsets -n argocd
```

## Creating a PR preview

In app repo...

1. Build docker image
2. Tag image with commit SHA
3. Push images to container rep

In ops repo...

1. Create new branch matching app repo feature branch
2. Update helm yaml with new image in helm/templates
3. Commit and push
4. Create PR with preview label
