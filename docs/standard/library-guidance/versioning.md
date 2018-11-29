---
title: Gestion de version et bibliothèques .NET
description: Meilleures pratiques recommandées pour la gestion de version des bibliothèques .NET.
author: jamesnk
ms.author: mairaw
ms.date: 10/02/2018
ms.openlocfilehash: f95c8ade1f91af5c13184b839b327c9397c6fe5a
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "50187856"
---
# <a name="versioning"></a><span data-ttu-id="15e2a-103">Gestion de version</span><span class="sxs-lookup"><span data-stu-id="15e2a-103">Versioning</span></span>

<span data-ttu-id="15e2a-104">Une bibliothèque de logiciels est rarement terminée dans la version 1.0.</span><span class="sxs-lookup"><span data-stu-id="15e2a-104">A software library is rarely complete in version 1.0.</span></span> <span data-ttu-id="15e2a-105">Les bonnes bibliothèques évoluent au fil du temps, avec l’ajout de fonctionnalités, la résolution de bogues et l’amélioration des performances.</span><span class="sxs-lookup"><span data-stu-id="15e2a-105">Good libraries evolve over time, adding features, fixing bugs and improving performance.</span></span> <span data-ttu-id="15e2a-106">Il est important de publier de nouvelles versions d’une bibliothèque .NET, fournissant une valeur supplémentaire à chaque version, sans interrompre les utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="15e2a-106">It is important that you can release new versions of a .NET library, providing additional value with each version, without breaking existing users.</span></span>

## <a name="breaking-changes"></a><span data-ttu-id="15e2a-107">Modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="15e2a-107">Breaking changes</span></span>

<span data-ttu-id="15e2a-108">Pour plus d’informations sur la gestion des modifications avec rupture entre les versions, consultez [Modifications avec rupture](./breaking-changes.md).</span><span class="sxs-lookup"><span data-stu-id="15e2a-108">For information about handling breaking changes between versions, see [Breaking changes](./breaking-changes.md).</span></span>

## <a name="version-numbers"></a><span data-ttu-id="15e2a-109">Numéros de version</span><span class="sxs-lookup"><span data-stu-id="15e2a-109">Version numbers</span></span>

<span data-ttu-id="15e2a-110">Une bibliothèque .NET offre de nombreuses façons de spécifier une version.</span><span class="sxs-lookup"><span data-stu-id="15e2a-110">A .NET library has many ways to specify a version.</span></span> <span data-ttu-id="15e2a-111">Ces versions sont les plus importantes :</span><span class="sxs-lookup"><span data-stu-id="15e2a-111">These versions are the most important:</span></span>

### <a name="nuget-package-version"></a><span data-ttu-id="15e2a-112">Version de package NuGet</span><span class="sxs-lookup"><span data-stu-id="15e2a-112">NuGet package version</span></span>

<span data-ttu-id="15e2a-113">La [version du package NuGet](/nuget/reference/package-versioning) s’affiche sur NuGet.org, le gestionnaire de package NuGet Visual Studio, et est ajoutée au code source lorsque le package est utilisé.</span><span class="sxs-lookup"><span data-stu-id="15e2a-113">The [NuGet package version](/nuget/reference/package-versioning) is displayed on NuGet.org, the Visual Studio NuGet package manager, and is added to source code when the package is used.</span></span> <span data-ttu-id="15e2a-114">La version du package NuGet correspond au numéro de version que les utilisateurs voient généralement, et ils y feront référence lorsqu’ils évoquent la version d’une bibliothèque qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="15e2a-114">The NuGet package version is the version number users will commonly see, and they'll refer to it when they talk about the version of a library they're using.</span></span> <span data-ttu-id="15e2a-115">La version du package NuGet est utilisée par NuGet et n’a aucun effet sur le comportement d’exécution.</span><span class="sxs-lookup"><span data-stu-id="15e2a-115">The NuGet package version is used by NuGet and has no effect on runtime behavior.</span></span>

