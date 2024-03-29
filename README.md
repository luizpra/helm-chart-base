# Basic helm chart repository creation

[Link](https://medium.com/@mattiaperi/create-a-public-helm-chart-repository-with-github-pages-49b180dbb417)

Create a directory wich will be our source charts and use helm create command.

```bash
mkdir ./helm-chart-sources
helm create helm-chart-sources/helm-chart-test
```

### Lint the Chart 
 
The `helm lint` command runs a series of tests to verify that the cart is ok.

```bash
helm lint helm-chart-sources/*
```

### Package

The `helm package` command packages a chart into a versioned chart archive file.

```bash
helm package helm-chart-sources/*

- helm-chart-test.0.1.0.tgz will be created
```

## Repository index 

A repository index is a special file called `index.yaml` that has a list of all of the packeges supplied by the repository, together with metadata that allows retrieving and verifying those packages. 

The command `helm repo index` will create the `index.yaml` file based on the charts found.

```bash
helm repo index --url https://luizpaulorosaabrantes.github.io/helm-chart/ .
```

Lets seed the structure of `index.yaml` file, use `cat index.yaml` command.
```yaml

apiVersion: v1
entries:
  helm-chart-test:
  - apiVersion: v2
    appVersion: 1.16.0
    created: "2021-11-14T21:05:46.417675303-03:00"
    description: A Helm chart for Kubernetes
    digest: b956c8181de9b24d13cf682051bd44c88822d54165df600ef7858cf4862d8612
    name: helm-chart-test
    type: application
    urls:
    - https://luizpaulorosaabrantes.github.io/helm-chart/helm-chart-test-0.1.0.tgz
    version: 0.1.0
generated: "2021-11-14T21:05:46.417283607-03:00"
```

## Configure github repo as Page Site

Go to github repo settings and find Pages, just point out to your branch.

## Added helm repo as repository

In order to test our repo lets add our new repo as helm repository.

```bash
helm repo add lp-test https://luizpaulorosaabrantes.github.io/helm-chart-base/
```

Check with repo was added `helm repo list`:

```
NAME            URL                                                       
bitnami         https://charts.bitnami.com/bitnami                        
fluxcd          https://charts.fluxcd.io                                  
lp-test         https://luizpaulorosaabrantes.github.io/helm-chart-base/  
```

### Adde new charts to an existting repository

Whenever you need to add a new chart to the Helm chart repository, it’s mandatory for you to regenerate the `index.yaml` file. The `helm repo index` command will completely rebuild the index.yaml file from scratch, including only the charts that it finds locally, which very likely is our case. However, it worth notice that you can use the --merge flag to incrementally add new charts to an existing index.yaml.

```bash
helm repo index --url https://luizpaulorosaabrantes.github.io/helm-chart-base/ --merge index.yaml .
```