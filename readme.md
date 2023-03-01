This Git Repo will show how to apply Helm Sub-Charts/Umbrella Charts in ACM.  The appsub model and ArgoCD (applicationset) way will be shown.

In this exercise, we will download the mysql helm chart from helm.sh

First, we will look at default settings that get applied based on contents of tgz

Next, we will look at how web can use sub-chart/umbrella to make changes to the values.yaml

This resulting helm chart will be deployed two ways

1.  appsub
2.  ArgoCD way

helm repo add stable https://charts.helm.sh/stable/
helm pull https://charts.helm.sh/stable/ stable/mysql --version 1.6.9

Look at contents of tgz file (this information is contained in the full subdirectory of this repo)

1.  Take note of the contents of the defaults values.yaml (specifically the resources section)

resources:
  requests:
    memory: 256Mi
    cpu: 100m

2.  Let's deploy this using appsub model in ACM
  A.  In ACM GUI, Applications --> Create Application --> Subscription
  Name: helm-mysql
  Namespace: helm-mysql (create this in advance)
  Source: Helm
  URL: https://charts.helm.sh/stable
  Chart Name: Mysql
  Alias: Mysql
  Package Version: 1.6.9
  Deploy to Local-Cluster
  
The resulting YAML definition of this operation is contained in the yaml directory and is called full-mysql-appsub.yaml

3.  Look at the topology view in ACM.  Take note of how you would view status and YAML definitions of the objects that are created.
4.  Once we are finished, let's remove this application and any sub-objects
5.  Now, we will create the application using an applicationset and changing the request memory from 256Mi (default) to 128Mi.  The 2 files that are created to do this are called a sub-chart or umbrella chart.  In contrast to the application deployment that we did earlier (based on Helm), this one will be a Git repo.  There are 2 files in this Git Repo (contained in the mysql subdirectory) called Chart.yaml and values.yaml.  The Chart.yaml defines the Helm repo, the chart, and the version of the chart as a dependency.  The values.yaml will have only settings that differ from the parent values.yaml (contained within the tgz file).
6.  Take note of the bottom of the Chart.yaml file and the values.yaml.  
Chart.yaml

dependencies:
- name: mysql
  version: 1.6.9
  repository: https://charts.helm.sh/stable

values.yaml

mysql:
  resources:
    requests:
      memory: 128Mi
      cpu: 100m
      
7.  In ACM, go to In ACM GUI, Applications --> Create Application --> ApplicationSet
    Name: helm-mysql
    Argo-Server: openshift-gitops
    Source: GitRepo
    URL: https://github.com/kcalliga/helm-mysql
    Revision: main
    Path: mysql
    Remote Namespace: helm-mysql
    Accept defaults on next screen

    Lastly, create a placement if one does not already exist to push to local-cluster (New Placement --> Openshift ClusterSet --> Label local-cluster: true)
    
8.  Wait for ACM Topology View to update and now go to Openshift Gitops/ArgoCD dashboard (https://openshift-gitops-server-openshift-gitops.apps.<clustername>.<basedomain>
    
9.  Look at the way objects are visualized here.




