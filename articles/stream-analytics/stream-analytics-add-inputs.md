---
title: Ajouter une entrée de données à vos tâches Azure Stream Analytics
description: Découvrez comment connecter une source de données à votre tâche Stream Analytics en tant qu'entrée de données de diffusion en continu partir de Event Hubs ou référencer des données à partir du stockage d'objets blob.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 13c3c948fbe24d5536b32967c8394060ee898377
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Ajout d'une entrée de données de diffusion en continu ou de données de référence à une tâche Stream Analytics
Découvrez comment connecter une source de données à votre tâche Stream Analytics en tant qu’entrée de données de diffusion en continu à partir de Event Hubs ou référencer des données à partir du stockage d’objets blob.

Les tâches Azure Stream Analytics peuvent être connectées à une ou plusieurs entrées de données, chacune d'entre elles définissant une connexion à une source de données existante. Comme les données sont envoyées à la source de données, elles sont utilisées par la tâche Stream Analytics et traitées en temps réel en tant que données de diffusion en continu. Stream Analytics propose une intégration de pointe à [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) et au [Stockage Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) externes et internes à l’abonnement du travail.

Cet article est une étape dans le [parcours d'apprentissage de Stream Analytics](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Entrée de données : données de référence et données de diffusion en continu
Il existe deux types d’entrées dans Stream Analytics : les flux de données et les données de référence.

* **Flux de données**: les tâches Stream Analytics doivent contenir au moins une entrée de flux de données qui sera traitée et transformée par la tâche. Le stockage d’objets blob Azure et Azure Event Hubs sont pris en charge en tant que sources d’entrée de flux de données. Les hubs d'événements Azure sont utilisés pour collecter les flux d'événements à partir des appareils, services et applications connectés. Le stockage d'objets blob Azure peut être utilisé comme source d'entrée pour la réception de données en bloc en tant que flux.  
* **Données de référence**: Stream Analytics prend en charge un deuxième type d'entrée auxiliaire appelé données de référence.  Par opposition aux données en mouvement, ces données sont statiques ou changent lentement.  Elles sont généralement utilisées pour effectuer des recherches et des corrélations avec des flux de données pour créer un jeu de données plus riche.  Le stockage d’objets blob Azure est la seule source d’entrée prise en charge pour les données de référence.  

Pour ajouter une entrée à votre tâche Stream Analytics :

1. Dans le portail Azure, cliquez sur **Entrées**, puis sur **Ajouter une entrée** dans votre travail Stream Analytics.
   
    ![Portail Azure - ajouter une entrée.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    Dans le portail Azure, cliquez sur le titre **Entrées** de votre tâche Stream Analytics.  
   
    ![Portail Azure - ajouter une entrée de données.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. Spécifiez le type de l'entrée : **Flux de données** ou **Données de référence**.
   
    ![Ajouter l’entrée de données appropriée (diffusion en continu ou référence)](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![Ajouter l’entrée de données appropriée (diffusion en continu ou référence)](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. Si vous créez une entrée Flux de données, spécifiez le type de source pour l'entrée.  Cet étape peut être ignorée lors de la création de Données de référence car seul le stockage d'objets Blob est pris en charge à ce stade.
   
    ![Ajouter une entrée de données de diffusion en continu](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Ajouter une entrée de données de diffusion en continu](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. Attribuez un nom convivial à cette entrée dans la zone Alias d'entrée.  Ce nom sera utilisé dans la requête de votre tâche plus tard pour faire référence à l'entrée.
   
    Renseignez le reste des propriétés de connexion requises pour vous connecter à votre source de données. 
   
    ![Ajouter une entrée de données de hub d’événements](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. Spécifiez les paramètres de sérialisation pour les données d'entrée :
   
   * Pour vous assurer que vos requêtes fonctionnent comme vous le souhaitez, spécifiez le **Format de sérialisation d'événements** des données entrantes.  Les formats de sérialisation pris en charge sont JSON, CSV et Avro. Vérifiez que le format JSON est conforme à la spécification et n’inclut pas de 0 au début des nombres décimaux.
   * Vérifiez le **codage** des données.  UTF-8 est le seul format de codage actuellement pris en charge.
     
     ![Paramètres de sérialisation des données pour l’entrée de données](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![Paramètres de sérialisation des données pour l’entrée de données](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. Après avoir terminé la création de l'entrée, Stream Analytics vérifie qu'il peut se connecter à la source d'entrée.  Vous pouvez afficher l'état de l'opération de test de la connexion dans le hub de notification.
   
    ![Tester la connexion de l’entrée de données de diffusion en continu](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![Tester la connexion de l’entrée de données de diffusion en continu](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Obtenir de l'aide sur les entrées de données de diffusion en continu
Pour obtenir une assistance, essayez notre [forum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Étapes suivantes
* [Présentation d’Azure Stream Analytics](stream-analytics-introduction.md)
* [Prise en main d’Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Mise à l’échelle des travaux Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Références sur le langage des requêtes d'Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Références sur l’API REST de gestion d’Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

