---
title: Richieste di gestione con i controller in ASP.NET MVC di base
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.assetid: 9da9eb52-8583-4069-af91-155ba3529d7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: 5dc6c7dc70027bb79875f389d535119a2543b873
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a><span data-ttu-id="34556-103">Richieste di gestione con i controller in ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="34556-103">Handling requests with controllers in ASP.NET Core MVC</span></span>

<span data-ttu-id="34556-104">Da [Steve Smith](https://ardalis.com/) e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="34556-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="34556-105">I controller, azioni e risultati dell'azione sono una parte fondamentale della modalità con cui gli sviluppatori di compilare App scritte in ASP.NET MVC di base.</span><span class="sxs-lookup"><span data-stu-id="34556-105">Controllers, actions, and action results are a fundamental part of how developers build apps using ASP.NET Core MVC.</span></span>

## <a name="what-is-a-controller"></a><span data-ttu-id="34556-106">Che cos'è un Controller?</span><span class="sxs-lookup"><span data-stu-id="34556-106">What is a Controller?</span></span>

<span data-ttu-id="34556-107">Un controller viene utilizzato per definire e raggruppare un set di azioni.</span><span class="sxs-lookup"><span data-stu-id="34556-107">A controller is used to define and group a set of actions.</span></span> <span data-ttu-id="34556-108">Un'azione (o *metodo di azione*) è un metodo in un controller che gestisce le richieste.</span><span class="sxs-lookup"><span data-stu-id="34556-108">An action (or *action method*) is a method on a controller which handles requests.</span></span> <span data-ttu-id="34556-109">I controller di raggruppano operazioni simili.</span><span class="sxs-lookup"><span data-stu-id="34556-109">Controllers logically group similar actions together.</span></span> <span data-ttu-id="34556-110">Questa aggregazione di azioni consente a un set comune di regole, ad esempio il routing, la memorizzazione nella cache e l'autorizzazione, da applicare collettivamente.</span><span class="sxs-lookup"><span data-stu-id="34556-110">This aggregation of actions allows common sets of rules, such as routing, caching, and authorization, to be applied collectively.</span></span> <span data-ttu-id="34556-111">Le richieste vengono mappate alle azioni tramite [routing](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="34556-111">Requests are mapped to actions through [routing](xref:mvc/controllers/routing).</span></span>

<span data-ttu-id="34556-112">Per convenzione, le classi controller:</span><span class="sxs-lookup"><span data-stu-id="34556-112">By convention, controller classes:</span></span>
* <span data-ttu-id="34556-113">Si trovano nel livello di radice del progetto *controller* cartella</span><span class="sxs-lookup"><span data-stu-id="34556-113">Reside in the project's root-level *Controllers* folder</span></span>
* <span data-ttu-id="34556-114">Ereditare da`Microsoft.AspNetCore.Mvc.Controller`</span><span class="sxs-lookup"><span data-stu-id="34556-114">Inherit from `Microsoft.AspNetCore.Mvc.Controller`</span></span>

<span data-ttu-id="34556-115">Un controller è una classe istanziabile in cui almeno una delle seguenti condizioni è true:</span><span class="sxs-lookup"><span data-stu-id="34556-115">A controller is an instantiable class in which at least one of the following conditions is true:</span></span>
* <span data-ttu-id="34556-116">Il nome della classe viene aggiunto il suffisso "Controller"</span><span class="sxs-lookup"><span data-stu-id="34556-116">The class name is suffixed with "Controller"</span></span>
* <span data-ttu-id="34556-117">La classe eredita da una classe il cui nome viene aggiunto il suffisso "Controller"</span><span class="sxs-lookup"><span data-stu-id="34556-117">The class inherits from a class whose name is suffixed with "Controller"</span></span>
* <span data-ttu-id="34556-118">La classe decorata con il `[Controller]` attributo</span><span class="sxs-lookup"><span data-stu-id="34556-118">The class is decorated with the `[Controller]` attribute</span></span>

<span data-ttu-id="34556-119">Una classe controller non deve avere un oggetto associato `[NonController]` attributo.</span><span class="sxs-lookup"><span data-stu-id="34556-119">A controller class must not have an associated `[NonController]` attribute.</span></span>

<span data-ttu-id="34556-120">Controller devono seguire il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="34556-120">Controllers should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="34556-121">Esistono due approcci per l'implementazione di questo principio.</span><span class="sxs-lookup"><span data-stu-id="34556-121">There are a couple approaches to implementing this principle.</span></span> <span data-ttu-id="34556-122">Se più azioni del controller richiedono lo stesso servizio, è consigliabile utilizzare [inserimento costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per richiedere tali dipendenze.</span><span class="sxs-lookup"><span data-stu-id="34556-122">If multiple controller actions require the same service, consider using [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to request those dependencies.</span></span> <span data-ttu-id="34556-123">Se il servizio è necessario da un metodo di azione singola, è consigliabile utilizzare [attacco intrusivo nel codice di azione](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) per richiedere la dipendenza.</span><span class="sxs-lookup"><span data-stu-id="34556-123">If the service is needed by only a single action method, consider using [Action Injection](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) to request the dependency.</span></span>

<span data-ttu-id="34556-124">All'interno di **M**enti -**V**ISTA -**C**ontroller, un controller è responsabile per l'elaborazione della richiesta iniziale e la creazione di istanze del modello.</span><span class="sxs-lookup"><span data-stu-id="34556-124">Within the **M**odel-**V**iew-**C**ontroller pattern, a controller is responsible for the initial processing of the request and instantiation of the model.</span></span> <span data-ttu-id="34556-125">In genere, le decisioni aziendali devono essere eseguite all'interno del modello.</span><span class="sxs-lookup"><span data-stu-id="34556-125">Generally, business decisions should be performed within the model.</span></span>

<span data-ttu-id="34556-126">Il controller accetta i risultati dell'elaborazione del modello di (se presente) e restituisce la visualizzazione corretta e i dati di visualizzazione associata oppure il risultato della chiamata API.</span><span class="sxs-lookup"><span data-stu-id="34556-126">The controller takes the result of the model's processing (if any) and returns either the proper view and its associated view data or the result of the API call.</span></span> <span data-ttu-id="34556-127">Altre informazioni, vedere [Panoramica di MVC ASP.NET Core](xref:mvc/overview) e [Guida introduttiva a Visual Studio e ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="34556-127">Learn more at [Overview of ASP.NET Core MVC](xref:mvc/overview) and [Getting started with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="34556-128">Il controller è un *a livello di interfaccia utente* astrazione.</span><span class="sxs-lookup"><span data-stu-id="34556-128">The controller is a *UI-level* abstraction.</span></span> <span data-ttu-id="34556-129">Le responsabilità sono per verificare che i dati di richiesta siano validi e di scegliere quale visualizzazione (o risultato di un'API) deve essere restituito.</span><span class="sxs-lookup"><span data-stu-id="34556-129">Its responsibilities are to ensure request data is valid and to choose which view (or result for an API) should be returned.</span></span> <span data-ttu-id="34556-130">Nelle App corretto factoring, non direttamente include dati accesso o la regola business.</span><span class="sxs-lookup"><span data-stu-id="34556-130">In well-factored apps, it does not directly include data access or business logic.</span></span> <span data-ttu-id="34556-131">Al contrario, il controller di delega per servizi per la gestione di tali responsabilità.</span><span class="sxs-lookup"><span data-stu-id="34556-131">Instead, the controller delegates to services handling these responsibilities.</span></span>

## <a name="defining-actions"></a><span data-ttu-id="34556-132">Definizione di azioni</span><span class="sxs-lookup"><span data-stu-id="34556-132">Defining Actions</span></span>

<span data-ttu-id="34556-133">Metodi pubblici in un controller, ad eccezione di quelle decorata con il `[NonAction]` attributo, sono azioni.</span><span class="sxs-lookup"><span data-stu-id="34556-133">Public methods on a controller, except those decorated with the `[NonAction]` attribute, are actions.</span></span> <span data-ttu-id="34556-134">Parametri sulle azioni sono associati a dati della richiesta e vengono convalidati utilizzando [associazione del modello](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="34556-134">Parameters on actions are bound to request data and are validated using [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="34556-135">Convalida del modello si verifica per tutto il modello di associazione.</span><span class="sxs-lookup"><span data-stu-id="34556-135">Model validation occurs for everything that's model-bound.</span></span> <span data-ttu-id="34556-136">Il `ModelState.IsValid` valore della proprietà indica se l'associazione di modelli e la convalida ha avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="34556-136">The `ModelState.IsValid` property value indicates whether model binding and validation succeeded.</span></span>

<span data-ttu-id="34556-137">Metodi di azione devono contenere la logica per il mapping di una richiesta di un problema di business.</span><span class="sxs-lookup"><span data-stu-id="34556-137">Action methods should contain logic for mapping a request to a business concern.</span></span> <span data-ttu-id="34556-138">Problemi aziendali devono essere rappresentate in genere come servizi che consente di accedere mediante il controller [inserimento di dipendenze](xref:mvc/controllers/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="34556-138">Business concerns should typically be represented as services that the controller accesses through [dependency injection](xref:mvc/controllers/dependency-injection).</span></span> <span data-ttu-id="34556-139">Azioni mappare il risultato dell'azione di business a uno stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34556-139">Actions then map the result of the business action to an application state.</span></span>

<span data-ttu-id="34556-140">Azioni possono restituire alcun valore, ma spesso restituire un'istanza di `IActionResult` (o `Task<IActionResult>` per i metodi asincroni) che produce una risposta.</span><span class="sxs-lookup"><span data-stu-id="34556-140">Actions can return anything, but frequently return an instance of `IActionResult` (or `Task<IActionResult>` for async methods) that produces a response.</span></span> <span data-ttu-id="34556-141">Il metodo di azione è responsabile della scelta *il tipo di risposta*.</span><span class="sxs-lookup"><span data-stu-id="34556-141">The action method is responsible for choosing *what kind of response*.</span></span> <span data-ttu-id="34556-142">Il risultato dell'azione *la risposta*.</span><span class="sxs-lookup"><span data-stu-id="34556-142">The action result *does the responding*.</span></span>

### <a name="controller-helper-methods"></a><span data-ttu-id="34556-143">Metodi di supporto del controller</span><span class="sxs-lookup"><span data-stu-id="34556-143">Controller Helper Methods</span></span>

<span data-ttu-id="34556-144">Controller è in genere ereditano da [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), anche se non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="34556-144">Controllers usually inherit from [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), although this is not required.</span></span> <span data-ttu-id="34556-145">Derivazione da `Controller` fornisce l'accesso a tre categorie di metodi di supporto:</span><span class="sxs-lookup"><span data-stu-id="34556-145">Deriving from `Controller` provides access to three categories of helper methods:</span></span>

#### <a name="1-methods-resulting-in-an-empty-response-body"></a><span data-ttu-id="34556-146">1. Metodi, creando un corpo della risposta vuota</span><span class="sxs-lookup"><span data-stu-id="34556-146">1. Methods resulting in an empty response body</span></span>

<span data-ttu-id="34556-147">Non `Content-Type` intestazione della risposta HTTP è incluso, poiché non dispone del contenuto per descrivere il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="34556-147">No `Content-Type` HTTP response header is included, since the response body lacks content to describe.</span></span>

<span data-ttu-id="34556-148">Esistono due tipi di risultati all'interno di questa categoria: reindirizzamento e codice di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="34556-148">There are two result types within this category: Redirect and HTTP Status Code.</span></span>

* <span data-ttu-id="34556-149">**Codice di stato HTTP**</span><span class="sxs-lookup"><span data-stu-id="34556-149">**HTTP Status Code**</span></span>

    <span data-ttu-id="34556-150">Questo tipo viene restituito un codice di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="34556-150">This type returns an HTTP status code.</span></span> <span data-ttu-id="34556-151">Alcuni metodi di supporto di questo tipo sono `BadRequest`, `NotFound`, e `Ok`.</span><span class="sxs-lookup"><span data-stu-id="34556-151">A couple helper methods of this type are `BadRequest`, `NotFound`, and `Ok`.</span></span> <span data-ttu-id="34556-152">Ad esempio, `return BadRequest();` produce un codice di 400 stato quando eseguita.</span><span class="sxs-lookup"><span data-stu-id="34556-152">For example, `return BadRequest();` produces a 400 status code when executed.</span></span> <span data-ttu-id="34556-153">Quando i metodi come `BadRequest`, `NotFound`, e `Ok` vengono sottoposti a overload, non è più considerata i risponditori codice di stato HTTP, poiché si sta eseguendo la negoziazione del contenuto.</span><span class="sxs-lookup"><span data-stu-id="34556-153">When methods such as `BadRequest`, `NotFound`, and `Ok` are overloaded, they no longer qualify as HTTP Status Code responders, since content negotiation is taking place.</span></span>

* <span data-ttu-id="34556-154">**Reindirizzamento**</span><span class="sxs-lookup"><span data-stu-id="34556-154">**Redirect**</span></span>

    <span data-ttu-id="34556-155">Questo tipo restituisce un reindirizzamento a un'azione o una destinazione (utilizzando `Redirect`, `LocalRedirect`, `RedirectToAction`, o `RedirectToRoute`).</span><span class="sxs-lookup"><span data-stu-id="34556-155">This type returns a redirect to an action or destination (using `Redirect`, `LocalRedirect`, `RedirectToAction`, or `RedirectToRoute`).</span></span> <span data-ttu-id="34556-156">Ad esempio, `return RedirectToAction("Complete", new {id = 123});` reindirizza a `Complete`, passando un oggetto anonimo.</span><span class="sxs-lookup"><span data-stu-id="34556-156">For example, `return RedirectToAction("Complete", new {id = 123});` redirects to `Complete`, passing an anonymous object.</span></span>

    <span data-ttu-id="34556-157">Il tipo di risultato di reindirizzamento è diverso dal tipo di codice di stato HTTP principalmente per l'aggiunta di un `Location` intestazione risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="34556-157">The Redirect result type differs from the HTTP Status Code type primarily in the addition of a `Location` HTTP response header.</span></span>

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a><span data-ttu-id="34556-158">2. Metodi, creando un corpo della risposta non vuoto con un tipo di contenuto predefinito.</span><span class="sxs-lookup"><span data-stu-id="34556-158">2. Methods resulting in a non-empty response body with a predefined content type</span></span>

<span data-ttu-id="34556-159">La maggior parte dei metodi di supporto di questa categoria includono un `ContentType` proprietà, che consente di impostare il `Content-Type` intestazione della risposta per descrivere il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="34556-159">Most helper methods in this category include a `ContentType` property, allowing you to set the `Content-Type` response header to describe the response body.</span></span>

<span data-ttu-id="34556-160">Esistono due tipi di risultati all'interno di questa categoria: [vista](xref:mvc/views/overview) e [risposta formattato](xref:mvc/models/formatting).</span><span class="sxs-lookup"><span data-stu-id="34556-160">There are two result types within this category: [View](xref:mvc/views/overview) and [Formatted Response](xref:mvc/models/formatting).</span></span>

* <span data-ttu-id="34556-161">**Visualizza**</span><span class="sxs-lookup"><span data-stu-id="34556-161">**View**</span></span>

    <span data-ttu-id="34556-162">Questo tipo restituisce una vista che utilizza un modello per il rendering HTML.</span><span class="sxs-lookup"><span data-stu-id="34556-162">This type returns a view which uses a model to render HTML.</span></span> <span data-ttu-id="34556-163">Ad esempio, `return View(customer);` passa un modello per la visualizzazione per l'associazione dati.</span><span class="sxs-lookup"><span data-stu-id="34556-163">For example, `return View(customer);` passes a model to the view for data-binding.</span></span>

* <span data-ttu-id="34556-164">**Risposta formattato**</span><span class="sxs-lookup"><span data-stu-id="34556-164">**Formatted Response**</span></span>

    <span data-ttu-id="34556-165">Questo tipo restituisce JSON o un formato di scambio di dati simili per rappresentare un oggetto in modo specifico.</span><span class="sxs-lookup"><span data-stu-id="34556-165">This type returns JSON or a similar data exchange format to represent an object in a specific manner.</span></span> <span data-ttu-id="34556-166">Ad esempio, `return Json(customer);` serializza l'oggetto specificato in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="34556-166">For example, `return Json(customer);` serializes the provided object into JSON format.</span></span>
    
    <span data-ttu-id="34556-167">Altri metodi più comuni di questo tipo sono `File`, `PhysicalFile`, e `VirtualFile`.</span><span class="sxs-lookup"><span data-stu-id="34556-167">Other common methods of this type include `File`, `PhysicalFile`, and `VirtualFile`.</span></span> <span data-ttu-id="34556-168">Ad esempio, `return PhysicalFile(customerFilePath, "text/xml");` restituisce un file XML descritto da un `Content-Type` valore dell'intestazione di risposta "text/XML".</span><span class="sxs-lookup"><span data-stu-id="34556-168">For example, `return PhysicalFile(customerFilePath, "text/xml");` returns an XML file described by a `Content-Type` response header value of "text/xml".</span></span>

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a><span data-ttu-id="34556-169">3. Metodi, creando un corpo della risposta non vuoto formattato in un tipo di contenuto negoziato con il client</span><span class="sxs-lookup"><span data-stu-id="34556-169">3. Methods resulting in a non-empty response body formatted in a content type negotiated with the client</span></span>

<span data-ttu-id="34556-170">Questa categoria è noto come **negoziazione del contenuto**.</span><span class="sxs-lookup"><span data-stu-id="34556-170">This category is better known as **Content Negotiation**.</span></span> <span data-ttu-id="34556-171">[Negoziazione di contenuto](xref:mvc/models/formatting#content-negotiation) applica ogni volta che un'azione restituisce un [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) tipo o un valore diverso da un [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementazione.</span><span class="sxs-lookup"><span data-stu-id="34556-171">[Content negotiation](xref:mvc/models/formatting#content-negotiation) applies whenever an action returns an [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) type or something other than an [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementation.</span></span> <span data-ttu-id="34556-172">Un'azione che restituisce un non -`IActionResult` implementazione (ad esempio, `object`) restituisce inoltre una risposta formattato.</span><span class="sxs-lookup"><span data-stu-id="34556-172">An action that returns a non-`IActionResult` implementation (for example, `object`) also returns a Formatted Response.</span></span>

<span data-ttu-id="34556-173">Alcuni metodi di supporto di questo tipo includono `BadRequest`, `CreatedAtRoute`, e `Ok`.</span><span class="sxs-lookup"><span data-stu-id="34556-173">Some helper methods of this type include `BadRequest`, `CreatedAtRoute`, and `Ok`.</span></span> <span data-ttu-id="34556-174">Esempi di questi metodi includono `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, e `return Ok(value);`, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="34556-174">Examples of these methods include `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, and `return Ok(value);`, respectively.</span></span> <span data-ttu-id="34556-175">Si noti che `BadRequest` e `Ok` eseguono negoziazione del contenuto solo quando viene passato un valore; senza viene passato un valore, vengono invece utilizzati come tipi di risultati di codice di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="34556-175">Note that `BadRequest` and `Ok` perform content negotiation only when passed a value; without being passed a value, they instead serve as HTTP Status Code result types.</span></span> <span data-ttu-id="34556-176">Il `CreatedAtRoute` (metodo), d'altra parte, viene sempre eseguita negoziazione del contenuto dall'overload tutti richiedono di essere passato un valore.</span><span class="sxs-lookup"><span data-stu-id="34556-176">The `CreatedAtRoute` method, on the other hand, always performs content negotiation since its overloads all require that a value be passed.</span></span>

### <a name="cross-cutting-concerns"></a><span data-ttu-id="34556-177">Problemi di montaggio incrociato</span><span class="sxs-lookup"><span data-stu-id="34556-177">Cross-Cutting Concerns</span></span>

<span data-ttu-id="34556-178">Le applicazioni condividono in genere parte del flusso di lavoro.</span><span class="sxs-lookup"><span data-stu-id="34556-178">Applications typically share parts of their workflow.</span></span> <span data-ttu-id="34556-179">Ad esempio un'app che richiede l'autenticazione per accedere il carrello acquisti o un'app che memorizza nella cache di dati in alcune pagine.</span><span class="sxs-lookup"><span data-stu-id="34556-179">Examples include an app that requires authentication to access the shopping cart, or an app that caches data on some pages.</span></span> <span data-ttu-id="34556-180">Per eseguire la logica prima o dopo un metodo di azione, utilizzare un *filtro*.</span><span class="sxs-lookup"><span data-stu-id="34556-180">To perform logic before or after an action method, use a *filter*.</span></span> <span data-ttu-id="34556-181">Utilizzando [filtri](xref:mvc/controllers/filters) sugli aspetti a montaggio incrociato può ridurre la duplicazione, consentendo loro di seguire il [principio non ripetere manualmente (sorgente)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="34556-181">Using [Filters](xref:mvc/controllers/filters) on cross-cutting concerns can reduce duplication, allowing them to follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="34556-182">Più di filtrare gli attributi, ad esempio `[Authorize]`, può essere applicato a livello di controller o un'azione in base al livello desiderato di granularità.</span><span class="sxs-lookup"><span data-stu-id="34556-182">Most filter attributes, such as `[Authorize]`, can be applied at the controller or action level depending upon the desired level of granularity.</span></span>

<span data-ttu-id="34556-183">Gestione degli errori e la memorizzazione nella cache di risposta sono spesso problemi di montaggio incrociato:</span><span class="sxs-lookup"><span data-stu-id="34556-183">Error handling and response caching are often cross-cutting concerns:</span></span>
   * [<span data-ttu-id="34556-184">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="34556-184">Error handling</span></span>](xref:mvc/controllers/filters#exception-filters)
   * [<span data-ttu-id="34556-185">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="34556-185">Response Caching</span></span>](xref:performance/caching/response)

<span data-ttu-id="34556-186">Numerosi problemi di montaggio incrociato possono essere gestiti utilizzando filtri o personalizzata [middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="34556-186">Many cross-cutting concerns can be handled using filters or custom [middleware](xref:fundamentals/middleware).</span></span>