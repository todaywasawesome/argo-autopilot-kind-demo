# Argo CD Autopilot: Bootstraping and promoting applications across environments with GitOps


## Prerequisites
* [Kind cli](https://kind.sigs.k8s.io/docs/user/quick-start#installation) installed
* Standard CLIs ([Kubectl](https://kubernetes.io/docs/tasks/tools/), [Argo CD](https://argoproj.github.io/argo-cd/cli_installation/), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git))


## GitOps Mini-workshop 

### Create Kind clusters
Let's start by creating two clusters

`kind create cluster --name mystaging`

`kind create cluster --name myprod`

Verify the two clusters were created and available with `kind get clusters`.

```
❯ kind get clusters
myprod
mystaging
```

### Install and Setup ArgoCD Autopilot
Full Cli install instructions can be [found here](https://github.com/argoproj-labs/argocd-autopilot#installation).

If you're using Mac you can do the following
```
# get the latest version or change to a specific version
VERSION=$(curl --silent "https://api.github.com/repos/argoproj-labs/argocd-autopilot/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')

# download and extract the binary
curl -L --output - https://github.com/argoproj-labs/argocd-autopilot/releases/download/$VERSION/argocd-autopilot-darwin-amd64.tar.gz | tar zx

# move the binary to your $PATH
mv ./argocd-autopilot-* /usr/local/bin/argocd-autopilot

# check the installation
argocd-autopilot version
```

Argo CD Autopilot is going to setup our repos and work directly with git so we need to provide a [personal access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) and a repo.

```
# Most of the commands need your git token, you can provide with --git-token to each command
# or export it beforehand:

    export GIT_TOKEN=<YOUR_TOKEN>

# 1. Create a new git repository

    argocd-autopilot repo create --owner <owner> --name <name>

# At this point you can specify the gitops repo in each command with --repo
# or you can export it as well:

    export GIT_REPO=<REPO_URL>

# 2. Run the bootstrap installation on your current kubernetes context.
# This will install argo-cd as well as the application-set controller.

    argocd-autopilot repo bootstrap

# 3. Create your first project

    argocd-autopilot project create my-project

# 4. Install your first application on your project

    argocd-autopilot app create demoapp --app github.com/argoproj-labs/argocd-autopilot/examples/demo-app/ -p my-project

 ```
 
 