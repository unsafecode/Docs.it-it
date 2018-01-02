---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Utilizzo di controlli controllo AJAX e controllo Extender (c#) | Documenti Microsoft
author: microsoft
description: Informazioni su come aggiungere controlli AJAX Control Toolkit ed estensioni per le pagine ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 42a210ac41e83e2379aa64979f42ce66c843f878
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a><span data-ttu-id="7b92d-103">Utilizzo di controlli controllo AJAX e controllo Extender (c#)</span><span class="sxs-lookup"><span data-stu-id="7b92d-103">Using AJAX Control Toolkit Controls and Control Extenders (C#)</span></span>
====================
<span data-ttu-id="7b92d-104">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7b92d-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7b92d-105">Informazioni su come aggiungere controlli AJAX Control Toolkit ed estensioni per le pagine ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7b92d-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="7b92d-106">AJAX Control Toolkit contiene un set di controlli e controllo Extender.</span><span class="sxs-lookup"><span data-stu-id="7b92d-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="7b92d-107">In questa breve esercitazione, come aggiungere controlli e controllo Extender per una pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7b92d-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7b92d-108">Per istruzioni sull'installazione AJAX Control Toolkit e aggiunta di AJAX Control Toolkit nella casella degli strumenti di Visual Studio e Visual Web Developer, vedere l'esercitazione [introduzione AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span><span class="sxs-lookup"><span data-stu-id="7b92d-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="7b92d-109">Utilizzo di controlli controllo AJAX</span><span class="sxs-lookup"><span data-stu-id="7b92d-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="7b92d-110">Un controllo AJAX Control Toolkit funziona come un normale controllo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7b92d-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="7b92d-111">È possibile trascinare il controllo dalla casella degli strumenti in una pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7b92d-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="7b92d-112">È possibile aggiungere il controllo alla pagina in visualizzazione progettazione o visualizzazione origine.</span><span class="sxs-lookup"><span data-stu-id="7b92d-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="7b92d-113">È richiesta una speciale quando si utilizzano i controlli di AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="7b92d-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="7b92d-114">La pagina deve contenere un controllo ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="7b92d-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="7b92d-115">Il controllo ScriptManager è responsabile per l'inclusione di tutto il codice JavaScript necessario richiesto dai controlli AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="7b92d-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="7b92d-116">Ad esempio, la scheda AJAX Control Toolkit include un controllo denominato il controllo dell'Editor.</span><span class="sxs-lookup"><span data-stu-id="7b92d-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="7b92d-117">Questo controllo consente di visualizzare un editor HTML avanzato.</span><span class="sxs-lookup"><span data-stu-id="7b92d-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="7b92d-118">Seguire questi passaggi per aggiungere il controllo dell'Editor a una pagina:</span><span class="sxs-lookup"><span data-stu-id="7b92d-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="7b92d-119">Creare una nuova pagina ASP.NET denominata ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="7b92d-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="7b92d-120">Selezionare il controllo ScriptManager; scheda Estensioni AJAX nella casella degli strumenti e trascinare il controllo nella pagina.</span><span class="sxs-lookup"><span data-stu-id="7b92d-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="7b92d-121">Selezionare il controllo dell'Editor; scheda AJAX Control Toolkit nella casella degli strumenti e trascinare il controllo nella pagina (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="7b92d-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="7b92d-122">La finestra di progettazione sarà come nella figura 2.</span><span class="sxs-lookup"><span data-stu-id="7b92d-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="7b92d-123">Eseguire il sito web selezionando l'opzione di menu **Debug, Avvia debug** o premendo il tasto F5.</span><span class="sxs-lookup"><span data-stu-id="7b92d-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="7b92d-124">Verrà visualizzata la pagina nella figura 3.</span><span class="sxs-lookup"><span data-stu-id="7b92d-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="7b92d-125">[![Selezionare il controllo dell'Editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7b92d-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span></span>

<span data-ttu-id="7b92d-126">**Figura 01**: selezione del controllo Editor HTML ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="7b92d-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span></span>


<span data-ttu-id="7b92d-127">[![Progettazione di Visual Studio con il controllo ScriptManager e modifica](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="7b92d-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span></span>

<span data-ttu-id="7b92d-128">**Figura 02**: progettazione di Visual Studio con il controllo ScriptManager e modifica ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="7b92d-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span></span>


<span data-ttu-id="7b92d-129">[![La pagina DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="7b92d-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span></span>

<span data-ttu-id="7b92d-130">**Figura 03**: DisplayEditor.aspx di pagina ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7b92d-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="7b92d-131">Utilizzo di AJAX controllo Toolkit controllo Extender</span><span class="sxs-lookup"><span data-stu-id="7b92d-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="7b92d-132">AJAX Control Toolkit contiene inoltre controllo Extender.</span><span class="sxs-lookup"><span data-stu-id="7b92d-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="7b92d-133">Come suggerisce il nome, un controllo extender estende la funzionalità di un controllo esistente.</span><span class="sxs-lookup"><span data-stu-id="7b92d-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="7b92d-134">Ad esempio, il controllo extender ConfirmButton estende il controllo pulsante di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7b92d-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="7b92d-135">L'estensione cambia il comportamento di pulsanti di controllo s in modo che il pulsante Visualizza una finestra di dialogo di conferma quando si fa clic sopra.</span><span class="sxs-lookup"><span data-stu-id="7b92d-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="7b92d-136">Un controllo extender, come avviene per un controllo AJAX Control Toolkit, è necessario un controllo ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="7b92d-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="7b92d-137">Prima di iniziare a usare le estensioni di controllo nella pagina, è necessario aggiungere un controllo ScriptManager a una pagina.</span><span class="sxs-lookup"><span data-stu-id="7b92d-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="7b92d-138">Seguire questi passaggi per utilizzare l'estensione di controllo ConfirmButton:</span><span class="sxs-lookup"><span data-stu-id="7b92d-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="7b92d-139">Creare una nuova pagina ASP.NET denominata ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="7b92d-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="7b92d-140">Aggiungere un controllo ScriptManager alla pagina, trascinare il controllo nella pagina di livello inferiore della scheda Estensioni AJAX.</span><span class="sxs-lookup"><span data-stu-id="7b92d-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="7b92d-141">Aggiungere un controllo pulsante alla pagina trascinando il pulsante; scheda Standard della casella degli strumenti nell'area di progettazione.</span><span class="sxs-lookup"><span data-stu-id="7b92d-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="7b92d-142">Fare clic su di **Aggiungi estensione** attività opzione (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="7b92d-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="7b92d-143">Nella finestra di dialogo scegliere Extender selezionare ConfirmButtonExtender (vedere Figura 5) e fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="7b92d-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="7b92d-144">Selezionare il controllo pulsante nella finestra di progettazione ed espandere le estensioni, Button1\_nodo ConfirmButtonExtender nella finestra Proprietà (vedere Figura 6).</span><span class="sxs-lookup"><span data-stu-id="7b92d-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="7b92d-145">Assegnare il valore *effettivamente?* alla proprietà ConfirmText.</span><span class="sxs-lookup"><span data-stu-id="7b92d-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="7b92d-146">Eseguire la pagina selezionando l'opzione di menu **Debug, Avvia debug** o premere il tasto F5.</span><span class="sxs-lookup"><span data-stu-id="7b92d-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="7b92d-147">[![L'opzione di attività Aggiungi estensione](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="7b92d-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span></span>

<span data-ttu-id="7b92d-148">**Figura 04**: opzione di estensione aggiungere l'attività ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="7b92d-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span></span>


<span data-ttu-id="7b92d-149">[![Selezionare il controllo extender ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="7b92d-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span></span>

<span data-ttu-id="7b92d-150">**Figura 05**: selezionando il controllo extender ConfirmButton ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="7b92d-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span></span>


<span data-ttu-id="7b92d-151">[![Impostazione di una proprietà ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="7b92d-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span></span>

<span data-ttu-id="7b92d-152">**Figura 06**: impostazione di una proprietà ConfirmButton ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="7b92d-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span></span>


<span data-ttu-id="7b92d-153">Quando si apre la pagina, verrà visualizzato un pulsante.</span><span class="sxs-lookup"><span data-stu-id="7b92d-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="7b92d-154">Quando si fa clic sul pulsante, la finestra di dialogo di conferma è visualizzato nella figura 7.</span><span class="sxs-lookup"><span data-stu-id="7b92d-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="7b92d-155">[![Visualizzare la finestra di dialogo di conferma](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="7b92d-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span></span>

<span data-ttu-id="7b92d-156">**Figura 07**: visualizzare la finestra di dialogo di conferma ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="7b92d-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span></span>


<span data-ttu-id="7b92d-157">Si noti che in genere non si trascina un controllo extender in una pagina.</span><span class="sxs-lookup"><span data-stu-id="7b92d-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="7b92d-158">Utilizzare invece il **Aggiungi estensione** opzione per aggiungere un'estensione a un controllo che è già stato aggiunto a una pagina di attività.</span><span class="sxs-lookup"><span data-stu-id="7b92d-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="7b92d-159">Si noti, inoltre, impostare controllo delle proprietà di estensione, aprire la finestra delle proprietà per il controllo da estendere.</span><span class="sxs-lookup"><span data-stu-id="7b92d-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="7b92d-160">Un singolo controllo ASP.NET può essere esteso da più estensioni di controllo.</span><span class="sxs-lookup"><span data-stu-id="7b92d-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="7b92d-161">Finestra delle proprietà per il controllo viene esteso elencherà tutte le estensioni di controllo associate al controllo.</span><span class="sxs-lookup"><span data-stu-id="7b92d-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7b92d-162">[Precedente](get-started-with-the-ajax-control-toolkit-cs.md)
[Successivo](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7b92d-162">[Previous](get-started-with-the-ajax-control-toolkit-cs.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span></span>