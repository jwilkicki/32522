# 32522

## Current behavior

This repository contains a Helm chart in the `argocd/core` directory.  This Helm chart implements an Argo CD App of Apps pattern where an Argo CD Application in turn creates more Argo CD Applications that install Helm charts into a Kubernetes cluster.  This layout reflects how our production repo looks; I included the directory structure in case something about it proves relevant in troubleshooting.  However, our production repo contains many more files.  The two application YAML's here are representative of an application that works with RenovateBot and one that does not.

In this particular case, there are two Argo CD Applications in this repo:  `argocd/core/templates/calico/calico-application.yaml` and `argocd/core/templates/external-dns/externaldns-application.yaml`  The `calico-application.yaml` installs a Helm chart for the tigera-operator (a Kubernetes operator that installs Calico). 
The `externaldns-application.yaml` installs the Helm Chart for the External DNS addon for updating DNS servers with A records for services running in a Kubernetes cluster.

Both of these applications specify versions of their respective Helm charts that are not the latest available.

I setup the Mend Renovate Github App on this repository and configured the `renovate.json` file so the `argocd` manager can find the two application files.

Renovate generates a PR for the `argocd/corecalico-application.yaml` with the expected update.  The `externaldns-application.yaml` in the `argocd/core/tempaltes` directory should generate a PR with a version update, but does not.

## Expected behavior

The expected behavior is for Renovate to generate a PR for both the `calico-application.yaml` and `externaldns-application.yaml` files in the `argocd/core/templates` directory.  The `calico-application.yaml` can be understood by RenovateBot and is the working example.  The `externaldns-application.yaml` file should be understood by RenovateBot since it is a valid Argo CD Application YAML file, but the package dependency is not found and is therefore the non-working example.

## Link to the Renovate issue or Discussion

[Link to discussion](https://github.com/renovatebot/renovate/discussions/32522)
