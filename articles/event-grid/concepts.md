---
title: Concept d’Azure Event Grid
description: Détaille Azure Event Grid et ses concepts. Définit plusieurs composants clés de Event Grid.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/16/2018
ms.author: babanisa
ms.openlocfilehash: e5499fca98118de6ef8e08c8ce278b90520425e6
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="concepts-in-azure-event-grid"></a>Concepts utilisés dans Azure Event Grid

Voici les principaux concepts utilisés dans Azure Event Grid :

## <a name="events"></a>Événements

Un événement correspond à la plus petite quantité d’informations décrivant intégralement quelque chose qui s’est produit dans le système. Chaque événement possède des informations communes telles que la source de l’événement, l’heure à laquelle l’événement a eu lieu et un identificateur unique. Chaque événement comporte également des informations spécifiques qui se rapportent uniquement au type d’événement en question. Par exemple, un événement concernant un nouveau fichier en cours de création dans le Stockage Azure contient des informations détaillées sur le fichier, et notamment la valeur `lastTimeModified`. Ou bien, un événement concernant une machine virtuelle en cours de redémarrage contient le nom de la machine virtuelle et le motif du redémarrage. Chaque événement est limité à 64 Ko de données.

## <a name="event-sourcespublishers"></a>Sources d’événement/éditeurs

La source d’un événement désigne l’endroit où l’événement se produit. Par exemple, le stockage Azure est la source d’événement des événements créés pour l’objet blob. La structure Azure VM est la source d’événement des événements de machine virtuelle. Les sources d’événement sont responsables de la publication des événements dans Event Grid.

## <a name="topics"></a>Rubriques

Les éditeurs catégorisent les événements en rubriques. La rubrique inclut un point de terminaison à l’endroit où l’éditeur envoie les événements. Pour répondre à certains types d’événements, les abonnés décident des rubriques auxquelles ils souhaitent s’abonner. Les rubriques fournissent également un schéma d’événement qui permet aux abonnés de savoir comment consommer les événements de manière appropriée.

Les rubriques système sont des rubriques intégrées fournies par les services Azure. Les rubriques personnalisées sont des rubriques tierces et applicatives.

Lorsque vous concevez votre application, vous pouvez choisir le nombre de rubriques à créer. Pour les solutions volumineuses, créez une rubrique personnalisée pour chaque catégorie d’événements associés. Par exemple, envisagez une application qui envoie des événements liés à la modification de comptes d’utilisateur et au traitement de commandes. Il est peu probable qu’un gestionnaire d’événements accepte les deux catégories d’événements. Créez deux rubriques personnalisées et laissez les gestionnaires d’événements s’abonner à celle qui les intéresse. Pour les solutions de petite taille, vous pouvez à la place envoyer tous les événements à une seule rubrique. Les abonnés à des événements peuvent filtrer les types d’événements qu’ils souhaitent.

## <a name="event-subscriptions"></a>Abonnements à des événements

Un abonnement indique à Event Grid quels événements d’une rubrique un abonné souhaite recevoir. Un abonnement contient également les informations sur la façon dont les événements doivent être distribués à l’abonné.

## <a name="event-handlers"></a>Gestionnaires d’événements

Pour Event Grid, un gestionnaire d’événements désigne l’endroit où l’événement est envoyé. Le gestionnaire effectue des actions supplémentaires pour traiter l’événement. Event Grid prend en charge plusieurs types d’abonnés. Selon le type d’abonné, Event Grid suit différents mécanismes pour garantir la distribution de l’événement. Pour les gestionnaires d’événements HTTP Webhook, l’exécution de l’événement est retentée jusqu'à ce que le gestionnaire renvoie un code d’état de `200 – OK`. Pour la file d’attente de stockage Azure, l’exécution des événements est retentée jusqu'à ce que le service de file d’attente soit en mesure de traiter correctement la transmission de type push du message dans la file d’attente.

## <a name="filters"></a>Filtres

Lors de l’abonnement à une rubrique, vous pouvez filtrer les événements qui sont envoyés au point de terminaison. Vous pouvez filtrer par type d’événement, ou modèle d’objet. Pour plus d’informations, consultez [Schéma d’abonnement à Event Grid](subscription-creation-schema.md).

## <a name="security"></a>Sécurité

Event Grid assure la sécurité lors de l’abonnement et de la publication de rubriques. Lors de l’abonnement, vous devez disposer des autorisations appropriées sur la ressource ou la rubrique. Lors de la publication, vous devez disposer d’un jeton SAS ou d’une clé d’authentification pour la rubrique. Pour en savoir plus, consultez la page [Sécurité et authentification pour Event Grid](security-authentication.md).

## <a name="failed-delivery"></a>Échec de la distribution

Lorsque Event Grid ne peut pas confirmer qu’un événement a été reçu par le point de terminaison de l’abonné, il procède à une nouvelle distribution de l’événement. Pour plus d’informations, consultez la page [Distribution et nouvelle tentative de distribution de messages avec Event Grid](delivery-and-retry.md).

## <a name="next-steps"></a>Étapes suivantes

* Pour une présentation d’Event Grid, consultez [À propos d’Event Grid](overview.md).
* Pour une prise en main rapide d’Event Grid, consultez [Créer et acheminer des événements personnalisés avec Azure Event Grid](custom-event-quickstart.md).
