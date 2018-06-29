---
title: Commande dotnet tool update - CLI .NET Core
description: La commande dotnet tool update met à jour l’outil global .NET Core spécifié sur votre machine.
author: mairaw
ms.author: mairaw
ms.date: 05/29/2018
ms.openlocfilehash: 35a0bd0f85f0beed06d4250d8f195ce4fe4fcca4
ms.sourcegitcommit: bbf70abe6b46073148f78cbf0619de6092b5800c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34696687"
---
# <a name="dotnet-tool-update"></a><span data-ttu-id="73505-103">dotnet tool update</span><span class="sxs-lookup"><span data-stu-id="73505-103">dotnet tool update</span></span>

[!INCLUDE [topic-appliesto-net-core-21plus.md](../../../includes/topic-appliesto-net-core-21plus.md)]

## <a name="name"></a><span data-ttu-id="73505-104">Name</span><span class="sxs-lookup"><span data-stu-id="73505-104">Name</span></span>

<span data-ttu-id="73505-105">`dotnet tool update` - Met à jour l’[outil global .NET Core](global-tools.md) spécifié sur votre machine.</span><span class="sxs-lookup"><span data-stu-id="73505-105">`dotnet tool update` - Updates the specified [.NET Core Global Tool](global-tools.md) on your machine.</span></span>

## <a name="synopsis"></a><span data-ttu-id="73505-106">Résumé</span><span class="sxs-lookup"><span data-stu-id="73505-106">Synopsis</span></span>

```
dotnet tool update <PACKAGE_NAME> <-g|--global> [--configfile] [--framework] [-v|--verbosity]
dotnet tool update <PACKAGE_NAME> <--tool-path> [--configfile] [--framework] [-v|--verbosity]
dotnet tool update <-h|--help>
```

## <a name="description"></a><span data-ttu-id="73505-107">Description</span><span class="sxs-lookup"><span data-stu-id="73505-107">Description</span></span>

<span data-ttu-id="73505-108">La commande `dotnet tool update` vous offre un moyen de mettre à jour des outils globaux .NET Core sur votre machine vers la dernière version stable du package.</span><span class="sxs-lookup"><span data-stu-id="73505-108">The `dotnet tool update` command provides a way for you to update .NET Core Global Tools on your machine to the latest stable version of the package.</span></span> <span data-ttu-id="73505-109">La commande désinstalle et réinstalle un outil en le mettant à jour.</span><span class="sxs-lookup"><span data-stu-id="73505-109">The command uninstalls and reinstalls a tool, effectively updating it.</span></span> <span data-ttu-id="73505-110">Pour utiliser la commande, vous devez soit spécifier que vous voulez mettre à jour un outil à partir d’une installation à l’échelle de l’utilisateur en utilisant l’option `--global`, soit spécifier un chemin vers l’emplacement auquel l’outil est installé à l’aide de l’option `--tool-path`.</span><span class="sxs-lookup"><span data-stu-id="73505-110">To use the command, you either have to specify that you want to update a tool from a user-wide installation using the `--global` option or specify a path to where the tool is installed using the `--tool-path` option.</span></span>

## <a name="arguments"></a><span data-ttu-id="73505-111">Arguments</span><span class="sxs-lookup"><span data-stu-id="73505-111">Arguments</span></span>

`PACKAGE_NAME`

<span data-ttu-id="73505-112">Nom/ID du package NuGet qui contient l’outil global .NET Core à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="73505-112">Name/ID of the NuGet package that contains the .NET Core Global Tool to update.</span></span> <span data-ttu-id="73505-113">Vous pouvez trouver le nom du package à l’aide de la commande [dotnet tool list](dotnet-tool-list.md).</span><span class="sxs-lookup"><span data-stu-id="73505-113">You can find the package name using the [dotnet tool list](dotnet-tool-list.md) command.</span></span>

## <a name="options"></a><span data-ttu-id="73505-114">Options</span><span class="sxs-lookup"><span data-stu-id="73505-114">Options</span></span>

`--add-source <SOURCE>`

<span data-ttu-id="73505-115">Ajoute une source de package NuGet supplémentaire à utiliser pendant l’installation.</span><span class="sxs-lookup"><span data-stu-id="73505-115">Adds an additional NuGet package source to use during installation.</span></span>

`--configfile <FILE>`

<span data-ttu-id="73505-116">Fichier de configuration NuGet (*nuget.config*) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="73505-116">The NuGet configuration (*nuget.config*) file to use.</span></span>

