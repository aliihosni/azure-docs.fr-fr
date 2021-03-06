---
title: Introduction au Kit de développement Azure Stack (ASDK) | Microsoft Docs
description: Décrit les nouveautés de l’ASDK et les cas d’utilisation courants pour l’évaluation de Microsoft Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 5a268a29c7a767084049bf56270aa8bc9d2ccc3f
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2018
---
# <a name="what-is-the-azure-stack-development-kit"></a>Qu’est-ce que le Kit de développement Azure Stack ?
Les [systèmes intégrés Microsoft Azure Stack](.\.\azure-stack-poc.md) présentent une taille comprise entre 4 et 12 nœuds et sont pris en charge conjointement par un fournisseur de matériel partenaire et Microsoft. Utilisez des systèmes intégrés Azure Stack pour autoriser de nouveaux scénarios pour vos charges de travail de production. Si vous êtes un opérateur Azure Stack gérant l’infrastructure de systèmes intégrés et proposant des offres, consultez la [documentation destinée à l’opérateur](https://docs.microsoft.com/azure/azure-stack).

Le Kit de développement Azure Stack (ASDK) est un déploiement à un seul nœud d’Azure Stack que vous pouvez télécharger et utiliser **gratuitement**. Tous les composants ASDK sont installés sur des machines virtuelles exécutées sur un serveur hôte unique qui doit respecter ou dépasser la [configuration matérielle minimale requise](asdk-deploy-considerations.md#hardware). L’ASDK est destiné à fournir un environnement dans lequel vous pouvez évaluer Azure Stack et développer des applications modernes à l’aide des API et les outils compatibles avec Azure dans un environnement *hors production*. 

> [!IMPORTANT]
> L’ASDK n’est pas conçu pour être utilisé ou pris en charge dans un environnement de production.

Étant donné que tous les composants ASDK sont déployés sur l’ordinateur hôte du Kit de développement, les ressources physiques disponibles sont limitées. Avec les déploiements ASDK, les machines virtuelles de l’infrastructure Azure Stack et celles du locataire coexistent sur le même ordinateur serveur. Cette configuration n’est pas destinée à l’évaluation des performances ou de la mise à l’échelle.

L’ASDK est conçu pour fournir une expérience de cloud hybride cohérente avec Azure pour :
- **Les administrateurs** (opérateurs Azure Stack). L’ASDK est une bonne ressource pour évaluer et découvrir les services Azure Stack disponibles.
- **Les développeurs**. L’ASDK peut servir à développer des applications hybrides ou modernes en local (environnements de développement/test). Cela permet de répéter l’expérience en développement avant ou pendant les déploiements en production d’Azure Stack. 

Regardez cette courte vidéo pour en savoir plus sur l’ASDK :

> [!VIDEO https://www.youtube.com/embed/dbVWDrl00MM]


## <a name="asdk-and-multi-node-azure-stack-differences"></a>Différences entre ASDK et les systèmes Azure Stack à plusieurs nœuds
Les déploiements ASDK à nœud unique et les déploiements Azure Stack à plusieurs nœuds présentent quelques différences importantes que vous devriez connaître.

|Description|ASDK|Système Azure Stack à plusieurs nœuds|
|-----|-----|-----|
|**Mettre à l'échelle**|Tous les composants sont installés sur un ordinateur serveur à nœud unique.|Peuvent comprendre entre 4 et12 nœuds.|
|**Résilience**|La configuration à un seul nœud ne fournit pas la haute disponibilité|Les fonctionnalités de [haute disponibilité](.\.\azure-stack-key-features.md#high-availability-for-azure-stack) sont prises en charge.|
|**Mise en réseau**|L’ASDK utilise une machine virtuelle nommée AzS-BGPNAT01 pour acheminer tout le trafic réseau ASDK. Il n’existe aucune exigence supplémentaire en matière de commutateur.|La machine virtuelle AzS-BGPNAT01 n’existe pas dans les déploiements à plusieurs nœuds. Une [infrastructure de routage réseau](.\.\azure-stack-network.md#network-infrastructure) plus complexe est nécessaire : commutateurs TOR (Top-Of-Rack), Baseboard Management Controller (BMC) et commutateurs limite (réseau de centre de données).|
|**Processus de correctif et mise à jour**|Pour passer à une nouvelle version de l’ASDK, vous devez redéployer l’ASDK sur l’ordinateur hôte du Kit de développement.|Le processus de [correctif et mise à jour](.\.\azure-stack-updates.md) utilisé pour mettre à jour la version d’Azure Stack installée.|
|**Support**|Forum MSDN Azure Stack. Le Support technique et Service clientèle de Microsoft (CSS) n’est *pas* disponible pour les environnements hors production.|[Forum MSDN Azure Stack](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStack) et support CSS complet.|
| | |

## <a name="learn-about-available-services"></a>Découvrir les services disponibles
En tant qu’opérateur Azure Stack, vous devez connaître les services que vous pouvez fournir à vos utilisateurs. Azure Stack prend en charge un sous-ensemble de services Azure et la liste des services pris en charge continuera d’évoluer au fil du temps.

### <a name="foundational-services"></a>Services fondamentaux
Par défaut, Azure Stack comprend les « services fondamentaux » suivants quand vous déployez l’ASDK :
- Calcul
- Stockage
- Mise en réseau
- Key Vault

Avec ces services fondamentaux, vous pouvez proposer une solution IaaS (Infrastructure-as-a-Service) à vos utilisateurs avec une configuration minimale.

### <a name="additional-services"></a>Services supplémentaires
Actuellement, les services PaaS (Platform-as-a-Service) supplémentaires suivants sont pris en charge :
- App Service
- Azure Functions
- Bases de données SQL et MySQL

> [!NOTE]
> Ces services nécessitent une configuration supplémentaire avant de pouvoir être mis à disposition des utilisateurs et ne sont pas disponibles par défaut lorsque vous installez l’ASDK.

## <a name="service-roadmap"></a>Feuille de route des services
Azure Stack continuera à prendre en charge de nouveaux services Azure. Pour en savoir plus sur les nouveautés Azure Stack à venir, consultez la [feuille de route Azure Stack](https://azure.microsoft.com/roadmap/?tag=azure-stack). 


## <a name="next-steps"></a>Étapes suivantes
Pour démarrer l’évaluation d’Azure Stack, vous devez préparer l’ordinateur serveur hôte du Kit de développement, puis [installer l’ASDK](asdk-deploy.md). Une fois que vous aurez effectué ces étapes préalables, vous pourrez vous connecter aux portails de l’administrateur et de l’utilisateur pour commencer à utiliser Azure Stack.