---
title: Riferimento della sintassi Razor per ASP.NET Core
author: rick-anderson
description: Informazioni sulla sintassi Razor per l'incorporamento di codice basato su server in pagine Web.
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: d932e28246998c60e2b3f9c77a2521fe55991e85
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="dd344-103">Sintassi Razor di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dd344-103">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="dd344-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [rottura Mullen uguale o Taylor](https://twitter.com/ntaylormullen), e [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="dd344-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="dd344-105">Razor è una sintassi di markup per l'incorporamento di codice basato su server in pagine Web.</span><span class="sxs-lookup"><span data-stu-id="dd344-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="dd344-106">La sintassi Razor è costituita da Razor markup, c# e HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="dd344-107">File contenente Razor in genere hanno una *. cshtml* estensione di file.</span><span class="sxs-lookup"><span data-stu-id="dd344-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="dd344-108">Il rendering di HTML</span><span class="sxs-lookup"><span data-stu-id="dd344-108">Rendering HTML</span></span>

<span data-ttu-id="dd344-109">Il linguaggio Razor predefinito è HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-109">The default Razor language is HTML.</span></span> <span data-ttu-id="dd344-110">Per il rendering HTML dal tag Razor non è diversa dal rendering di HTML da un file HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="dd344-111">Markup HTML in *. cshtml* file Razor viene eseguito il rendering dal server senza modifiche.</span><span class="sxs-lookup"><span data-stu-id="dd344-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="dd344-112">Sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="dd344-112">Razor syntax</span></span>

<span data-ttu-id="dd344-113">Razor supporta c# e viene utilizzato il `@` simbolo per la transizione da HTML in c#.</span><span class="sxs-lookup"><span data-stu-id="dd344-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="dd344-114">Razor valuta le espressioni c# e li visualizza nell'output HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="dd344-115">Quando un `@` simbolo è seguito da un [parola chiave riservata Razor](#razor-reserved-keywords), passa nel codice Razor specifico.</span><span class="sxs-lookup"><span data-stu-id="dd344-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="dd344-116">In caso contrario, esegue la transizione allo semplice in c#.</span><span class="sxs-lookup"><span data-stu-id="dd344-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="dd344-117">A escape un `@` simbolo nel codice Razor, usare un secondo `@` simbolo:</span><span class="sxs-lookup"><span data-stu-id="dd344-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="dd344-118">Il codice viene eseguito il rendering in HTML con un singolo `@` simbolo:</span><span class="sxs-lookup"><span data-stu-id="dd344-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="dd344-119">Gli attributi HTML e il contenuto che contiene gli indirizzi di posta elettronica non considerano il `@` simbolo come un carattere di transizione.</span><span class="sxs-lookup"><span data-stu-id="dd344-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="dd344-120">Nell'esempio seguente gli indirizzi di posta elettronica sono invariati da Razor l'analisi:</span><span class="sxs-lookup"><span data-stu-id="dd344-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="dd344-121">Espressioni implicite Razor</span><span class="sxs-lookup"><span data-stu-id="dd344-121">Implicit Razor expressions</span></span>

<span data-ttu-id="dd344-122">Le espressioni implicite Razor iniziano con `@` seguito dal codice c#:</span><span class="sxs-lookup"><span data-stu-id="dd344-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="dd344-123">Fatta eccezione per il linguaggio c# `await` (parola chiave), le espressioni implicite non devono contenere spazi.</span><span class="sxs-lookup"><span data-stu-id="dd344-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="dd344-124">Se l'istruzione c# è un chiaro finale, possono coesistere spazi:</span><span class="sxs-lookup"><span data-stu-id="dd344-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="dd344-125">Le espressioni implicite **Impossibile** contengono generics c#, come i caratteri all'interno delle parentesi (`<>`) vengono interpretate come un tag HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="dd344-126">Il codice seguente è **non** valido:</span><span class="sxs-lookup"><span data-stu-id="dd344-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="dd344-127">Il codice precedente genera un errore del compilatore simile a uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="dd344-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="dd344-128">L'elemento "int" non è stata chiusa.</span><span class="sxs-lookup"><span data-stu-id="dd344-128">The "int" element was not closed.</span></span> <span data-ttu-id="dd344-129">Tutti gli elementi devono essere una chiusura automatica o ha un corrispondente tag di fine.</span><span class="sxs-lookup"><span data-stu-id="dd344-129">All elements must be either self-closing or have a matching end tag.</span></span>
 * <span data-ttu-id="dd344-130">Impossibile convertire il gruppo di metodi 'Recuperata un'' per 'object' di tipo non delegato.</span><span class="sxs-lookup"><span data-stu-id="dd344-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="dd344-131">Si intendeva richiamare il metodo?'</span><span class="sxs-lookup"><span data-stu-id="dd344-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="dd344-132">Chiamate di metodo generico devono essere incluso in un [espressione Razor esplicita](#explicit-razor-expressions) o [blocco di codice Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="dd344-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="dd344-133">Espressioni Razor esplicite</span><span class="sxs-lookup"><span data-stu-id="dd344-133">Explicit Razor expressions</span></span>

<span data-ttu-id="dd344-134">Costituiti da espressioni Razor esplicite un `@` simbolo con parentesi bilanciato.</span><span class="sxs-lookup"><span data-stu-id="dd344-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="dd344-135">Per visualizzare l'ora dell'ultima settimana, viene utilizzato il markup Razor seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="dd344-136">Qualsiasi contenuto all'interno di `@()` parentesi viene valutata e il rendering dell'output.</span><span class="sxs-lookup"><span data-stu-id="dd344-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="dd344-137">Le espressioni implicite, descritte nella sezione precedente, in genere non possono contenere spazi.</span><span class="sxs-lookup"><span data-stu-id="dd344-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="dd344-138">Nel codice seguente, una settimana non viene sottratto dall'ora corrente:</span><span class="sxs-lookup"><span data-stu-id="dd344-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="dd344-139">Il codice viene eseguito il rendering HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="dd344-140">Espressioni di tipo Explicit possono essere utilizzate per concatenare testo con un risultato dell'espressione:</span><span class="sxs-lookup"><span data-stu-id="dd344-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="dd344-141">Senza l'espressione esplicita, `<p>Age@joe.Age</p>` viene considerato come un indirizzo di posta elettronica e `<p>Age@joe.Age</p>` viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="dd344-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="dd344-142">Quando scritto in un'espressione esplicita, `<p>Age33</p>` viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="dd344-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="dd344-143">Espressioni di tipo Explicit possono essere utilizzate per il rendering dell'output da metodi generici in *. cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="dd344-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="dd344-144">In un'espressione implicita, i caratteri all'interno delle parentesi (`<>`) vengono interpretate come un tag HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-144">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="dd344-145">È il seguente markup **non** Razor valido:</span><span class="sxs-lookup"><span data-stu-id="dd344-145">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="dd344-146">Il codice precedente genera un errore del compilatore simile a uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="dd344-146">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="dd344-147">L'elemento "int" non è stata chiusa.</span><span class="sxs-lookup"><span data-stu-id="dd344-147">The "int" element was not closed.</span></span> <span data-ttu-id="dd344-148">Tutti gli elementi devono essere una chiusura automatica o ha un corrispondente tag di fine.</span><span class="sxs-lookup"><span data-stu-id="dd344-148">All elements must be either self-closing or have a matching end tag.</span></span>
 * <span data-ttu-id="dd344-149">Impossibile convertire il gruppo di metodi 'Recuperata un'' per 'object' di tipo non delegato.</span><span class="sxs-lookup"><span data-stu-id="dd344-149">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="dd344-150">Si intendeva richiamare il metodo?'</span><span class="sxs-lookup"><span data-stu-id="dd344-150">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="dd344-151">Di seguito viene illustrato l'operazione di scrittura modo corretto il codice.</span><span class="sxs-lookup"><span data-stu-id="dd344-151">The following markup shows the correct way write this code.</span></span> <span data-ttu-id="dd344-152">Il codice viene scritto come un'espressione esplicita:</span><span class="sxs-lookup"><span data-stu-id="dd344-152">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="dd344-153">Codifica di espressione</span><span class="sxs-lookup"><span data-stu-id="dd344-153">Expression encoding</span></span>

<span data-ttu-id="dd344-154">Le espressioni c# che restituiscono una stringa sono codificati in HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-154">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="dd344-155">Le espressioni c# che restituiscono `IHtmlContent` il rendering viene eseguito direttamente tramite `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="dd344-155">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="dd344-156">Le espressioni c# che non restituiscono `IHtmlContent` vengono convertiti in una stringa da `ToString` e codificati prima vengono sottoposti a rendering.</span><span class="sxs-lookup"><span data-stu-id="dd344-156">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="dd344-157">Il codice viene eseguito il rendering HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-157">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="dd344-158">Il codice HTML viene visualizzato nel browser come:</span><span class="sxs-lookup"><span data-stu-id="dd344-158">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="dd344-159">`HtmlHelper.Raw`output non è codificato ma sottoposto a rendering come markup HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-159">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="dd344-160">Utilizzando `HtmlHelper.Raw` utente unsanitized input è un rischio per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="dd344-160">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="dd344-161">L'input dell'utente potrebbe contenere JavaScript dannoso o altri exploit.</span><span class="sxs-lookup"><span data-stu-id="dd344-161">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="dd344-162">La purificazione degli input dell'utente è difficile.</span><span class="sxs-lookup"><span data-stu-id="dd344-162">Sanitizing user input is difficult.</span></span> <span data-ttu-id="dd344-163">Evitare di utilizzare `HtmlHelper.Raw` con l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="dd344-163">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="dd344-164">Il codice viene eseguito il rendering HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-164">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="dd344-165">Blocchi di codice Razor</span><span class="sxs-lookup"><span data-stu-id="dd344-165">Razor code blocks</span></span>

<span data-ttu-id="dd344-166">Blocchi di codice Razor iniziano con `@` e sono racchiusi tra `{}`.</span><span class="sxs-lookup"><span data-stu-id="dd344-166">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="dd344-167">A differenza delle espressioni, non viene eseguito il rendering di codice c# all'interno di blocchi di codice.</span><span class="sxs-lookup"><span data-stu-id="dd344-167">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="dd344-168">Blocchi di codice e le espressioni in una vista condividono lo stesso ambito e vengono definite nell'ordine:</span><span class="sxs-lookup"><span data-stu-id="dd344-168">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

<span data-ttu-id="dd344-169">Il codice viene eseguito il rendering HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-169">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="dd344-170">Transizioni implicite</span><span class="sxs-lookup"><span data-stu-id="dd344-170">Implicit transitions</span></span>

<span data-ttu-id="dd344-171">È la lingua predefinita in un blocco di codice c#, ma la pagina Razor può eseguire la transizione al HTML:</span><span class="sxs-lookup"><span data-stu-id="dd344-171">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="dd344-172">Transizione delimitata esplicita</span><span class="sxs-lookup"><span data-stu-id="dd344-172">Explicit delimited transition</span></span>

<span data-ttu-id="dd344-173">Per definire una sottosezione di un blocco di codice che deve eseguire il rendering HTML, Racchiudi tra i caratteri per il rendering Razor  **\<testo >** tag:</span><span class="sxs-lookup"><span data-stu-id="dd344-173">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="dd344-174">Utilizzare questo approccio per eseguire il rendering HTML che non è racchiuso tra tag HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-174">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="dd344-175">Senza un tag HTML o Razor, si verifica un errore di runtime Razor.</span><span class="sxs-lookup"><span data-stu-id="dd344-175">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="dd344-176">Il  **\<testo >** tag è utile per controllare gli spazi vuoti durante il rendering del contenuto:</span><span class="sxs-lookup"><span data-stu-id="dd344-176">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="dd344-177">Solo il contenuto tra il  **\<testo >** viene eseguito il rendering di tag.</span><span class="sxs-lookup"><span data-stu-id="dd344-177">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="dd344-178">Senza spazi prima o dopo il  **\<testo >** tag viene visualizzato nell'output HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-178">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="dd344-179">Transizione riga esplicita con @:</span><span class="sxs-lookup"><span data-stu-id="dd344-179">Explicit Line Transition with @:</span></span>

<span data-ttu-id="dd344-180">Per visualizzare il resto di un'intera riga in formato HTML all'interno di un blocco di codice, utilizzare il `@:` sintassi:</span><span class="sxs-lookup"><span data-stu-id="dd344-180">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="dd344-181">Senza il `@:` nel codice, viene generato un errore di runtime Razor.</span><span class="sxs-lookup"><span data-stu-id="dd344-181">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="dd344-182">Avviso: Extra `@` caratteri in un file Razor possono causare errori del compilatore causa istruzioni in un secondo momento nel blocco.</span><span class="sxs-lookup"><span data-stu-id="dd344-182">Warning: Extra `@` characters in a Razor file can cause cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="dd344-183">Questi errori del compilatore possono essere difficili da comprendere perché l'errore effettivo si verifica prima dell'errore segnalato.</span><span class="sxs-lookup"><span data-stu-id="dd344-183">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="dd344-184">Questo errore è comune dopo la combinazione di più espressioni implicita/esplicita in un singolo blocco di codice.</span><span class="sxs-lookup"><span data-stu-id="dd344-184">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="dd344-185">Strutture di controllo</span><span class="sxs-lookup"><span data-stu-id="dd344-185">Control Structures</span></span>

<span data-ttu-id="dd344-186">Strutture di controllo sono un'estensione dei blocchi di codice.</span><span class="sxs-lookup"><span data-stu-id="dd344-186">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="dd344-187">Tutti gli aspetti dei blocchi di codice (una transizione di markup, c# inline) inoltre valide per le strutture seguenti:</span><span class="sxs-lookup"><span data-stu-id="dd344-187">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="dd344-188">Istruzioni condizionali @if, altrimenti, else e@switch</span><span class="sxs-lookup"><span data-stu-id="dd344-188">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="dd344-189">`@if`controlli durante l'esecuzione di codice:</span><span class="sxs-lookup"><span data-stu-id="dd344-189">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="dd344-190">`else`e `else if` non richiedono la `@` simbolo:</span><span class="sxs-lookup"><span data-stu-id="dd344-190">`else` and `else if` don't require the `@` symbol:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

<span data-ttu-id="dd344-191">Il markup seguente viene illustrato come utilizzare un'istruzione switch:</span><span class="sxs-lookup"><span data-stu-id="dd344-191">The following markup shows how to use a switch statement:</span></span>

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="dd344-192">Ciclo @for, @foreach, @while, e @do mentre</span><span class="sxs-lookup"><span data-stu-id="dd344-192">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="dd344-193">HTML basato su modelli può essere visualizzato con le istruzioni di controllo ciclo.</span><span class="sxs-lookup"><span data-stu-id="dd344-193">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="dd344-194">Per eseguire il rendering di un elenco di persone:</span><span class="sxs-lookup"><span data-stu-id="dd344-194">To render a list of people:</span></span>

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

<span data-ttu-id="dd344-195">Sono supportate le seguenti istruzioni di ciclo:</span><span class="sxs-lookup"><span data-stu-id="dd344-195">The following looping statements are supported:</span></span>

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="dd344-196">Composta@using</span><span class="sxs-lookup"><span data-stu-id="dd344-196">Compound @using</span></span>

<span data-ttu-id="dd344-197">In c#, un `using` istruzione viene utilizzata per verificare che un oggetto è stato eliminato.</span><span class="sxs-lookup"><span data-stu-id="dd344-197">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="dd344-198">In Razor, lo stesso meccanismo viene utilizzato per creare l'helper HTML che contiene contenuto aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="dd344-198">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="dd344-199">Nel codice seguente, il rendering di un tag form con helper HTML di `@using` istruzione:</span><span class="sxs-lookup"><span data-stu-id="dd344-199">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

<span data-ttu-id="dd344-200">Azioni a livello di ambito possono essere eseguite con [gli helper di Tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="dd344-200">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="dd344-201">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="dd344-201">@try, catch, finally</span></span>

<span data-ttu-id="dd344-202">Gestione delle eccezioni è simile a c#:</span><span class="sxs-lookup"><span data-stu-id="dd344-202">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="dd344-203">Razor è in grado di proteggere le sezioni critiche con le istruzioni di blocco:</span><span class="sxs-lookup"><span data-stu-id="dd344-203">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="dd344-204">Commenti</span><span class="sxs-lookup"><span data-stu-id="dd344-204">Comments</span></span>

<span data-ttu-id="dd344-205">Razor supporta commenti in c# e il codice HTML:</span><span class="sxs-lookup"><span data-stu-id="dd344-205">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="dd344-206">Il codice viene eseguito il rendering HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-206">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="dd344-207">Commenti Razor vengono rimossi dal server prima del rendering della pagina Web.</span><span class="sxs-lookup"><span data-stu-id="dd344-207">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="dd344-208">Utilizza Razor `@*  *@` per delimitare i commenti.</span><span class="sxs-lookup"><span data-stu-id="dd344-208">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="dd344-209">Il codice seguente viene impostato come commento, in modo il server non esegue il rendering di tag:</span><span class="sxs-lookup"><span data-stu-id="dd344-209">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="dd344-210">Direttive</span><span class="sxs-lookup"><span data-stu-id="dd344-210">Directives</span></span>

<span data-ttu-id="dd344-211">Direttive Razor vengono rappresentate tramite espressioni implicite con seguenti le parole chiave riservate di `@` simbolo.</span><span class="sxs-lookup"><span data-stu-id="dd344-211">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="dd344-212">Una direttiva cambia in genere la modalità di una vista viene analizzata o Abilita funzionalità diverse.</span><span class="sxs-lookup"><span data-stu-id="dd344-212">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="dd344-213">Comprendere come Razor genera il codice per una vista, è facile comprendere il funzionano delle direttive.</span><span class="sxs-lookup"><span data-stu-id="dd344-213">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="dd344-214">Il codice genera una classe simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-214">The code generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="dd344-215">Più avanti in questo articolo, la sezione [visualizzazione classe Razor c# generato per una vista](#viewing-the-razor-c-class-generated-for-a-view) viene illustrato come visualizzare la classe generata.</span><span class="sxs-lookup"><span data-stu-id="dd344-215">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="dd344-216">Il `@using` direttiva aggiunge c# `using` direttiva alla visualizzazione generata:</span><span class="sxs-lookup"><span data-stu-id="dd344-216">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="dd344-217">Il `@model` direttiva specifica il tipo di modello passato a una visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="dd344-217">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="dd344-218">In un'applicazione ASP.NET MVC di base creata con l'account utente individuali, il *Views/Account/Login.cshtml* vista contiene la dichiarazione di modello seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-218">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="dd344-219">La classe generata eredita da `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="dd344-219">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="dd344-220">Razor espone un `Model` proprietà per l'accesso al modello passato alla visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="dd344-220">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="dd344-221">Il `@model` direttiva specifica il tipo di questa proprietà.</span><span class="sxs-lookup"><span data-stu-id="dd344-221">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="dd344-222">Specifica la direttiva di `T` in `RazorPage<T>` che generato classe che la visualizzazione deriva da.</span><span class="sxs-lookup"><span data-stu-id="dd344-222">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="dd344-223">Se il `@model` iisn't direttiva specificata, il `Model` proprietà è di tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd344-223">If the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="dd344-224">Il valore del modello viene passato dal controller alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="dd344-224">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="dd344-225">Per ulteriori informazioni, vedere [fortemente tipizzate modelli e @model (parola chiave).</span><span class="sxs-lookup"><span data-stu-id="dd344-225">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="dd344-226">Il `@inherits` direttiva fornisce il controllo completo della classe che eredita la vista:</span><span class="sxs-lookup"><span data-stu-id="dd344-226">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="dd344-227">Il codice seguente è un tipo di pagina Razor personalizzato:</span><span class="sxs-lookup"><span data-stu-id="dd344-227">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="dd344-228">Il `CustomText` viene visualizzato in una vista:</span><span class="sxs-lookup"><span data-stu-id="dd344-228">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="dd344-229">Il codice viene eseguito il rendering HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-229">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="dd344-230">`@model`e `@inherits` può essere utilizzato nella stessa visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="dd344-230">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="dd344-231">`@inherits`può trovarsi in un *viewimports. cshtml* file che l'importazione nella visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="dd344-231">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="dd344-232">Il codice riportato di seguito è riportato un esempio di una visualizzazione fortemente tipizzata:</span><span class="sxs-lookup"><span data-stu-id="dd344-232">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="dd344-233">Se "rick@contoso.com" viene passato nel modello, la vista genera il markup HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-233">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="dd344-234">Il `@inject` direttiva consente alla pagina Razor inserire un servizio di [contenitore dei servizi](xref:fundamentals/dependency-injection) in una vista.</span><span class="sxs-lookup"><span data-stu-id="dd344-234">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="dd344-235">Per ulteriori informazioni, vedere [Dependency injection nelle viste](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="dd344-235">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="dd344-236">Il `@functions` direttiva consente a una pagina Razor aggiungere contenuto a livello di funzione a una vista:</span><span class="sxs-lookup"><span data-stu-id="dd344-236">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="dd344-237">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dd344-237">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="dd344-238">Il codice genera il markup HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="dd344-238">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="dd344-239">Il codice seguente è la classe generata Razor c#:</span><span class="sxs-lookup"><span data-stu-id="dd344-239">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="dd344-240">Il `@section` direttiva viene utilizzata in combinazione con il [layout](xref:mvc/views/layout) per abilitare le viste per il rendering del contenuto in diverse parti della pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="dd344-240">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="dd344-241">Per ulteriori informazioni, vedere [sezioni](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="dd344-241">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="dd344-242">Helper di tag</span><span class="sxs-lookup"><span data-stu-id="dd344-242">Tag Helpers</span></span>

<span data-ttu-id="dd344-243">Esistono tre direttive che riguardano [gli helper di Tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="dd344-243">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="dd344-244">Direttiva</span><span class="sxs-lookup"><span data-stu-id="dd344-244">Directive</span></span> | <span data-ttu-id="dd344-245">Funzione</span><span class="sxs-lookup"><span data-stu-id="dd344-245">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="dd344-246">Rende disponibili per una vista gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="dd344-246">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="dd344-247">Rimuove gli helper di Tag aggiunti in precedenza da una vista.</span><span class="sxs-lookup"><span data-stu-id="dd344-247">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="dd344-248">Specifica un prefisso di tag per abilitare il supporto di Helper di Tag e rendere esplicito l'utilizzo di Helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="dd344-248">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="dd344-249">Parole chiave riservate Razor</span><span class="sxs-lookup"><span data-stu-id="dd344-249">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="dd344-250">Parole chiave Razor</span><span class="sxs-lookup"><span data-stu-id="dd344-250">Razor keywords</span></span>

* <span data-ttu-id="dd344-251">pagina (richiede ASP.NET Core 2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="dd344-251">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="dd344-252">funzioni</span><span class="sxs-lookup"><span data-stu-id="dd344-252">functions</span></span>
* <span data-ttu-id="dd344-253">eredita</span><span class="sxs-lookup"><span data-stu-id="dd344-253">inherits</span></span>
* <span data-ttu-id="dd344-254">modello</span><span class="sxs-lookup"><span data-stu-id="dd344-254">model</span></span>
* <span data-ttu-id="dd344-255">section</span><span class="sxs-lookup"><span data-stu-id="dd344-255">section</span></span>
* <span data-ttu-id="dd344-256">helper (attualmente non supportate da ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="dd344-256">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="dd344-257">Parole chiave Razor sono precedute da `@(Razor Keyword)` (ad esempio, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="dd344-257">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="dd344-258">Parole chiave c# Razor</span><span class="sxs-lookup"><span data-stu-id="dd344-258">C# Razor keywords</span></span>

* <span data-ttu-id="dd344-259">maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="dd344-259">case</span></span>
* <span data-ttu-id="dd344-260">do</span><span class="sxs-lookup"><span data-stu-id="dd344-260">do</span></span>
* <span data-ttu-id="dd344-261">default</span><span class="sxs-lookup"><span data-stu-id="dd344-261">default</span></span>
* <span data-ttu-id="dd344-262">for</span><span class="sxs-lookup"><span data-stu-id="dd344-262">for</span></span>
* <span data-ttu-id="dd344-263">foreach</span><span class="sxs-lookup"><span data-stu-id="dd344-263">foreach</span></span>
* <span data-ttu-id="dd344-264">if</span><span class="sxs-lookup"><span data-stu-id="dd344-264">if</span></span>
* <span data-ttu-id="dd344-265">else</span><span class="sxs-lookup"><span data-stu-id="dd344-265">else</span></span>
* <span data-ttu-id="dd344-266">blocco</span><span class="sxs-lookup"><span data-stu-id="dd344-266">lock</span></span>
* <span data-ttu-id="dd344-267">switch</span><span class="sxs-lookup"><span data-stu-id="dd344-267">switch</span></span>
* <span data-ttu-id="dd344-268">try</span><span class="sxs-lookup"><span data-stu-id="dd344-268">try</span></span>
* <span data-ttu-id="dd344-269">catch</span><span class="sxs-lookup"><span data-stu-id="dd344-269">catch</span></span>
* <span data-ttu-id="dd344-270">finally</span><span class="sxs-lookup"><span data-stu-id="dd344-270">finally</span></span>
* <span data-ttu-id="dd344-271">utilizzo</span><span class="sxs-lookup"><span data-stu-id="dd344-271">using</span></span>
* <span data-ttu-id="dd344-272">while</span><span class="sxs-lookup"><span data-stu-id="dd344-272">while</span></span>

<span data-ttu-id="dd344-273">Parole chiave c# Razor devono essere doppio carattere di escape con `@(@C# Razor Keyword)` (ad esempio, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="dd344-273">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="dd344-274">Il primo `@` ignora il parser Razor.</span><span class="sxs-lookup"><span data-stu-id="dd344-274">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="dd344-275">Il secondo `@` ignora il parser del linguaggio c#.</span><span class="sxs-lookup"><span data-stu-id="dd344-275">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="dd344-276">Parole chiave riservate non utilizzate da Razor</span><span class="sxs-lookup"><span data-stu-id="dd344-276">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="dd344-277">namespace</span><span class="sxs-lookup"><span data-stu-id="dd344-277">namespace</span></span>
* <span data-ttu-id="dd344-278">classe</span><span class="sxs-lookup"><span data-stu-id="dd344-278">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="dd344-279">Visualizzazione della classe c# Razor generata per una vista</span><span class="sxs-lookup"><span data-stu-id="dd344-279">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="dd344-280">Aggiungere la classe seguente al progetto ASP.NET MVC di base:</span><span class="sxs-lookup"><span data-stu-id="dd344-280">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="dd344-281">Eseguire l'override di `RazorTemplateEngine` aggiunto da MVC con la `CustomTemplateEngine` classe:</span><span class="sxs-lookup"><span data-stu-id="dd344-281">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="dd344-282">Impostare un punto di interruzione per il `return csharpDocument` istruzione di `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="dd344-282">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="dd344-283">Quando l'esecuzione del programma si interrompe al punto di interruzione, visualizzare il valore di `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="dd344-283">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Visualizzazione di generatedCode Visualizzatore di testo](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="dd344-285">Le ricerche di visualizzazione e distinzione maiuscole/minuscole</span><span class="sxs-lookup"><span data-stu-id="dd344-285">View lookups and case sensitivity</span></span>

<span data-ttu-id="dd344-286">Il motore di visualizzazione Razor esegue ricerche tra maiuscole e minuscole per le viste.</span><span class="sxs-lookup"><span data-stu-id="dd344-286">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="dd344-287">Tuttavia, la ricerca effettiva è determinata dal file system sottostante:</span><span class="sxs-lookup"><span data-stu-id="dd344-287">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="dd344-288">File di origine in base:</span><span class="sxs-lookup"><span data-stu-id="dd344-288">File based source:</span></span> 
  * <span data-ttu-id="dd344-289">Nei sistemi operativi con i sistemi di file tra maiuscole e minuscole (ad esempio, Windows), le ricerche di file fisico provider si trovano tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="dd344-289">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="dd344-290">Ad esempio, `return View("Test")` comporta corrispondenze per */Views/Home/Test.cshtml*, */Views/home/test.cshtml*e qualsiasi altra variante di maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="dd344-290">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="dd344-291">Nei sistemi di file tra maiuscole e minuscole (ad esempio Linux, OS x e con `EmbeddedFileProvider`), le ricerche tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="dd344-291">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="dd344-292">Ad esempio, `return View("Test")` specificamente corrispondenze */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dd344-292">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="dd344-293">Precompilata viste: con il Core ASP.NET 2.0 e versioni successivo, la ricerca di viste precompilate è tra maiuscole e minuscole in tutti i sistemi operativi.</span><span class="sxs-lookup"><span data-stu-id="dd344-293">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="dd344-294">Il comportamento è identico al comportamento del provider di file fisico in Windows.</span><span class="sxs-lookup"><span data-stu-id="dd344-294">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="dd344-295">Se due visualizzazioni precompilate differiscono solo nel caso, il risultato di ricerca è non deterministico.</span><span class="sxs-lookup"><span data-stu-id="dd344-295">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="dd344-296">Gli sviluppatori sono invitati a corrispondere le maiuscole e minuscole dei nomi di file e directory per le maiuscole e minuscole di:</span><span class="sxs-lookup"><span data-stu-id="dd344-296">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="dd344-297">Nomi di area, controller e azione.</span><span class="sxs-lookup"><span data-stu-id="dd344-297">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="dd344-298">Pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="dd344-298">Razor Pages.</span></span>
    
<span data-ttu-id="dd344-299">Minuscole assicura che le distribuzioni di trovare le visualizzazioni indipendentemente dal file system sottostante.</span><span class="sxs-lookup"><span data-stu-id="dd344-299">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>