`--framework <FRAMEWORK>`

<span data-ttu-id="73505-117">Spécifie le [framework cible](../../standard/frameworks.md) pour lequel mettre à jour l’outil.</span><span class="sxs-lookup"><span data-stu-id="73505-117">Specifies the [target framework](../../standard/frameworks.md) to update the tool for.</span></span>

`-g|--global`

<span data-ttu-id="73505-118">Spécifie que la mise à jour concerne un outil à l’échelle de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="73505-118">Specifies that the update is for a user-wide tool.</span></span> <span data-ttu-id="73505-119">Non combinable avec l’option `--tool-path`.</span><span class="sxs-lookup"><span data-stu-id="73505-119">Can't be combined with the `--tool-path` option.</span></span> <span data-ttu-id="73505-120">Si vous ne spécifiez pas cette option, vous devez spécifier l’option `--tool-path`.</span><span class="sxs-lookup"><span data-stu-id="73505-120">If you don't specify this option, you must specify the `--tool-path` option.</span></span>

`-h|--help`

<span data-ttu-id="73505-121">Affiche une aide brève pour la commande.</span><span class="sxs-lookup"><span data-stu-id="73505-121">Prints out a short help for the command.</span></span>

`--tool-path <PATH>`

<span data-ttu-id="73505-122">Spécifie l’emplacement d’installation de l’outil global.</span><span class="sxs-lookup"><span data-stu-id="73505-122">Specifies the location where the Global Tool is installed.</span></span> <span data-ttu-id="73505-123">Le chemin peut être absolu ou relatif.</span><span class="sxs-lookup"><span data-stu-id="73505-123">PATH can be absolute or relative.</span></span> <span data-ttu-id="73505-124">Non combinable avec l’option `--global`.</span><span class="sxs-lookup"><span data-stu-id="73505-124">Can't be combined with the `--global` option.</span></span> <span data-ttu-id="73505-125">Si vous ne spécifiez pas cette option, vous devez spécifier l’option `--global`.</span><span class="sxs-lookup"><span data-stu-id="73505-125">If you don't specify this option, you must specify the `--global` option.</span></span>

`-v|--verbosity <LEVEL>`

<span data-ttu-id="73505-126">Définit le niveau de détail de la commande.</span><span class="sxs-lookup"><span data-stu-id="73505-126">Sets the verbosity level of the command.</span></span> <span data-ttu-id="73505-127">Les valeurs autorisées sont `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]` et `diag[nostic]`.</span><span class="sxs-lookup"><span data-stu-id="73505-127">Allowed values are `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]`, and `diag[nostic]`.</span></span>

## <a name="examples"></a><span data-ttu-id="73505-128">Exemples</span><span class="sxs-lookup"><span data-stu-id="73505-128">Examples</span></span>

<span data-ttu-id="73505-129">Met à jour l’outil global [dotnetsay](https://www.nuget.org/packages/dotnetsay/) :</span><span class="sxs-lookup"><span data-stu-id="73505-129">Updates the [dotnetsay](https://www.nuget.org/packages/dotnetsay/) Global Tool:</span></span>

`dotnet tool update -g dotnetsay`

<span data-ttu-id="73505-130">Met à jour l’outil global [dotnetsay](https://www.nuget.org/packages/dotnetsay/) situé dans un dossier Windows spécifique :</span><span class="sxs-lookup"><span data-stu-id="73505-130">Updates the [dotnetsay](https://www.nuget.org/packages/dotnetsay/) Global Tool located on a specific Windows folder:</span></span>

`dotnet tool update dotnetsay --tool-path c:\global-tools`

<span data-ttu-id="73505-131">Met à jour l’outil global [dotnetsay](https://www.nuget.org/packages/dotnetsay/) situé dans un dossier Linux/macOS spécifique :</span><span class="sxs-lookup"><span data-stu-id="73505-131">Updates the [dotnetsay](https://www.nuget.org/packages/dotnetsay/) Global Tool located on a specific Linux/macOS folder:</span></span>

`dotnet tool update dotnetsay --tool-path ~/bin`

## <a name="see-also"></a><span data-ttu-id="73505-132">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="73505-132">See also</span></span>

[<span data-ttu-id="73505-133">Outils globaux .NET Core</span><span class="sxs-lookup"><span data-stu-id="73505-133">.NET Core Global Tools</span></span>](global-tools.md)