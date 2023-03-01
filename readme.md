In this exercise, we will download the mysql helm chart from helm.sh

First, we will look at default settings that get applied based on contents of tgz

Next, we will look at how web can use sub-chart/umbrella to make changes to the values.yaml

This resulting helm chart will be deployed two ways

1.  appsub
2.  ArgoCD way

helm repo add stable https://charts.helm.sh/stable/
helm pull https://charts.helm.sh/stable/ stable/mysql --version 1.6.9

Look at contents of tgz file (this information is contained in the full subdirectory of this repo