```xml
<PackageVersion>1.0.0-alpha1</PackageVersion>
```

<span data-ttu-id="15e2a-116">L’identificateur de package NuGet combiné avec la version du package NuGet sert à identifier un package dans NuGet.</span><span class="sxs-lookup"><span data-stu-id="15e2a-116">The NuGet package identifier combined with the NuGet package version is used to identify a package in NuGet.</span></span> <span data-ttu-id="15e2a-117">Par exemple, `Newtonsoft.Json` + `11.0.2`.</span><span class="sxs-lookup"><span data-stu-id="15e2a-117">For example, `Newtonsoft.Json` + `11.0.2`.</span></span> <span data-ttu-id="15e2a-118">Un package avec un suffixe est un package de préversion et affiche un comportement spécial qui le rend idéal pour les tests.</span><span class="sxs-lookup"><span data-stu-id="15e2a-118">A package with a suffix is a pre-release package and has special behavior that makes it ideal for testing.</span></span> <span data-ttu-id="15e2a-119">Pour plus d’informations, consultez [Packages de préversion](./nuget.md#pre-release-packages).</span><span class="sxs-lookup"><span data-stu-id="15e2a-119">For more information, see [Pre-release packages](./nuget.md#pre-release-packages).</span></span>

<span data-ttu-id="15e2a-120">Étant donné que la version du package NuGet est la version la plus visible pour les développeurs, il est judicieux de le mettre à jour à l’aide de l’outil [Gestion sémantique de version (SemVer)](https://semver.org/).</span><span class="sxs-lookup"><span data-stu-id="15e2a-120">Because the NuGet package version is the most visible version to developers, it's a good idea to update it using [Semantic Versioning (SemVer)](https://semver.org/).</span></span> <span data-ttu-id="15e2a-121">SemVer indique l’importance des modifications entre la mise en production et aide les développeurs à prendre une décision informée quant au choix de la version à utiliser.</span><span class="sxs-lookup"><span data-stu-id="15e2a-121">SemVer indicates the significance of changes between release and helps developers make an informed decision when choosing what version to use.</span></span> <span data-ttu-id="15e2a-122">Par exemple, le passage de `1.0` à `2.0` indique qu’il existe potentiellement des modifications avec rupture.</span><span class="sxs-lookup"><span data-stu-id="15e2a-122">For example, going from `1.0` to `2.0` indicates that there are potentially breaking changes.</span></span>

<span data-ttu-id="15e2a-123">**✔️ ENVISAGER** d’utiliser [SemVer 2.0.0](https://semver.org/) pour versionner votre package NuGet.</span><span class="sxs-lookup"><span data-stu-id="15e2a-123">**✔️ CONSIDER** using [SemVer 2.0.0](https://semver.org/) to version your NuGet package.</span></span>

<span data-ttu-id="15e2a-124">**✔️ À FAIRE** : Utiliser la version du package NuGet dans la documentation publique car il s’agit du numéro de version que les utilisateurs verront couramment.</span><span class="sxs-lookup"><span data-stu-id="15e2a-124">**✔️ DO** use the NuGet package version in public documentation as it's the version number that users will commonly see.</span></span>

<span data-ttu-id="15e2a-125">**✔️ À FAIRE** : Inclure un suffixe de préversion lors de la publication d’un package non stable.</span><span class="sxs-lookup"><span data-stu-id="15e2a-125">**✔️ DO** include a pre-release suffix when releasing a non-stable package.</span></span>

> <span data-ttu-id="15e2a-126">Comme les utilisateurs doivent s’abonner pour obtenir les packages de préversion, ils constateront que le package n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="15e2a-126">Users must opt in to getting pre-release packages, so they will understand that the package is not complete.</span></span>

### <a name="assembly-version"></a><span data-ttu-id="15e2a-127">Version de l’assembly</span><span class="sxs-lookup"><span data-stu-id="15e2a-127">Assembly version</span></span>

<span data-ttu-id="15e2a-128">La version d’assembly correspond à ce que le CLR utilise lors de l’exécution pour sélectionner la version d’un assembly à charger.</span><span class="sxs-lookup"><span data-stu-id="15e2a-128">The assembly version is what the CLR uses at runtime to select which version of an assembly to load.</span></span> <span data-ttu-id="15e2a-129">La sélection d’un assembly à l’aide du contrôle de version s’applique uniquement aux assemblys avec un nom fort.</span><span class="sxs-lookup"><span data-stu-id="15e2a-129">Selecting an assembly using versioning only applies to assemblies with a strong name.</span></span>

```xml
<AssemblyVersion>1.0.0.0</AssemblyVersion>
```

<span data-ttu-id="15e2a-130">Le Kit de développement Windows .NET Framework CLR exige une correspondance exacte pour charger un assembly avec un nom fort.</span><span class="sxs-lookup"><span data-stu-id="15e2a-130">The Windows .NET Framework CLR demands an exact match to load a strong named assembly.</span></span> <span data-ttu-id="15e2a-131">Par exemple, `Libary1, Version=1.0.0.0` a été compilé avec une référence à `Newtonsoft.Json, Version=11.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="15e2a-131">For example, `Libary1, Version=1.0.0.0` was compiled with a reference to `Newtonsoft.Json, Version=11.0.0.0`.</span></span> <span data-ttu-id="15e2a-132">L’infrastructure .NET Framework chargera uniquement cette version particulière `11.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="15e2a-132">The .NET Framework will only load that exact version `11.0.0.0`.</span></span> <span data-ttu-id="15e2a-133">Pour charger une version différente lors de l’exécution, une redirection de liaison doit être ajoutée au fichier de configuration de l’application .NET.</span><span class="sxs-lookup"><span data-stu-id="15e2a-133">To load a different version at runtime, a binding redirect must be added to the .NET application's config file.</span></span>

<span data-ttu-id="15e2a-134">Les noms forts combinés avec la version de l’assembly permet un [chargement strict de la version d’assembly](../../framework/app-domains/assembly-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="15e2a-134">Strong naming combined with assembly version enables [strict assembly version loading](../../framework/app-domains/assembly-versioning.md).</span></span> <span data-ttu-id="15e2a-135">Même si l’utilisation d’un nom fort pour une bibliothèque présente plusieurs avantages, cela génère souvent des exceptions d’exécution indiquant qu’un assembly est introuvable, et [nécessite des redirections de liaison](../../framework/configure-apps/redirect-assembly-versions.md) dans `app.config`/`web.config` pour corriger le problème.</span><span class="sxs-lookup"><span data-stu-id="15e2a-135">While strong naming a library has a number of benefits, it often results in runtime exceptions that an assembly can't be found and [requires binding redirects](../../framework/configure-apps/redirect-assembly-versions.md) in `app.config`/`web.config` to be fixed.</span></span> <span data-ttu-id="15e2a-136">Le chargement d’assembly .NET Core a été simplifié, et le CLR .NET Core chargera automatiquement les assemblys lors de l’exécution avec une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="15e2a-136">.NET Core assembly loading has been relaxed, and the .NET Core CLR will automatically load assemblies at runtime with a higher version.</span></span>

<span data-ttu-id="15e2a-137">**✔️ À ENVISAGER** : Inclure uniquement une version majeure dans AssemblyVersion.</span><span class="sxs-lookup"><span data-stu-id="15e2a-137">**✔️ CONSIDER** only including a major version in the AssemblyVersion.</span></span>

> <span data-ttu-id="15e2a-138">Par exemple, Library 1.0 et Library 1.0.1 afficher tous deux une valeur AssemblyVersion de `1.0.0.0`, tandis que Library 2.0 affiche une valeur AssemblyVersion de `2.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="15e2a-138">e.g. Library 1.0 and Library 1.0.1 both have an AssemblyVersion of `1.0.0.0`, while Library 2.0 has AssemblyVersion of `2.0.0.0`.</span></span> <span data-ttu-id="15e2a-139">Lorsque la version d’assembly change moins souvent, cela réduit les redirections de liaison.</span><span class="sxs-lookup"><span data-stu-id="15e2a-139">When the assembly version changes less often, it reduces binding redirects.</span></span>

<span data-ttu-id="15e2a-140">**✔️ À ENVISAGER** : Synchroniser le numéro de version majeure d’AssemblyVersion et la version du package NuGet.</span><span class="sxs-lookup"><span data-stu-id="15e2a-140">**✔️ CONSIDER** keeping the major version number of the AssemblyVersion and the NuGet package version in sync.</span></span>

> <span data-ttu-id="15e2a-141">La valeur AssemblyVersion est incluse dans certains messages d’information affichées à l’utilisateur, par exemple, le nom de l’assembly et les noms du type d’assembly qualifié dans les messages d’exception.</span><span class="sxs-lookup"><span data-stu-id="15e2a-141">The AssemblyVersion is included in some informational messages displayed to the user, e.g. the assembly name and assembly qualified type names in exception messages.</span></span> <span data-ttu-id="15e2a-142">Maintenir une relation entre les versions fournit des informations supplémentaires aux développeurs sur la version qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="15e2a-142">Maintaining a relationship between the versions provides more information to developers about which version they are using.</span></span>

<span data-ttu-id="15e2a-143">**❌ À ÉVITER** : Utiliser une version AssemblyVersion fixe.</span><span class="sxs-lookup"><span data-stu-id="15e2a-143">**❌ DO NOT** have a fixed AssemblyVersion.</span></span>

> <span data-ttu-id="15e2a-144">Bien qu'une valeur AssemblyVersion immuable évite d'avoir à lier des redirections, cela signifie qu'une seule version de l'assembly peut être installée dans le Global Assembly Cache (GAC).</span><span class="sxs-lookup"><span data-stu-id="15e2a-144">While an unchanging AssemblyVersion avoids the need for binding redirects, it means that only a single version of the assembly can be installed in the Global Assembly Cache (GAC).</span></span> <span data-ttu-id="15e2a-145">En outre, les applications qui font référence à l’assembly dans le GAC s’arrêtent si une autre application met à jour l’assembly GAC avec les dernières modifications.</span><span class="sxs-lookup"><span data-stu-id="15e2a-145">Also, the applications that reference the assembly in the GAC will break if another application updates the GAC assembly with breaking changes.</span></span>

### <a name="assembly-file-version"></a><span data-ttu-id="15e2a-146">Version du fichier d'assembly</span><span class="sxs-lookup"><span data-stu-id="15e2a-146">Assembly file version</span></span>

<span data-ttu-id="15e2a-147">La version du fichier d’assembly est utilisée pour afficher une version de fichier dans Windows et n’a aucun effet sur le comportement d’exécution.</span><span class="sxs-lookup"><span data-stu-id="15e2a-147">The assembly file version is used to display a file version in Windows and has no effect on runtime behavior.</span></span> <span data-ttu-id="15e2a-148">La définition de cette version est facultative.</span><span class="sxs-lookup"><span data-stu-id="15e2a-148">Setting this version is optional.</span></span> <span data-ttu-id="15e2a-149">Elle apparaît dans la boîte de dialogue Propriétés du fichier dans l’Explorateur Windows :</span><span class="sxs-lookup"><span data-stu-id="15e2a-149">It's visible in the File Properties dialog in Windows Explorer:</span></span>

```xml
<FileVersion>11.0.2.21924</FileVersion>
```

<span data-ttu-id="15e2a-150">![Explorateur Windows](./media/versioning/win-properties.png "Explorateur Windows")</span><span class="sxs-lookup"><span data-stu-id="15e2a-150">![Windows Explorer](./media/versioning/win-properties.png "Windows Explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="15e2a-151">Un avertissement de génération inoffensive est déclenché si cette version n’adopte pas le format `Major.Minor.Build.Revision`.</span><span class="sxs-lookup"><span data-stu-id="15e2a-151">An innocuous build warning is raised if this version does not follow the format `Major.Minor.Build.Revision`.</span></span> <span data-ttu-id="15e2a-152">Vous pouvez ignorer cet avertissement sans risque.</span><span class="sxs-lookup"><span data-stu-id="15e2a-152">The warning can be safely ignored.</span></span>

<span data-ttu-id="15e2a-153">**✔️ À ENVISAGER** : Inclure un numéro de build d’intégration continue en tant que révision AssemblyFileVersion.</span><span class="sxs-lookup"><span data-stu-id="15e2a-153">**✔️ CONSIDER** including a continuous integration build number as the AssemblyFileVersion revision.</span></span>

> <span data-ttu-id="15e2a-154">Par exemple, vous générez la version 1.0.0 de votre projet, et comme le numéro de build d’intégration continue est 99, votre AssemblyFileVersion est 1.0.0.99.</span><span class="sxs-lookup"><span data-stu-id="15e2a-154">For example, you are building version 1.0.0 of your project, and the continuous integration build number is 99 so your AssemblyFileVersion is 1.0.0.99.</span></span>

### <a name="assembly-informational-version"></a><span data-ttu-id="15e2a-155">Version des informations sur l'assembly</span><span class="sxs-lookup"><span data-stu-id="15e2a-155">Assembly informational version</span></span>

<span data-ttu-id="15e2a-156">La version des informations sur l’assembly est utilisée pour enregistrer des informations de version supplémentaires et n’a aucun effet sur le comportement d’exécution.</span><span class="sxs-lookup"><span data-stu-id="15e2a-156">The assembly informational version is used to record additional version information and has no effect on runtime behavior.</span></span> <span data-ttu-id="15e2a-157">La définition de cette version est facultative.</span><span class="sxs-lookup"><span data-stu-id="15e2a-157">Setting this version is optional.</span></span> <span data-ttu-id="15e2a-158">Si vous utilisez SourceLink, cette version sera définie sur la build avec la version du package NuGet ainsi qu’une version de contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="15e2a-158">If you're using SourceLink, this version will be set on build with the NuGet package version plus a source control version.</span></span> <span data-ttu-id="15e2a-159">Par exemple, `1.0.0-beta1+204ff0a` inclut le hachage de validation du code source sur lequel l’assembly repose.</span><span class="sxs-lookup"><span data-stu-id="15e2a-159">For example, `1.0.0-beta1+204ff0a` includes the commit hash of the source code the assembly was built from.</span></span> <span data-ttu-id="15e2a-160">Pour plus d’informations, consultez [SourceLink](./sourcelink.md).</span><span class="sxs-lookup"><span data-stu-id="15e2a-160">For more information, see [SourceLink](./sourcelink.md).</span></span>

```xml
<AssemblyInformationalVersion>The quick brown fox jumped over the lazy dog.</AssemblyInformationalVersion>
```

<span data-ttu-id="15e2a-161">**❌ À ÉVITER** : Définir vous-même la version des informations sur l’assembly.</span><span class="sxs-lookup"><span data-stu-id="15e2a-161">**❌ AVOID** setting the assembly informational version yourself.</span></span>

> <span data-ttu-id="15e2a-162">Autorisez SourceLink à générer automatiquement la version qui contient les métadonnées de contrôle source et NuGet.</span><span class="sxs-lookup"><span data-stu-id="15e2a-162">Allow SourceLink to automatically generate the version containing NuGet and source control metadata.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="15e2a-163">[Précédent](./publish-nuget-package.md)
[Suivant](./breaking-changes.md)</span><span class="sxs-lookup"><span data-stu-id="15e2a-163">[Previous](./publish-nuget-package.md)
[Next](./breaking-changes.md)</span></span>