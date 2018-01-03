---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (VB) | Documenti Microsoft
author: microsoft
description: Informazioni su come combinare il contenuto dinamico e memorizzati nella cache nella stessa pagina. Sostituzione post-cache consente di visualizzare il contenuto dinamico, ad esempio intestazione o di annunci...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f07f4ecec36e71679dbc471b65f26d260349a07e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="7d80c-104">Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (VB)</span><span class="sxs-lookup"><span data-stu-id="7d80c-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="7d80c-105">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7d80c-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7d80c-106">Informazioni su come combinare il contenuto dinamico e memorizzati nella cache nella stessa pagina.</span><span class="sxs-lookup"><span data-stu-id="7d80c-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="7d80c-107">Sostituzione post-cache consente di visualizzare il contenuto dinamico, ad esempio pubblicitari o notizie, all'interno di una pagina che è stato memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="7d80c-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="7d80c-108">Sfruttando la cache di output, è possibile migliorare notevolmente le prestazioni di un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7d80c-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="7d80c-109">Anziché la rigenerazione di una pagina ogni volta che viene richiesta la pagina, la pagina può essere generata una volta e memorizzata nella cache per più utenti.</span><span class="sxs-lookup"><span data-stu-id="7d80c-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="7d80c-110">Ma si è verificato un problema.</span><span class="sxs-lookup"><span data-stu-id="7d80c-110">But there is a problem.</span></span> <span data-ttu-id="7d80c-111">Cosa accade se si desidera visualizzare il contenuto dinamico nella pagina?</span><span class="sxs-lookup"><span data-stu-id="7d80c-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="7d80c-112">Si supponga, ad esempio, che si desidera visualizzare un banner pubblicitario nella pagina.</span><span class="sxs-lookup"><span data-stu-id="7d80c-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="7d80c-113">Non si desidera banner pubblicitario da memorizzare nella cache in modo che ogni utente vede l'annuncio stesso.</span><span class="sxs-lookup"><span data-stu-id="7d80c-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="7d80c-114">Si risulterebbero qualsiasi money in questo modo.</span><span class="sxs-lookup"><span data-stu-id="7d80c-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="7d80c-115">Fortunatamente, è una soluzione semplice.</span><span class="sxs-lookup"><span data-stu-id="7d80c-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="7d80c-116">È possibile usufruire di una funzionalità del framework di ASP.NET denominato *sostituzione post-cache*.</span><span class="sxs-lookup"><span data-stu-id="7d80c-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="7d80c-117">Sostituzione post-cache consente di sostituire il contenuto dinamico in una pagina in cui è stato memorizzato nella cache in memoria.</span><span class="sxs-lookup"><span data-stu-id="7d80c-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="7d80c-118">In genere, quando si esegue l'output nella cache una pagina utilizzando la &lt;OutputCache&gt; attributo, la pagina viene memorizzato nella cache sul server e client (browser web).</span><span class="sxs-lookup"><span data-stu-id="7d80c-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="7d80c-119">Quando si utilizza la sostituzione post-cache, una pagina nella cache solo sul server.</span><span class="sxs-lookup"><span data-stu-id="7d80c-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="7d80c-120">Utilizzando la sostituzione post-Cache</span><span class="sxs-lookup"><span data-stu-id="7d80c-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="7d80c-121">L'utilizzo di sostituzione post-cache richiede due passaggi.</span><span class="sxs-lookup"><span data-stu-id="7d80c-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="7d80c-122">In primo luogo, è necessario definire un metodo che restituisce una stringa che rappresenta il contenuto dinamico che si desidera visualizzare nella pagina memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="7d80c-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="7d80c-123">Successivamente, chiamare il metodo HttpResponse.WriteSubstitution() per inserire il contenuto dinamico nella pagina.</span><span class="sxs-lookup"><span data-stu-id="7d80c-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="7d80c-124">Si supponga, ad esempio, che si desidera visualizzare in modo casuale notizie diversi in una pagina memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="7d80c-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="7d80c-125">La classe nel listato 1 espone un singolo metodo, denominato RenderNews(), che restituisce in modo casuale una notizia da un elenco di tre elementi di notizie.</span><span class="sxs-lookup"><span data-stu-id="7d80c-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="7d80c-126">**Elenco 1 – Models\News.vb**</span><span class="sxs-lookup"><span data-stu-id="7d80c-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="7d80c-127">Per sfruttare i vantaggi di sostituzione post-cache, si chiama il metodo HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="7d80c-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="7d80c-128">Il metodo WriteSubstitution() imposta il codice per sostituire un'area della pagina memorizzata nella cache con contenuto dinamico.</span><span class="sxs-lookup"><span data-stu-id="7d80c-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="7d80c-129">Il metodo WriteSubstitution() viene utilizzato per visualizzare l'elemento notizie casuali nella visualizzazione elenco 2.</span><span class="sxs-lookup"><span data-stu-id="7d80c-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="7d80c-130">**Elenco di 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="7d80c-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="7d80c-131">Il metodo RenderNews viene passato al metodo WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="7d80c-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="7d80c-132">Si noti che non viene chiamato il metodo RenderNews.</span><span class="sxs-lookup"><span data-stu-id="7d80c-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="7d80c-133">Invece viene passato un riferimento al metodo a WriteSubstitution() con l'aiuto dell'operatore AddressOf.</span><span class="sxs-lookup"><span data-stu-id="7d80c-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="7d80c-134">La visualizzazione dell'indice viene memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="7d80c-134">The Index view is cached.</span></span> <span data-ttu-id="7d80c-135">La vista viene restituita dal controller nel listato 3.</span><span class="sxs-lookup"><span data-stu-id="7d80c-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="7d80c-136">Si noti che l'azione Index () è decorato con un &lt;OutputCache&gt; attributo che causa la visualizzazione dell'indice da memorizzare nella cache per 60 secondi.</span><span class="sxs-lookup"><span data-stu-id="7d80c-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="7d80c-137">**Elenco di 3: Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="7d80c-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="7d80c-138">Anche se la visualizzazione dell'indice viene memorizzato nella cache, notizie casuali diversi vengono visualizzate quando si richiede la pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="7d80c-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="7d80c-139">Quando si richiede la pagina di indice, l'ora visualizzata nella pagina rimane invariato per 60 secondi (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="7d80c-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="7d80c-140">Il fatto che il cambiamento di ora non significa che la pagina nella cache.</span><span class="sxs-lookup"><span data-stu-id="7d80c-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="7d80c-141">Tuttavia, il contenuto inserito le modifiche di metodo – casuale notizia – WriteSubstitution() con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="7d80c-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="7d80c-142">**Figura 1: inserimento di notizie dinamica in una pagina memorizzata nella cache**</span><span class="sxs-lookup"><span data-stu-id="7d80c-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="7d80c-144">Utilizzo di sostituzione post-Cache in metodi di supporto</span><span class="sxs-lookup"><span data-stu-id="7d80c-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="7d80c-145">Un modo più semplice per sfruttare la sostituzione post-cache è per incapsulare la chiamata al metodo WriteSubstitution() all'interno di un metodo helper personalizzati.</span><span class="sxs-lookup"><span data-stu-id="7d80c-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="7d80c-146">Questo approccio è illustrato il metodo helper listato 4.</span><span class="sxs-lookup"><span data-stu-id="7d80c-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="7d80c-147">**Listato 4 – Helpers\AdHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="7d80c-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="7d80c-148">Listato 4 contiene un modulo di Visual Basic che espone due metodi: RenderBanner() e RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="7d80c-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="7d80c-149">Il metodo RenderBanner() rappresenta il metodo di supporto effettivo.</span><span class="sxs-lookup"><span data-stu-id="7d80c-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="7d80c-150">Questo metodo estende la classe HtmlHelper MVC ASP.NET standard, in modo che sia possibile chiamare Html.RenderBanner() in una vista come qualsiasi altro metodo di supporto.</span><span class="sxs-lookup"><span data-stu-id="7d80c-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="7d80c-151">Il metodo RenderBanner() chiama il metodo HttpResponse.WriteSubstitution() passando il metodo RenderBannerInternal() al metodo WriteSubsitution().</span><span class="sxs-lookup"><span data-stu-id="7d80c-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="7d80c-152">Il metodo RenderBannerInternal() è un metodo privato.</span><span class="sxs-lookup"><span data-stu-id="7d80c-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="7d80c-153">Questo metodo non esposto come metodo di supporto.</span><span class="sxs-lookup"><span data-stu-id="7d80c-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="7d80c-154">Il metodo RenderBannerInternal() restituisce in modo casuale un'immagine di annuncio dell'intestazione da un elenco di immagini di annuncio tre banner.</span><span class="sxs-lookup"><span data-stu-id="7d80c-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="7d80c-155">La visualizzazione dell'indice modificata nel listato 5 viene illustrato come è possibile utilizzare il metodo di supporto RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="7d80c-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="7d80c-156">Si noti che un altro &lt;% @ Import %&gt; direttiva è inclusa nella parte superiore della vista da importare lo spazio dei nomi MvcApplication1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="7d80c-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="7d80c-157">Se non si importa questo spazio dei nomi, il metodo RenderBanner() non verrà visualizzato come un metodo per la proprietà Html.</span><span class="sxs-lookup"><span data-stu-id="7d80c-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="7d80c-158">**Elenco di 5-Views\Home\Index.aspx (con il metodo RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="7d80c-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="7d80c-159">Quando si richiede la pagina il rendering da parte della vista nel listato 5, con ogni richiesta viene visualizzato un annuncio di intestazione diverso (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="7d80c-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="7d80c-160">La pagina nella cache, ma il banner pubblicitario viene inserito in modo dinamico dal metodo di supporto RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="7d80c-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="7d80c-161">**Figura 2: la visualizzazione dell'indice la visualizzazione di un annuncio banner casuale**</span><span class="sxs-lookup"><span data-stu-id="7d80c-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="7d80c-163">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="7d80c-163">Summary</span></span>

<span data-ttu-id="7d80c-164">In questa esercitazione viene illustrato come è possibile aggiornare dinamicamente il contenuto in una pagina memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="7d80c-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="7d80c-165">È stato descritto come utilizzare il metodo HttpResponse.WriteSubstitution() per attivare il contenuto dinamico essere inseriti in una pagina memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="7d80c-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="7d80c-166">È stato inoltre descritto incapsulare la chiamata al metodo WriteSubstitution() all'interno di un metodo helper HTML.</span><span class="sxs-lookup"><span data-stu-id="7d80c-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="7d80c-167">Sfruttare i vantaggi della memorizzazione nella cache quando possibile, ciò può avere un impatto significativo sulle prestazioni delle applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="7d80c-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="7d80c-168">Come illustrato in questa esercitazione, è possibile sfruttare la memorizzazione nella cache anche quando è necessario visualizzare il contenuto dinamico delle pagine.</span><span class="sxs-lookup"><span data-stu-id="7d80c-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7d80c-169">[Precedente](improving-performance-with-output-caching-vb.md)
[Successivo](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7d80c-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>