---
title: Exemple JSON Azure Policy - Initiative de la stratégie de facturation en fonction d’étiquettes | Microsoft Docs
description: Cet exemple de stratégie JSON exige des valeurs d’étiquette spécifiées pour le nom de produit et le centre de coûts.
services: azure-policy
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-policy
ms.devlang: ''
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 10/30/2017
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 45dea9bb8ed6826d0c95a8a89e4270b82503a262
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2018
---
# <a name="billing-tags-policy-initiative"></a>Initiative de la stratégie de facturation en fonction d’étiquettes

Cette stratégie exige des valeurs d’étiquette spécifiées pour le nom de produit et le centre de coûts. Utilise des stratégies intégrées pour appliquer les étiquettes nécessaires. Spécifiez les valeurs nécessaires pour les balises.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Exemple de modèle

[!code-json[main](../../../policy-templates/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.json "Billing Tags Policy Initiative")]

Vous pouvez déployer ce modèle en utilisant le [portail Azure](#deploy-with-the-portal) ou avec [PowerShell](#deploy-with-powershell).

## <a name="deploy-with-the-portal"></a>Déployer avec le portail

[![Déploiement sur Azure](http://azuredeploy.net/deploybutton.png)](https://aka.ms/getpolicy)

## <a name="deploy-with-powershell"></a>Déployer avec PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$policydefinitions = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.definitions.json"
$policysetparameters = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.parameters.json"

$policyset= New-AzureRmPolicySetDefinition -Name "multiple-billing-tags" -DisplayName "Billing Tags Policy Initiative" -Description "Specify cost Center tag and product name tag" -PolicyDefinition $policydefinitions -Parameter $policysetparameters

New-AzureRmPolicyAssignment -PolicySetDefinition $policyset -Name <assignmentname> -Scope <scope>  -costCenterValue <required value for Cost Center tag> -productNameValue <required value for product Name tag>  -Sku @{"Name"="A1";"Tier"="Standard"}
```

### <a name="clean-up-powershell-deployment"></a>Nettoyer un déploiement PowerShell

Exécutez la commande suivante pour supprimer le groupe de ressources, la machine virtuelle et toutes les ressources associées.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="apply-tags-to-existing-resources"></a>Appliquer des balises aux ressources existantes

Après avoir attribué les stratégies, vous pouvez déclencher une mise à jour sur toutes les ressources existantes pour appliquer les stratégies de balise que vous avez ajoutées. Le script suivant conserve toutes les autres balises existant sur les ressources :

```powershell
$group = Get-AzureRmResourceGroup -Name "ExampleGroup" 

$resources = Find-AzureRmResource -ResourceGroupName $group.ResourceGroupName 

foreach($r in $resources)
{
    try{
        $r | Set-AzureRmResource -Tags ($a=if($r.Tags -eq $NULL) { @{}} else {$r.Tags}) -Force -UsePatchSemantics
    }
    catch{
        Write-Host  $r.ResourceId + "can't be updated"
    }
}
```


## <a name="next-steps"></a>Étapes suivantes

- Vous pouvez trouver d’autres exemples de modèles Azure Policy dans [Modèles pour Azure Policy](../json-samples.md).
