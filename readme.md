In this exercise, we will download the httpd helm chart from bitnami

First, we will look at default settings that get applied based on contents of tgz

Next, we will look at how web can use sub-chart/umbrella to make changes to the values.yaml

This resulting helm chart will be deployed two ways

1.  appsub
2.  ArgoCD way

helm repo add bitnami https://charts.bitnami.com/bitnami
helm pull bitnami/apache

Look at contents of tgz file
