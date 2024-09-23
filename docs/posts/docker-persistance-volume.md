---
layout: post
title: Docker Persistance Volume
---

**Reference Documents**

[docker official docs](https://docs.docker.com/docker-for-azure/persistent-data-volumes/)

[microsoft official documenta](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-volume-azure-files)

[Cloudstor](https://docs.docker.com/docker-for-azure/persistent-data-volumes/)


**With Kubernetes(k8s)**
[reference from microsoft wiki](https://docs.microsoft.com/en-us/azure/aks/azure-files-volume)

* Storage create ARM template with anisble 

```yml
- hosts: localhost
  tasks:
    - name: Azure Storage
      azure_rm_deployment:
        resource_group: rg01
        name: Deployment-Storage
        template:
          $schema: https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#
          contentVersion: 1.0.0.0
          variables:
            storageAccountName: "[concat('storage', uniqueString(resourceGroup().id))]"
            fileShareName01: "file01"
            location: "westeurope"
          resources:
          - type: Microsoft.Storage/storageAccounts
            apiVersion: '2018-07-01'
            name: "[variables('storageAccountName')]"
            location: "[variables('location')]"
            kind: StorageV2
            sku:
              name: Standard_LRS
              tier: Standard
            properties:
              accessTier: Hot
          - type: Microsoft.Storage/storageAccounts/fileServices/shares
            apiVersion: '2019-04-01'
            name: "[concat(variables('storageAccountName'), '/default/', variables('fileShareName01'))"
            dependsOn:
            - "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"

```

* Create a Kubernetes secret

```
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=$STORAGE_SECREAT_KEY

```

* Mount the file share as a volume

```yml
apiVersion: v1
kind: Pod
metadata:
  name: jenkins
spec:
  containers:
  - image: jenkins/jenkins
    name: jenkins-dev
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
      - name: azure
        mountPath: /var/jenkins_home
  volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: file01
      readOnly: false

```

**With Docker or Dowcker Swarm**

.....will be available soon