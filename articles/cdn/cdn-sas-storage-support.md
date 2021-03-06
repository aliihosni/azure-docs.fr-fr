---
title: Utilisation d’Azure CDN avec SAP | Microsoft Docs
description: Azure CDN prend en charge l’utilisation de la signature d’accès partagé (SAP) pour accorder un accès limité à des conteneurs de stockage privé.
services: cdn
documentationcenter: ''
author: dksimpson
manager: ''
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: v-deasim
ms.openlocfilehash: 8b609beb67cfb0873bf9926ca648f0ad5568ad2e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2018
---
# <a name="using-azure-cdn-with-sas"></a>Utilisation d’Azure CDN avec SAP

Quand vous configurez un compte de stockage pour Azure Content Delivery Network (CDN) en vue de l’utiliser comme cache de contenu, par défaut toute personne connaissant les URL de vos conteneurs de stockage peut accéder aux fichiers que vous avez chargés. Pour protéger les fichiers dans votre compte de stockage, vous pouvez rendre privé l’accès à vos conteneurs de stockage. Toutefois, si vous le faites, personne ne peut accéder à vos fichiers. 

Si vous souhaitez accorder un accès limité à des conteneurs de stockage privé, vous pouvez utiliser la fonctionnalité de signature d’accès partagé (SAP) de votre compte de stockage Azure. Une SAP est un URI qui octroie des droits d’accès restreints à vos ressources Stockage Azure sans exposer votre clé de compte. Vous pouvez fournir une SAP aux clients auxquels vous préférez ne pas donner votre clé de compte de stockage, mais auxquels vous souhaitez déléguer l’accès à certaines ressources de compte de stockage. Quand vous distribuez un URI de signature d’accès partagé à ces clients, vous leur accordez l’accès à une ressource pour une période spécifiée.
 
