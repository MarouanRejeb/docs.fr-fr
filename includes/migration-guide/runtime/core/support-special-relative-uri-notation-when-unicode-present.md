### <a name="support-special-relative-uri-notation-when-unicode-is-present"></a><span data-ttu-id="10d91-101">Prise en charge d’une notation d’URI relatif spéciale quand des caractères Unicode sont présents</span><span class="sxs-lookup"><span data-stu-id="10d91-101">Support special relative URI notation when Unicode is present</span></span>

|   |   |
|---|---|
|<span data-ttu-id="10d91-102">Détails</span><span class="sxs-lookup"><span data-stu-id="10d91-102">Details</span></span>|<span data-ttu-id="10d91-103"><xref:System.Uri> ne lève plus une <xref:System.NullReferenceException> quand il appelle <xref:System.Uri.TryCreate%2A> sur certains URI relatifs contenant des caractères Unicode. La reproduction la plus simple du <xref:System.NullReferenceException> est indiquée ci-dessous, où les deux instructions sont équivalentes :</span><span class="sxs-lookup"><span data-stu-id="10d91-103"><xref:System.Uri> will no longer throw a <xref:System.NullReferenceException> when calling <xref:System.Uri.TryCreate%2A> on certain relative URIs containing Unicode.The simplest reproduction of the <xref:System.NullReferenceException> is below, with the two statements being equivalent:</span></span><pre><code class="lang-csharp">bool success = Uri.TryCreate(&quot;http:%C3%A8&quot;, UriKind.RelativeOrAbsolute, out Uri href);&#13;&#10;bool success = Uri.TryCreate(&quot;http:&#232;&quot;, UriKind.RelativeOrAbsolute, out Uri href);&#13;&#10;</code></pre><span data-ttu-id="10d91-104"><xref:System.NullReferenceException> ne peut être reproduite que si les conditions suivantes sont réunies :</span><span class="sxs-lookup"><span data-stu-id="10d91-104">To reproduce the <xref:System.NullReferenceException>, the following items must be true:</span></span><ul><li><span data-ttu-id="10d91-105">Vous devez spécifier l’URI comme étant relatif en le faisant précéder de « http: » sans le faire suivre de « // ».</span><span class="sxs-lookup"><span data-stu-id="10d91-105">The URI must be specified as relative by prepending it with ‘http:’ and not following it with ‘//’.</span></span></li><li><span data-ttu-id="10d91-106">L’URI doit contenir des symboles non réservés ou Unicode encodés en pourcentage.</span><span class="sxs-lookup"><span data-stu-id="10d91-106">The URI must contain percent-encoded Unicode or unreserved symbols.</span></span></li></ul>|
|<span data-ttu-id="10d91-107">Suggestion</span><span class="sxs-lookup"><span data-stu-id="10d91-107">Suggestion</span></span>|<span data-ttu-id="10d91-108">Les utilisateurs qui dépendent de ce comportement pour interdire les URI relatifs doivent spécifier <xref:System.UriKind.Absolute?displayProperty=nameWithType> à la place quand ils créent un URI.</span><span class="sxs-lookup"><span data-stu-id="10d91-108">Users depending on this behavior to disallow relative URIs should instead specify <xref:System.UriKind.Absolute?displayProperty=nameWithType> when creating a URI.</span></span>|
|<span data-ttu-id="10d91-109">Portée</span><span class="sxs-lookup"><span data-stu-id="10d91-109">Scope</span></span>|<span data-ttu-id="10d91-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="10d91-110">Edge</span></span>|
|<span data-ttu-id="10d91-111">Version</span><span class="sxs-lookup"><span data-stu-id="10d91-111">Version</span></span>|<span data-ttu-id="10d91-112">4.7.2</span><span class="sxs-lookup"><span data-stu-id="10d91-112">4.7.2</span></span>|
|<span data-ttu-id="10d91-113">Type</span><span class="sxs-lookup"><span data-stu-id="10d91-113">Type</span></span>|<span data-ttu-id="10d91-114">Runtime</span><span class="sxs-lookup"><span data-stu-id="10d91-114">Runtime</span></span>|
|<span data-ttu-id="10d91-115">API affectées</span><span class="sxs-lookup"><span data-stu-id="10d91-115">Affected APIs</span></span>|<ul><li><xref:System.Uri.TryCreate(System.Uri,System.Uri,System.Uri@)?displayProperty=nameWithType></li><li><xref:System.Uri.TryCreate(System.String,System.UriKind,System.Uri@)?displayProperty=nameWithType></li><li><xref:System.Uri.TryCreate(System.Uri,System.String,System.Uri@)?displayProperty=nameWithType></li></ul>|
