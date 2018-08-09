---
title: Implémentation de passerelles d’API avec Ocelot
description: Découvrez comment implémenter des passerelles d’API avec Ocelot et comment utiliser Ocelot dans un environnement basé sur un conteneur.
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 07/03/2018
ms.openlocfilehash: 7400603aa11b2a741db727c97c2e4b2a17268ac0
ms.sourcegitcommit: 59b51cd7c95c75be85bd6ef715e9ef8c85720bac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37878663"
---
# <a name="implementing-api-gateways-with-ocelot"></a>Implémentation de passerelles d’API avec Ocelot

L’application de microservices de référence [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) utilise [Ocelot](https://github.com/ThreeMammals/Ocelot) parce qu’Ocelot est une passerelle d’API simple et légère que vous pouvez déployer n’importe où avec vos microservices/conteneurs, comme dans les environnements suivants utilisés par eShopOnContainers.

- Hôte Docker, sur votre PC de développement local, localement ou dans le cloud.
- Cluster Kubernetes, localement ou dans le cloud managé, tel qu’Azure Kubernetes Service (AKS).
- Cluster Service Fabric, localement ou dans le cloud.
- Cloud Service Fabric, comme PaaS/Serverless dans Azure.

## <a name="architect-and-design-your-api-gateways"></a>Structurer et concevoir les passerelles d’API

Le diagramme d’architecture suivant illustre l’implémentation de passerelles d’API avec Ocelot dans eShopOnContainers.

![Diagramme d’architecture eShopOnContainers illustrant les applications clientes, les microservices et les passerelles d’API intermédiaires](./media/image28.png)

**Figure 8-27.** Architecture d’eShopOnContainers avec passerelles d’API

Ce diagramme montre comment l’application entière est déployée sur un PC de développement ou hôte Docker unique avec « Docker pour Windows » ou « Docker pour Mac ». Toutefois, le déploiement dans un autre orchestrateur serait assez similaire, mais n’importe quel conteneur dans le diagramme pourrait être monté en charge dans l’orchestrateur. 

De plus, les ressources de l’infrastructure telles que les bases de données, le cache et les répartiteurs de messages doivent être déchargées à partir de l’orchestrateur et déployées dans des systèmes hautement disponibles pour l’infrastructure, tels qu’Azure SQL Database, Azure Cosmos DB, Azure Redis, Azure Service Bus ou toute solution de clustering à haute disponibilité en local.

Comme vous pouvez le constater dans le diagramme, la présence de plusieurs passerelles d’API permet à plusieurs équipes de développement d’être autonomes (dans ce cas, les fonctionnalités Marketing et les fonctionnalités d’achat [Shopping]) lors du développement et du déploiement de leurs microservices et de leurs propres passerelles d’API associées. 

Si vous aviez une passerelle d’API monolithique unique, cela signifierait qu’un point unique serait mis à jour par plusieurs équipes de développement, ce qui associerait tous les microservices à une seule partie de l’application.

En allant beaucoup plus loin dans la conception, parfois, une passerelle d’API de granularité fine peut également être limitée à un seul microservice métier selon l’architecture choisie. Le fait que les limites de la passerelle d’API soient dictées par l’activité ou le domaine vous aide à obtenir une meilleure conception. 

Par exemple, une granularité fine au niveau de la passerelle d’API peut être particulièrement utile pour les applications d’interface utilisateur composites plus avancées qui sont basées sur des microservices, car le concept d’une passerelle d’API à granularité fine est similaire à un service de composition d’interface utilisateur. 

Pour plus d’informations sur les services de composition d’interface utilisateur, consultez [Création d’une interface utilisateur composite basée sur des microservices](https://docs.microsoft.com/dotnet/standard/microservices-architecture/architect-microservice-container-applications/microservice-based-composite-ui-shape-layout).

Comme élément clé à retenir, pour de nombreuses applications moyennes ou grandes, l’utilisation d’un produit de passerelle d’API personnalisée constitue généralement une bonne approche. Toutefois, pas comme agrégateur monolithique unique ni comme passerelle d’API personnalisée, centrale, unique, sauf si cette passerelle API autorise plusieurs zones de configuration indépendantes pour les équipes de développement qui créent des microservices autonomes.

### <a name="sample-microservicescontainers-to-reroute-through-the-api-gateways"></a>Exemples de microservices/conteneurs à réacheminer via les passerelles d’API

Par exemple, `eShopOnContainers` a environ six types de microservices internes qui doivent être publiés via les passerelles d’API, comme illustré à la figure suivante.
 
![](./media/image29.png)

**Figure 8-28.** Dossiers de microservices dans une solution eShopOnContainers dans Visual Studio

En ce qui concerne le service d’identité (Identity), dans sa conception, il est tenu à l’écart du routage des passerelles d’API, car il est le seul problème transversal du système, mais Ocelot permet également de l’inclure dans le cadre des listes de redirection.

Tous ces services sont actuellement implémentés en tant que services d’API web ASP.NET Core, comme vous pouvez le voir au niveau du code. Concentrons-nous sur l’un de ces microservices, tel que le code du microservice de catalogue (Catalog).

![](./media/image30.png)

**Figure 8-29.** Exemple de microservice d’API web (microservice Catalog)

Vous pouvez voir que le microservice Catalog est un projet d’API web ASP.NET Core standard avec plusieurs contrôleurs et méthodes, comme dans le code suivant.

```csharp
[HttpGet]
[Route("items/{id:int}")]
[ProducesResponseType((int)HttpStatusCode.NotFound)]
[ProducesResponseType(typeof(CatalogItem),(int)HttpStatusCode.OK)]
public async Task<IActionResult> GetItemById(int id)
{
    if (id <= 0)
    {
        return BadRequest();
    }
    var item = await _catalogContext.CatalogItems.
                                          SingleOrDefaultAsync(ci => ci.Id == id);
    //…

    if (item != null)
    {
        return Ok(item);
    }
    return NotFound();
}
```
La requête HTTP finit par exécuter ce type de code C# qui accède à la base de données du microservice, ainsi qu’une autre action supplémentaire.

En ce qui concerne l’URL de microservice, quand les conteneurs sont déployés sur votre PC de développement local (hôte Docker local), le conteneur de chaque microservice a toujours un port interne, généralement le port 80, spécifié dans son fichier Dockerfile, comme dans le fichier Dockerfile suivant :

```
FROM microsoft/aspnetcore:2.0.5 AS base
WORKDIR /app
EXPOSE 80
```

Le port 80 indiqué dans le code est interne à l’hôte Docker et n’est donc pas accessible par les applications clientes. Les applications clientes peuvent accéder uniquement aux ports externes (le cas échéant) publiés lors du déploiement avec `docker-compose`.

Ces ports externes ne doivent pas être publiés lors du déploiement dans un environnement de production. C’est précisément la raison pour laquelle vous souhaitez utiliser la passerelle d’API, afin d’éviter la communication directe entre les applications clientes et les microservices.

Toutefois, lors du développement, vous souhaitez accéder directement au microservice/conteneur et l’exécuter via Swagger. C’est pourquoi dans eShopOnContainers, les ports externes sont encore spécifiés même quand ils ne sont pas utilisés par la passerelle d’API ou les applications clientes.

Voici un exemple de fichier `docker-compose.override.yml` pour le microservice Catalog :

```
catalog.api:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - ASPNETCORE_URLS=http://0.0.0.0:80
    - ConnectionString=YOUR_VALUE
    - ... Other Environment Variables
  ports:
    - "5101:80"   # Important: In a production environment you should remove the external port (5101) kept here for microservice debugging purposes. 
                  # The API Gateway redirects and access through the internal port (80).
```

Vous pouvez voir dans la configuration docker-compose.override.yml que le port interne pour le conteneur Catalog est le port 80, mais que le port 5101 est utilisé pour l’accès externe. Toutefois, ce port ne doit pas être utilisé par l’application lors de l’utilisation d’une passerelle d’API, mais seulement pour déboguer, exécuter et tester le seul microservice Catalog.

Normalement, vous n’effectuez pas de déploiement avec docker-compose dans un environnement de production, car le bon environnement de déploiement en production pour les microservices est un orchestrateur comme Kubernetes ou Service Fabric. Lors du déploiement dans ces environnements, vous utilisez des fichiers de configuration différents dans lesquels vous ne publiez directement aucun port externe pour les microservices, mais vous utilisez toujours le proxy inverse à partir de la passerelle d’API.

Exécutez le microservice de catalogue sur votre hôte Docker local, soit en exécutant la solution eShopOnContainers complète à partir de Visual Studio (elle exécute tous les services dans les fichiers docker-compose), soit en démarrant simplement le microservice Catalog avec la commande docker-compose suivante dans CMD ou PowerShell positionné dans le dossier contenant les fichiers `docker-compose.yml` et docker-compose.override.yml.

```
docker-compose run --service-ports catalog.api
```

Cette commande exécute uniquement le conteneur de service catalog.api, ainsi que les dépendances qui sont spécifiées dans docker-compose.yml. Dans ce cas, le conteneur SQL Server et le conteneur RabbitMQ.

Ensuite, vous pouvez accéder directement au microservice Catalog et voir ses méthodes via l’interface utilisateur Swagger, en accédant directement via ce port « externe », dans ce cas `http://localhost:5101`. 

![](./media/image31.png)

**Figure 8-30.** Test du microservice Catalog avec son interface utilisateur Swagger

À ce stade, vous pouvez définir un point d’arrêt dans le code C# dans Visual Studio, tester le microservice avec les méthodes exposées dans l’interface utilisateur Swagger et enfin tout nettoyer avec la commande `docker-compose down`.

Toutefois, cette communication à accès direct au microservice, dans ce cas via le port externe 5101, est précisément ce que vous voulez éviter dans votre application. Et vous pouvez l’éviter en définissant le niveau supplémentaire d’indirection de la passerelle d’API (dans ce cas, Ocelot). De cette façon, l’application cliente n’accédera pas directement au microservice.

## <a name="implementing-your-api-gateways-with-ocelot"></a>Implémentation des passerelles d’API avec Ocelot

Ocelot est essentiellement un ensemble de middlewares que vous pouvez appliquer dans un ordre spécifique.

Ocelot est conçu pour fonctionner uniquement avec ASP.NET Core. Il cible netstandard2.0 et peut donc être utilisé partout où .NET Standard 2.0 est pris en charge, notamment le runtime .NET Core 2.0 et le runtime .NET Framework 4.6.1 et versions ultérieures.

Installez Ocelot et ses dépendances dans votre projet ASP.NET Core avec le [package NuGet d’Ocelot](https://www.nuget.org/packages/Ocelot/), à partir de Visual Studio.

```
Install-Package Ocelot
```

Dans eShopOnContainers, son implémentation des passerelles d’API est un projet WebHost ASP.NET Core simple et les middlewares d’Ocelot traitent toutes les fonctionnalités des passerelles d’API, comme illustré dans l’image suivante :

![](./media/image32.png)

**Figure 8-31.** Projet de base OcelotApiGw dans eShopOnContainers

Ce projet WebHost ASP.NET Core est composé de deux fichiers simples, `Program.cs` et `Startup.cs`.

Le fichier Program.cs doit simplement créer et configurer la fonction BuildWebHost ASP.NET Core standard. 

```csharp
namespace OcelotApiGw
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args)
        {
            var builder = WebHost.CreateDefaultBuilder(args);

            builder.ConfigureServices(s => s.AddSingleton(builder))                
                                                          .ConfigureAppConfiguration(
                              ic => ic.AddJsonFile(Path.Combine("configuration",
                                                                "configuration.json")))
                                                                .UseStartup<Startup>();
            var host = builder.Build();
            return host;
        }
    }
}
```

Le point important ici pour Ocelot est le fichier `configuration.json` que vous devez fournir au générateur via la méthode `AddJsonFile()`. Ce fichier `configuration.json` vous permet de spécifier tous les réacheminements des passerelles d’API, ce qui signifie les points de terminaison externes avec des ports spécifiques et les points de terminaison internes corrélés, généralement à l’aide de ports différents.

```
{
    "ReRoutes": [],
    "GlobalConfiguration": {}
}
```

La configuration comprend deux sections. Un tableau de réacheminements et une configuration globale. Les réacheminements sont les objets qui indiquent à Ocelot comment traiter une demande en amont. La configuration globale permet le remplacement des paramètres spécifiques aux réacheminements. Elle est utile si vous ne souhaitez pas gérer un grand nombre de paramètres spécifiques aux réacheminements.

Voici un exemple simplifié de fichier de [configuration des réacheminements](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/ApiGateways/Web.Bff.Shopping/apigw/configuration.json) provenant d’une des passerelles d’API d’eShopOnContainers.

```
{
  "ReRoutes": [
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "catalog.api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/c/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ]
    },
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket.api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
    }
    
  ],
    "GlobalConfiguration": {
      "RequestIdKey": "OcRequestId",
      "AdministrationPath": "/administration"
    }
  }
```

La fonctionnalité principale d’une passerelle d’API Ocelot consiste à prendre les requêtes HTTP entrantes et à les transférer vers un service en aval, actuellement sous la forme d’une autre requête HTTP. Ocelot décrit le routage d’une demande vers une autre en tant que réacheminement.

Par exemple, concentrons-nous sur l’un des réacheminements dans le fichier configuration.json mentionné plus haut, la configuration pour le microservice de panier d’achat (Basket).

```
{
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket.api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
}
```

Les éléments DownstreamPathTemplate, Scheme et DownstreamHostAndPorts composent l’URL de microservice interne vers laquelle cette demande sera transmise. 

Le port est le port interne utilisé par le service. Lorsque vous utilisez des conteneurs, le port spécifié dans son fichier Dockerfile.

L’élément `Host` est un nom de service qui dépend de la résolution de noms de service que vous utilisez. Lors de l’utilisation de docker-compose, les noms des services sont fournis par l’hôte Docker, lequel utilise les noms de service fournis dans les fichiers docker-compose. Si vous utilisez un orchestrateur comme Kubernetes ou Service Fabric, ce nom doit être résolu par la résolution de noms ou DNS fournie par chaque orchestrateur.

DownstreamHostAndPorts est un tableau qui contient l’hôte et le port de tous les services en aval vers lesquels vous souhaitez transférer des demandes. Généralement, il contient uniquement une entrée, mais vous pouvez parfois équilibrer la charge des demandes à vos services en aval, et Ocelot vous permet d’ajouter plusieurs entrées puis de sélectionner un équilibreur de charge. Toutefois, si vous utilisez Azure et un orchestrateur quelconque, il est probablement plus judicieux d’équilibrer la charge avec l’infrastructure d’orchestrateur et le cloud.

UpstreamPathTemplate est l’URL dont Ocelot se servira pour identifier l’URL DownstreamPathTemplate à utiliser pour une demande donnée à partir du client. Enfin, la méthode UpstreamHttpMethod est utilisée pour qu’Ocelot puisse faire la distinction entre les différentes demandes (GET, POST, PUT) sur la même URL.

À ce stade, vous pourriez avoir une seule passerelle d’API Ocelot (WebHost ASP.NET Core) utilisant un ou [plusieurs fichiers configuration.json fusionnés](http://ocelot.readthedocs.io/en/latest/features/configuration.html#merging-configuration-files) ou vous pouvez également stocker la [configuration dans un magasin Consul KV](http://ocelot.readthedocs.io/en/latest/features/configuration.html#store-configuration-in-consul). 

Toutefois, comme indiqué dans la section d’architecture et de conception, si vous voulez vraiment avoir des microservices autonomes, il peut être préférable de diviser cette passerelle d’API monolithique en plusieurs passerelles d’API et/ou BFF (backend for frontend). Pour ce faire, voyons comment implémenter cette approche avec des conteneurs Docker.

### <a name="using-a-single-docker-container-image-to-run-multiple-different-api-gateway--bff-container-types"></a>Utilisation d’une image de conteneur Docker individuelle pour exécuter différents types de conteneur de passerelles d’API/BFF 

La conception dans eShopOnContainers implémente une image de conteneur Docker individuelle avec la passerelle d’API Ocelot, puis, lors du déploiement vers Docker, elle crée des conteneurs/services différents pour chaque type de passerelle d’API/BFF en fournissant un fichier configuration.json différent pour chaque conteneur.

![](./media/image33.png)

**Figure 8-32.** Réutilisation d’une image Docker d’Ocelot unique sur plusieurs types de passerelle d’API

Dans eShopOnContainers, « l’image Docker de passerelle d’API Ocelot générique » est créée avec le projet nommé « OcelotApiGw » et le nom d’image « eshop/ocelotapigw » qui est spécifié dans le fichier docker-compose.yml. Ensuite, lors du déploiement sur Docker, quatre conteneurs de passerelle d’API sont créés à partir de cette même image Docker, comme indiqué dans l’extrait suivant du fichier docker-compose.yml.

```
PARTIAL DOCKER-COMPOSE.YML

  mobileshoppingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile
 
  mobilemarketingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile
 
  webshoppingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  webmarketingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile
```

De plus, comme vous pouvez le voir dans le fichier docker-compose.override.yml, la seule différence entre ces conteneurs de passerelle d’API est le fichier de configuration Ocelot, qui est différent pour chaque conteneur de service et spécifié lors de l’exécution via un volume Docker, comme indiqué dans le fichier docker-compose.override.yml suivant.

```
mobileshoppingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity.api              
  ports:
    - "5200:80"   
  volumes:
    - ./src/ApiGateways/Mobile.Bff.Shopping/apigw:/app/configuration
 
mobilemarketingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity.api              
  ports:
    - "5201:80"   
  volumes:
    - ./src/ApiGateways/Mobile.Bff.Marketing/apigw:/app/configuration

webshoppingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity.api              
  ports:
    - "5202:80"   
  volumes:
    - ./src/ApiGateways/Web.Bff.Shopping/apigw:/app/configuration

webmarketingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity.api              
  ports:
    - "5203:80"   
  volumes:
    - ./src/ApiGateways/Web.Bff.Marketing/apigw:/app/configuration
```

En raison de ce code précédent et comme l’indique l’Explorateur Visual Studio ci-dessous, le seul fichier nécessaire pour définir chaque passerelle d’API d’entreprise/BFF spécifique est un fichier configuration.json, étant donné que les quatre passerelles d’API reposent sur la même image Docker.

![](./media/image34.png)

**Figure 8-33.** Le seul fichier nécessaire pour définir chaque passerelle d’API / BFF avec Ocelot est un fichier de configuration 

En divisant la passerelle d’API en plusieurs passerelles d’API, différentes équipes de développement portant sur différents sous-ensembles de microservices peuvent gérer leurs propres passerelles d’API à l’aide de fichiers de configuration Ocelot indépendants. De plus, en même temps, elles peuvent réutiliser la même image Docker d’Ocelot. 

Maintenant, si vous exécutez eShopOnContainers avec les passerelles d’API (incluses par défaut dans Visual Studio lorsque vous ouvrez la solution eShopOnContainers-ServicesAndWebApps.sln ou si vous exécutez « docker-compose up »), les exemples d’itinéraires suivants sont effectués. 

Par exemple, lors de la visite de l’URL en amont http://localhost:5202/api/v1/c/catalog/items/2/ prise en charge par la passerelle d’API webshoppingapigw, vous obtenez le résultat à partir de l’URL interne en aval http://catalog.api/api/v1/2 dans l’hôte Docker, comme dans le navigateur suivant.

![](./media/image35.png)

**Figure 8-34.** Accès à un microservice via une URL fournie par la passerelle d’API 

En raisons des tests ou du débogage, si vous souhaitez accéder directement au conteneur Docker Catalog (uniquement dans l’environnement de développement) sans passer par la passerelle d’API, dans la mesure où « catalog.api » est une résolution DNS interne à l’hôte Docker (découverte de service gérée par les noms de service docker-compose), la seule façon d’accéder directement au conteneur est via le port externe publié dans le fichier docker-compose.override.yml, qui est fourni uniquement pour les tests de développement, comme http://localhost:5101/api/v1/Catalog/items/1 dans le navigateur suivant.

![](./media/image36.png)

**Figure 8-35.** Accès direct à un microservice à des fins de test 

Toutefois, l’application est configurée pour pouvoir accéder à tous les microservices via les passerelles d’API et non pas via les « raccourcis » des ports directs. 

### <a name="the-gateway-aggregation-pattern-in-eshoponcontainers"></a>Modèle d’agrégation de passerelle dans eShopOnContainers

Comme expliqué précédemment, une manière souple d’implémenter l’agrégation des demandes est d’utiliser les services personnalisés, par code. Vous pouvez également implémenter l’agrégation des demandes avec la fonctionnalité d’agrégation des demandes dans Ocelot, mais elle ne sera peut-être pas aussi flexible qu’il le faut. Par conséquent, la manière sélectionnée pour implémenter l’agrégation dans eShopOnContainers consiste à utiliser un service API web ASP.NET Core explicite pour chaque agrégateur. 

Selon cette approche, le diagramme de composition de passerelle d’API est en réalité un peu plus étendu lorsque sont pris en compte les services d’agrégation qui ne figurent pas dans le diagramme d’architecture globale simplifiée illustrée précédemment. 

Dans le diagramme suivant, vous pouvez également voir les services d’agrégation fonctionner avec leurs passerelles d’API associées.

![](./media/image37.png)

**Figure 8-36.** Architecture eShopOnContainers avec services d’agrégation

L’image suivante offre un plus grand zoom avant et vous pouvez remarquer comment, pour le secteur d’activité « Shopping », les applications clientes peuvent être améliorées en réduisant les échanges avec les microservices lors de l’utilisation de ces services d’agrégation sous le domaine des passerelles d’API. 

 ![](./media/image38.png)

**Figure 8-37.** Vision agrandie des services d’agrégation

Vous pouvez remarquer que le diagramme peut se compliquer lorsqu’il montre les demandes possibles provenant des passerelles d’API. Vous pouvez voir comment les flèches en bleu seraient simplifiées, du point de vue des applications clientes, lors de l’utilisation du modèle d’agrégation en réduisant les échanges et la latence de la communication, et en améliorant finalement considérablement l’expérience utilisateur pour les applications distantes (applications mobiles et SPA), en particulier. 

Dans le cas du secteur d’activité « Marketing » et des microservices, il s’agit d’un cas d’usage très simple, et il n’était pas nécessaire d’utiliser des agrégateurs, bien que cela soit possible, le cas échéant.

### <a name="authentication-and-authorization-in-ocelot-api-gateways"></a>Authentification et autorisation dans les passerelles d’API Ocelot

Dans une passerelle d’API Ocelot, vous pouvez placer le service d’authentification, tel qu’un service API web ASP.NET Core avec [IdentityServer](http://identityserver.io/) pour fournir le jeton d’authentification, à l’extérieur ou à l’intérieur de la passerelle d’API.

Comme eShopOnContainers utilise plusieurs passerelles d’API avec des limites basées sur des secteurs d’activité et BFF, le service d’identité ou d’authentification est tenu à l’écart des passerelles d’API, tel qu’indiqué en jaune dans le diagramme suivant.

 ![](./media/image39.png)

**Figure 8-38.** Position du service d’identité dans eShopOnContainers

Toutefois, Ocelot prend également en charge le placement du microservice d’identité ou d’authentification dans les limites de la passerelle d’API, comme dans cet autre diagramme.

 ![](./media/image40.png)

**Figure 8-39.** Authentification dans la passerelle d’API Ocelot

Étant donné que l’application eShopOnContainers a divisé la passerelle d’API en plusieurs passerelles d’API BFF (Backend for Frontend) et de secteurs d’activité, une autre option aurait été de créer une passerelle d’API supplémentaire pour les problèmes transversaux. Ce choix serait juste dans une architecture basée sur des microservices plus complexes avec plusieurs microservices de problèmes transversaux. Comme un seul problème transversal figure dans eShopOnContainers, il a été décidé de gérer simplement le service de sécurité hors du domaine des passerelles d’API, par souci de simplicité.

Dans tous les cas, si l’application est sécurisée au niveau des passerelles d’API, le module d’authentification de la passerelle d’API Ocelot est consulté dans un premier temps lorsque vous tentez d’utiliser un microservice sécurisé quelconque. Cela redirige la requête HTTP pour consulter le microservice d’identité ou d’authentification afin d’obtenir le jeton d’accès permettant de consulter les services protégés avec le jeton d’accès.

La manière de sécuriser par authentification n’importe quel service au niveau de la passerelle d’API consiste à définir l’élément AuthenticationProviderKey dans ses paramètres associés, dans le fichier configuration.json.

```
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket.api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
    }
```

Quand Ocelot s’exécute, il examine les réacheminements AuthenticationOptions.AuthenticationProviderKey et vérifie qu’il existe un fournisseur d’authentification enregistré avec la clé donnée. S’il n’en existe pas, Ocelot ne démarre pas. S’il en existe, le réacheminement utilise ce fournisseur lorsqu’il s’exécute.

Étant donné que la méthode WebHost d’Ocelot est configurée avec `authenticationProviderKey = "IdentityApiKey"`, elle nécessite une authentification chaque fois que ce service a des demandes quelconques sans jeton d’authentification. 

```csharp
namespace OcelotApiGw
{
    public class Startup
    {
        private readonly IConfiguration _cfg;

        public Startup(IConfiguration configuration) => _cfg = configuration;

        public void ConfigureServices(IServiceCollection services)
        {
            var identityUrl = _cfg.GetValue<string>("IdentityUrl");
            var authenticationProviderKey = "IdentityApiKey";
                         //…
            services.AddAuthentication()
                .AddJwtBearer(authenticationProviderKey, x =>
                {
                    x.Authority = identityUrl;
                    x.RequireHttpsMetadata = false;
                    x.TokenValidationParameters = new Microsoft.IdentityModel.Tokens.TokenValidationParameters()
                    {
                        ValidAudiences = new[] { "orders", "basket", "locations", "marketing", "mobileshoppingagg", "webshoppingagg" }
                    };                   
                });
            //...
```

Ensuite, vous devez également définir une autorisation avec l’attribut [Authorize] sur une ressource quelconque accessible comme les microservices, comme dans le contrôleur de microservice de panier d’achat (Basket) suivant.

```csharp
namespace Microsoft.eShopOnContainers.Services.Basket.API.Controllers
{
    [Route("api/v1/[controller]")]
    [Authorize]
    public class BasketController : Controller
    {   
      //...
    }
}
```

Les éléments ValidAudiences tels que « basket » sont mis en corrélation avec l’audience définie dans chaque microservice avec `AddJwtBearer()` dans la fonction ConfigureServices() de la classe Startup, comme dans le code ci-dessous.

```csharp
// prevent from mapping "sub" claim to nameidentifier.
JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();

var identityUrl = Configuration.GetValue<string>("IdentityUrl"); 
                
services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;

}).AddJwtBearer(options =>
{
    options.Authority = identityUrl;
    options.RequireHttpsMetadata = false;
    options.Audience = "basket";
});
```

Dans l’étape suivante, si vous essayez d’accéder à un microservice sécurisé quelconque comme le microservice Basket avec une URL de réacheminement basée sur la passerelle d’API comme http://localhost:5202/api/v1/b/basket/1, vous obtenez une erreur 401 Non autorisé, sauf si vous fournissez un jeton valide. En revanche, si une URL de réacheminement est authentifiée, Ocelot appelle tout schéma en aval qui lui est associé (URL de microservice interne).

**Autorisation au niveau des réacheminements d’Ocelot.**  Ocelot prend en charge l’autorisation basée sur les revendications évaluée après l’authentification. Vous définissez l’autorisation à un niveau d’acheminement en ajoutant le code suivant à la configuration de réacheminement. 

```
"RouteClaimsRequirement": {
    "UserType": "employee"
}
```

Dans cet exemple, quand l’intergiciel d’autorisation est appelé, Ocelot détermine si l’utilisateur a le type de revendication « UserType » dans le jeton, et si la valeur de cette revendication est « employee ». Si ce n’est pas le cas, l’utilisateur n’est pas autorisé et la réponse est 403 (Refusé). 

## <a name="using-kubernetes-ingress-plus-ocelot-api-gateways"></a>Utilisation de l’entrée Kubernetes et des passerelles d’API Ocelot

Lorsque vous utilisez Kubernetes (comme dans un cluster Azure Kubernetes Service), vous unifiez généralement toutes les requêtes HTTP via le [niveau d’entrée Kubernetes](https://kubernetes.io/docs/concepts/services-networking/ingress/) basé sur *Nginx*. 

Dans Kubernetes, si vous n’utilisez aucune approche d’entrée, vos services et pods ont des adresses IP routables uniquement par le réseau en cluster. 

Toutefois, si vous utilisez une approche d’entrée, vous disposez d’un niveau intermédiaire entre Internet et vos services (y compris vos passerelles d’API), agissant comme un proxy inverse.

Par définition, une entrée est une collection de règles qui autorisent les connexions entrantes à atteindre les services de cluster. Une entrée est configurée pour fournir aux services des URL accessibles en externe, un trafic d’équilibrage de charge, un processus de terminaison SSL, etc. Les utilisateurs demandent une entrée en publiant la ressource d’entrée sur le serveur d’API.

Dans eShopOnContainers, lorsque vous effectuez un développement local et utilisez simplement votre machine de développement en tant qu’hôte Docker, vous n’utilisez pas d’entrée, mais uniquement les multiples passerelles d’API. 

Toutefois, lorsque vous ciblez un environnement de « production » basé sur Kuberentes, eShopOnCOntainers utilise une entrée en face des passerelles d’API. De cette façon, les clients appellent toujours la même URL de base, mais les demandes sont acheminées vers plusieurs passerelles d’API ou BFF. 

Notez que les passerelles d’API sont des serveurs frontaux ou des façades exposant uniquement les services, mais pas les applications web qui sont généralement hors de leur portée. De plus, les passerelles d’API peuvent masquer certains microservices internes. 

Toutefois, l’entrée redirige simplement les requêtes HTTP et n’essaie pas de masquer de microservice ni d’application web.

Le fait d’avoir un niveau Nginx d’entrée dans Kubernetes en face des applications web, ainsi que plusieurs passerelles d’API/BFF Ocelot, constitue l’architecture idéale, telle qu’elle est illustrée dans le diagramme suivant.

 ![](./media/image41.png)

**Figure 8-40.** Niveau d’entrée dans eShopOnContainers lors d’un déploiement dans Kubernetes

Quand vous déployez eShopOnContainers dans Kubernetes, il expose simplement quelques services ou points de terminaison via l’_entrée_, en fait la liste suivante de suffixes sur les URL :

-   `/` pour l’application web SPA cliente
-   `/webmvc` pour l’application web MVC cliente
-   `/webstatus` pour l’application web cliente affichant l’état/les vérifications d'intégrité
-   `/webshoppingapigw` pour les processus métier d’achat et BFF web
-   `/webmarketingapigw` pour les processus métier marketing et BFF web
-   `/mobileshoppingapigw` pour les processus métier d’achat et BFF mobiles
-   `/mobilemarketingapigw` pour les processus métier marketing et BFF mobiles

Lorsque vous déployez dans Kubernetes, chaque passerelle d’API Ocelot utilise un fichier « configuration.json » différent pour chaque _pod_ exécutant les passerelles d’API. Ces fichiers « configuration.json » sont fournis en montant (initialement avec le script deploy.ps1) un volume créé sur la base d’une _carte de configuration_ Kubernetes et nommé « ocelot ». Chaque conteneur monte le fichier de configuration qui lui est associé dans le dossier du conteneur nommé `/app/configuration`.

Dans les fichiers de code source d’eShopOnContainers, les fichiers « configuration.json » d’origine sont disponibles dans le dossier `k8s/ocelot/`. Il existe un fichier pour chaque passerelle d’API/BFF.


## <a name="additional-cross-cutting-features-in-an-ocelot-api-gateway"></a>Fonctionnalités transversales supplémentaires dans une passerelle d’API Ocelot

D’autres fonctionnalités importantes peuvent être étudiées et utilisées lorsque vous utilisez une passerelle d’API Ocelot. Elles sont décrites dans les liens suivants.

-   **Découverte de services côté client intégrant Ocelot avec Consul ou Eureka** 
    [*http://ocelot.readthedocs.io/en/latest/features/servicediscovery.html*](http://ocelot.readthedocs.io/en/latest/features/servicediscovery.html)

-   **Mise en cache au niveau de la passerelle d’API** 
    [*http://ocelot.readthedocs.io/en/latest/features/caching.html*](http://ocelot.readthedocs.io/en/latest/features/caching.html)

-   **Connexion au niveau de la passerelle d’API** 
    [*http://ocelot.readthedocs.io/en/latest/features/logging.html*](http://ocelot.readthedocs.io/en/latest/features/logging.html)

-   **Qualité de service (nouvelles tentatives et disjoncteurs) au niveau de la passerelle d’API** 
    [*http://ocelot.readthedocs.io/en/latest/features/qualityofservice.html*](http://ocelot.readthedocs.io/en/latest/features/qualityofservice.html)

-   **Limitation du débit** 
    [*http://ocelot.readthedocs.io/en/latest/features/ratelimiting.html*](http://ocelot.readthedocs.io/en/latest/features/ratelimiting.html )




>[!div class="step-by-step"]
[Précédent] (background-tasks-with-ihostedservice.md) [Suivant] (../microservice-ddd-cqrs-patterns/index.md)