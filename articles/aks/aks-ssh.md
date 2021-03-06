---
title: SSH dans les nœuds de cluster Azure Container Service (AKS)
description: Créer une connexion SSH avec des nœuds de cluster Azure Container Service (AKS)
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 04/06/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 085a2976443db8ece7a36dbfc133b173432ce4c8
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="ssh-into-azure-container-service-aks-cluster-nodes"></a>SSH dans les nœuds de cluster Azure Container Service (AKS)

Vous avez parfois besoin d’accéder à un nœud Azure Container Service (AKS) à des fins de maintenance, de collecte de journaux ou d’autres opérations de dépannage. Les nœuds Azure Container Service (AKS) ne sont pas exposés sur Internet. Utilisez les étapes décrites dans ce document pour créer une connexion SSH avec un nœud AKS.

## <a name="get-aks-node-address"></a>Obtenir l’adresse du nœud AKS

Obtenez l’adresse IP d’un nœud de cluster AKS à l’aide de la commande `az vm list-ip-addresses`. Remplacez le nom du groupe de ressources par le nom de votre groupe de ressources AKS.

```console
$ az vm list-ip-addresses --resource-group MC_myAKSCluster_myAKSCluster_eastus -o table

VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-42032720-0  10.240.0.6
aks-nodepool1-42032720-1  10.240.0.5
aks-nodepool1-42032720-2  10.240.0.4
```

## <a name="create-ssh-connection"></a>Créer une connexion SSH

Exécutez l’image conteneur `debian` et attachez-y une session de terminal. Le conteneur peut ensuite être utilisé pour créer une session SSH avec n’importe quel nœud du cluster AKS.

```console
kubectl run -it --rm aks-ssh --image=debian
```

Installez un client SSH dans le conteneur.

```console
apt-get update && apt-get install openssh-client -y
```

Ouvrez un deuxième terminal et répertoriez de tous les pods pour obtenir le nom du nouveau pod créé.

```console
$ kubectl get pods

NAME                       READY     STATUS    RESTARTS   AGE
aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
```

Copiez votre clé SSH dans le pod, remplacez le nom du pod par la valeur appropriée.

```console
kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
```

Mettez à jour le fichier `id_rsa` afin qu’il soit en lecture seule pour l’utilisateur.

```console
chmod 0600 id_rsa
```

Créez maintenant une connexion SSH au nœud AKS. Le nom d’utilisateur par défaut d’un cluster AKS est `azureuser`. Si ce compte a été modifié au moment de la création du cluster, remplacez le nom d’utilisateur administrateur par celui qui convient.

```console
$ ssh -i id_rsa azureuser@10.240.0.6

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

48 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Tue Feb 27 20:10:38 2018 from 167.220.99.8
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@aks-nodepool1-42032720-0:~$
```

## <a name="remove-ssh-access"></a>Supprimer l’accès SSH

Lorsque vous avez terminé, quittez la session SSH, puis la session de conteneur interactive. Cette action supprime le pod utilisé pour l’accès SSH à partir du cluster AKS.