Avec une signature SAP, vous pouvez définir différents paramètres d’accès à un objet blob, tels que les heures de début et d’expiration, les autorisations (lecture/écriture) et les plages d’adresses IP. Cet article décrit comment utiliser la SAP avec Azure CDN. Pour plus d’informations sur les SAP, notamment sur la création de leurs options de paramètre, consultez [Utilisation des signatures d’accès partagé (SAP)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Configuration d’Azure CDN pour fonctionner avec une SAP de stockage
Les trois options suivantes sont recommandées pour l’utilisation de SAP avec Azure CDN. Toutes les options partent du principe que vous avez déjà créé une SAP opérationnelle (voir les prérequis). 
 
### <a name="prerequisites"></a>Prérequis

Pour commencer, créez un compte de stockage, puis générez une SAP pour votre ressource. Vous pouvez générer deux types de signatures d’accès partagé : une SAP de service ou une SAP de compte. Pour plus d’informations, consultez [Types de signatures d’accès partagé](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Après avoir généré un jeton SAP, vous pouvez accéder à votre fichier de stockage d’objets blob en ajoutant `?sv=<SAS token>` à l’URL. Cette URL a le format suivant : 

`https://<account name>.blob.core.windows.net/<container>/<file>?sv=<SAS token>`
 
Par exemple : 
 ```
https://democdnstorage1.blob.core.windows.net/container1/demo.jpg?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Pour plus d’informations sur la définition des paramètres, consultez [Considérations relatives aux paramètres SAP](#sas-parameter-considerations) et [Paramètres de la signature d’accès partagé (SAP)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![Paramètres de SAP CDN](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-azure-cdn"></a>Option 1 : Utilisation de SAP avec transfert direct vers le stockage blob à partir d’Azure CDN

Cette option est la plus simple. Elle utilise un jeton SAP unique, qui est passé d’Azure CDN au serveur d’origine. Elle est prise en charge par **Azure CDN de Verizon** et **Azure CDN d’Akamai**. 
 
1. Sélectionnez un point de terminaison, sélectionnez **Règles de mise en cache**, puis sélectionnez **Mettre en cache chaque URL unique** dans la liste **Mise en cache de chaîne de requête**.

    ![Règles de mise en cache de CDN](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Après avoir configuré SAP sur votre compte de stockage, utilisez le jeton SAP avec l’URL d’Azure CDN pour accéder au fichier. 
   
   L’URL qui en résulte a le format suivant : `https://<endpoint hostname>.azureedge.net/<container>/<file>?sv=<SAS token>`

   Par exemple :    
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Affinez la durée du cache à l’aide de règles de mise en cache ou en ajoutant des en-têtes `Cache-Control` au serveur d’origine. Étant donné qu’Azure CDN traite le jeton SAP comme une chaîne de requête simple, la bonne pratique consiste à définir une durée de mise en cache qui expire au plus tard au moment de l’expiration de la signature SAP. Dans le cas contraire, si un fichier est mis en cache pour une durée plus longue que celle pendant laquelle la signature SAP est active, le fichier peut être accessible à partir du serveur d’origine Azure CDN après l’expiration de la SAP. Si ce cas se produit et que vous souhaitez rendre votre fichier de mise en cache inaccessible, vous devez effectuer une opération de vidage sur le fichier afin de le supprimer du cache. Pour plus d’informations sur la définition de la durée du cache sur Azure CDN, consultez [Contrôler le comportement de mise en cache d’Azure CDN avec des règles de mise en cache](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-security-token-using-a-rewrite-rule"></a>Option 2 : Jeton de sécurité CDN masqué à l’aide d’une règle de réécriture
 
Avec cette option, vous pouvez sécuriser le stockage blob d’origine sans qu’un utilisateur Azure CDN ait besoin de recourir à un jeton SAP dans l’URL. Vous pouvez utiliser cette option si vous n’avez pas besoin de restrictions d’accès spécifiques pour le fichier, mais que vous voulez empêcher les utilisateurs d’accéder à l’origine du stockage directement afin d’accélérer le temps de déchargement d’Azure CDN. Cette option est disponible uniquement pour les profils **Azure CDN Premium de Verizon**. 
 
1. Utilisez le [moteur de règles](cdn-rules-engine.md) pour créer une règle de réécriture d’URL. La propagation des nouvelles règles prend environ 90 minutes.

   ![Bouton Gérer du CDN](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Bouton de moteur de règles de CDN](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   L’exemple de règle de réécriture d’URL suivant utilise un modèle d’expression régulière avec un groupe de capture et un point de terminaison nommé *storagedemo* :
   
   Source :   
   `(/test/*.)`
   
   Destination :   
   ```
   $1?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```

   ![Règle de réécriture d’URL de CDN](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-2.png)

2. Une fois que la nouvelle règle devient active, vous pouvez accéder au fichier sur Azure CDN sans utiliser de jeton SAP dans l’URL, dans le format suivant : `https://<endpoint hostname>.azureedge.net/<container>/<file>`
 
   Par exemple :    
   `https://demoendpoint.azureedge.net/container1/demo.jpg`
       
   Notez que toute personne peut désormais accéder aux fichiers sur le point de terminaison CDN, même si elle n’utilise pas de jeton SAP. 

3. Affinez la durée du cache à l’aide de règles de mise en cache ou en ajoutant des en-têtes `Cache-Control` au serveur d’origine. Étant donné qu’Azure CDN traite le jeton SAP comme une chaîne de requête simple, la bonne pratique consiste à définir une durée de mise en cache qui expire au plus tard au moment de l’expiration de la signature SAP. Dans le cas contraire, si un fichier est mis en cache pour une durée plus longue que celle pendant laquelle la signature SAP est active, le fichier peut être accessible à partir du serveur d’origine Azure CDN après l’expiration de la SAP. Si ce cas se produit et que vous souhaitez rendre votre fichier de mise en cache inaccessible, vous devez effectuer une opération de vidage sur le fichier afin de le supprimer du cache. Pour plus d’informations sur la définition de la durée du cache sur Azure CDN, consultez [Contrôler le comportement de mise en cache d’Azure CDN avec des règles de mise en cache](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Option 3 : Utilisation de l’authentification de jeton de sécurité de CDN avec une règle de réécriture

Cette option est la plus sécurisée et personnalisable. Pour utiliser l’authentification de jeton de sécurité d’Azure CDN, vous devez avoir un profil **Azure CDN Premium de Verizon**. L’accès du client est basé sur les paramètres de sécurité que vous avez définis sur le jeton de sécurité. Toutefois, si la signature SAP devient non valide, Azure CDN ne peut plus revalider le contenu à partir du serveur d’origine.

1. [Créez un jeton de sécurité Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) et activez-le à l’aide du moteur de règles pour le point de terminaison de CDN et du chemin où les utilisateurs peuvent accéder au fichier.

   Une URL de jeton de sécurité présente le format suivant :   
   `https://<endpoint hostname>.azureedge.net/<container>/<file>?<security_token>`
 
   Par exemple :    
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg?a4fbc3710fd3449a7c99986bkquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   Les options de paramètres pour l’authentification de jeton de sécurité diffèrent de celles pour un jeton SAP. Si vous choisissez d’utiliser un délai d’expiration quand vous créez un jeton de sécurité, affectez-lui la même valeur que le délai d’expiration du jeton SAP. Cela garantit le caractère prévisible du délai d’expiration. 
 
2. Utilisez le [moteur de règles](cdn-rules-engine.md) pour créer une règle de réécriture d’URL visant à activer l’accès du jeton SAP à tous les objets blob dans le conteneur. La propagation des nouvelles règles prend environ 90 minutes.

   L’exemple de règle de réécriture d’URL suivant utilise un modèle d’expression régulière avec un groupe de capture et un point de terminaison nommé *storagedemo* :
   
   Source :   
   `(/test/*.)`
   
   Destination :   
   ```
   $1&sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```

   ![Règle de réécriture d’URL de CDN](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-3.png)

3. Si vous renouvelez la signature SAP, mettez à jour la règle de réécriture d’URL avec le nouveau jeton SAP. 

## <a name="sas-parameter-considerations"></a>Considérations relatives aux paramètres SAP

Azure CDN ne peut pas changer son comportement de remise en se basant sur les paramètres SAP, car il ne le voit pas. Les restrictions de paramètres définies s’appliquent uniquement aux requêtes envoyées par Azure CDN au serveur d’origine, et non aux requêtes du client vers Azure CDN. Cette distinction est importante quand vous définissez des paramètres SAP. Si ces fonctionnalités avancées sont nécessaires et que vous utilisez [l’Option 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), définissez les restrictions appropriées sur le jeton de sécurité Azure CDN.

| Nom du paramètre SAP | Description |
| --- | --- |
| Démarrer | Heure à laquelle Azure CDN peut commencer à accéder au fichier blob. En raison des variations d’horloges (quand un signal d’horloge arrive à des moments différents pour différents composants), vous devez choisir une heure 15 minutes plus tôt si vous souhaitez que la ressource soit disponible immédiatement. |
| Terminer | Heure après laquelle Azure CDN ne peut plus accéder au fichier blob. Les fichiers précédemment mis en cache sur Azure CDN sont toujours accessibles. Pour contrôler le délai d’expiration de fichier, définissez le délai d’expiration approprié sur le jeton de sécurité Azure CDN ou nettoyez la ressource. |
| Adresses IP autorisées | facultatif. Si vous utilisez **Azure CDN de Verizon**, vous pouvez affecter à ce paramètre les plages définies dans [Azure CDN from Verizon Edge Server IP Ranges (Plages d’adresses IP de serveur Edge Azure CDN de Verizon)](https://msdn.microsoft.com/library/mt757330.aspx). Si vous utilisez **CDN Azure d’Akamai**, vous ne pouvez pas définir le paramètre de plages IP, car les adresses IP ne sont pas statiques.|
| Protocoles autorisés | Protocole(s) autorisé(s) pour une requête effectuée avec la SAP de compte. Le paramètre HTTPS est recommandé.|

## <a name="see-also"></a>Voir aussi
- [Utilisation des signatures d’accès partagé (SAP)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Signatures d’accès partagé, partie 2 : créer et utiliser une signature d’accès partagé avec Stockage Blob](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Sécurisation des ressources Azure Content Delivery Network avec l’authentification du jeton](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
