---
title: 'Azure AD : vue d’ensemble de la réinitialisation de mot de passe en libre-service Azure AD | Microsoft Docs'
description: En quoi la réinitialisation de mot de passe en libre-service d’Azure AD peut-elle être utile à votre organisation ?
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: e084db41cd199a9609e3edaf8b427a85ab2366b4
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2018
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>La réinitialisation de mot de passe en libre-service Azure AD pour les professionnels de l’informatique

Grâce à la réinitialisation de mot de passe en libre-service (SSPR) Azure Active Directory (Azure AD), les utilisateurs peuvent réinitialiser leurs mots de passe eux-mêmes quand et où ils en ont besoin. Dans le même temps, les administrateurs peuvent contrôler comment le mot de passe d’un utilisateur est réinitialisé. Les utilisateurs n’ont plus besoin d’appeler un support technique pour réinitialiser leur mot de passe. SSPR Azure AD inclut :

* **Modification de mot de passe en libre-service** : l’utilisateur connaît son mot de passe, mais il souhaite le modifier.
* **Réinitialisation de mot de passe en libre-service** : l’utilisateur ne parvient pas à se connecter et il souhaite réinitialiser son mot de passe à l’aide d’une ou de plusieurs des méthodes d’authentification validées suivantes :
   * Envoyer un SMS sur un téléphone mobile validé.
   * Appeler sur un téléphone mobile ou de bureau validé.
   * Envoyer un courrier électronique à un compte de messagerie secondaire validé.
   * Répondre à leurs questions de sécurité.
* **Déverrouillage de compte de libre-service** : l’utilisateur ne parvient pas se connecter avec son mot de passe et a été verrouillé. L’utilisateur souhaite déverrouiller son compte sans faire appel à l’administrateur à l’aide de ses méthodes d’authentification.

## <a name="why-choose-azure-ad-sspr"></a>Pourquoi choisir SSPR Azure AD ?

SSPR Azure AD vous aide à :

* **Réduction des coûts** : la réinitialisation de mot de passe assistée par le support technique représente généralement 20 % des appels d’un service informatique. 
* **Amélioration de l’expérience utilisateur final** et **réduction du volume de support technique**  en permettant aux utilisateurs finaux de résoudre leurs propres problèmes de mot de passe. Il n’est pas nécessaire d’appeler un support technique ni d’ouvrir une demande de support.
* **Encouragement à la mobilité** : les utilisateurs peuvent réinitialiser leur mot de passe où qu’ils se trouvent.
* **Maintien du contrôle** de la stratégie de sécurité. les administrateurs peuvent continuer à appliquer les stratégies en place.

Si vous êtes prêt, vous pouvez commencer à utiliser SSPR Azure AD à l’aide de notre [guide de démarrage rapide](quickstart-sspr.md). Vous pouvez rapidement autoriser vos utilisateurs à réinitialiser leurs mots de passe.

## <a name="azure-ad-sspr-availability"></a>Disponibilité de SSPR Azure AD

SSPR Azure AD est disponible dans trois niveaux, selon votre abonnement :

* **Azure AD gratuit** : les administrateurs chargés uniquement du cloud peuvent réinitialiser leurs mots de passe.
* **Azure AD Basic** ou tout **abonnement Office 365 payant** : les clients utilisant uniquement le cloud peuvent réinitialiser leurs propres mots de passe.
* **Azure AD Premium** : n’importe quel utilisateur ou administrateur, notamment les clients utilisant uniquement le cloud et les utilisateurs fédérés ou synchronisés par mot de passe, peuvent réinitialiser leurs propres mots de passe. L’écriture différée doit être activée pour les mots de passe locaux.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Tarifs, contrat SLA, mises à jour et feuille de route Azure AD

Vous trouverez plus d’informations sur les licences, les prix et les plans futurs sur les sites suivants :

* [Détails de tarification d’Azure AD](https://azure.microsoft.com/pricing/details/active-directory/)
* [Tarification d’Office 365](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [Contrats de niveau de service Azure](https://azure.microsoft.com/support/legal/sla/)
* [Contrat de niveau de service Microsoft Online Services](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [Mises à jour Azure](https://azure.microsoft.com/updates/)
* [Feuille de route Azure](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Étapes suivantes

* Prêt à vous lancer avec SSPR ? [Configurez la réinitialisation de mot de passe en libre-service Azure AD](quickstart-sspr.md).
* Planifiez un déploiement SSPR réussi pour vos utilisateurs à l’aide des conseils de notre [guide de lancement](howto-sspr-deployment.md).
* [Réinitialisez ou modifiez votre mot de passe](../active-directory-passwords-update-your-own-password.md).
* [Inscrivez-vous pour la réinitialisation du mot de passe en libre-service](../active-directory-passwords-reset-register.md).
