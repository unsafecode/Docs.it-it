---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Creazione di caselle di controllo si escludono a vicenda (VB) | Documenti Microsoft
author: wenz
description: "Quando può essere selezionato solo uno di un set di opzioni, pulsanti di opzione in genere vengono utilizzati. È uno svantaggio, tuttavia: dopo aver selezionato un pulsante di opzione in un gruppo,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 023ca0b145de8147a98e78f4dba20846dc344f06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="f5bfc-104">Creazione di caselle di controllo si escludono a vicenda (VB)</span><span class="sxs-lookup"><span data-stu-id="f5bfc-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>
====================
<span data-ttu-id="f5bfc-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f5bfc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f5bfc-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f5bfc-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="f5bfc-107">Quando può essere selezionato solo uno di un set di opzioni, pulsanti di opzione in genere vengono utilizzati.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="f5bfc-108">È uno svantaggio, tuttavia: dopo aver selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="f5bfc-109">Caselle di controllo può essere deselezionate in qualsiasi momento, tuttavia non si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="f5bfc-110">In questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="f5bfc-111">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f5bfc-111">Overview</span></span>

<span data-ttu-id="f5bfc-112">Quando può essere selezionato solo uno di un set di opzioni, pulsanti di opzione in genere vengono utilizzati.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="f5bfc-113">È uno svantaggio, tuttavia: dopo aver selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="f5bfc-114">Caselle di controllo può essere deselezionate in qualsiasi momento, tuttavia non si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="f5bfc-115">In questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="f5bfc-116">Passaggi</span><span class="sxs-lookup"><span data-stu-id="f5bfc-116">Steps</span></span>

<span data-ttu-id="f5bfc-117">ASP.NET AJAX Control Toolkit contiene l'estensione MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="f5bfc-118">Questo consente ai programmatori di assegnare un nome di gruppo di qualsiasi casella di controllo (`Key` attributo).</span><span class="sxs-lookup"><span data-stu-id="f5bfc-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="f5bfc-119">Da tutte le caselle di controllo nello stesso gruppo, è possibile selezionare solo una alla volta.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="f5bfc-120">Per iniziare l'inserimento di due caselle di controllo in una nuova pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="f5bfc-121">Possono esistere più, ma due di essi sono sufficienti per illustrare il principio:</span><span class="sxs-lookup"><span data-stu-id="f5bfc-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="f5bfc-122">Per entrambe le caselle di controllo, un controllo MutuallyExclusiveCheckBoxExtender deve essere inserito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="f5bfc-123">Entrambi gli attributi di chiave necessario hanno lo stesso valore, come il valore degli attributi degli elementi di pulsante di opzione HTML devono essere identici per indicare il gruppo che cui appartengono.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="f5bfc-124">La proprietà TargetControlID dei punti di estensione per l'ID della casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="f5bfc-125">Infine, includere ASP.NET AJAX `ScriptManager` che sono richieste da tutti gli elementi di ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="f5bfc-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="f5bfc-126">Salvare ed eseguire la pagina: È possibile selezionare e deselezionare caselle di controllo, tuttavia non entrambe le caselle di controllo controllare.</span><span class="sxs-lookup"><span data-stu-id="f5bfc-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="f5bfc-127">[![Solo una casella di controllo può essere verificata in un momento](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f5bfc-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="f5bfc-128">Solo una casella di controllo può essere verificata in un momento ([fare clic per visualizzare l'immagine ingrandita](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f5bfc-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f5bfc-129">Precedente</span><span class="sxs-lookup"><span data-stu-id="f5bfc-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)