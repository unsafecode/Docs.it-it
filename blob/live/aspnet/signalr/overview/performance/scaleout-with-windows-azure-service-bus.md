---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: "Scalabilità orizzontale SignalR con il Bus di servizio di Azure | Documenti Microsoft"
author: MikeWasson
description: Versioni del software utilizzato in questa versione di Visual Studio 2013 .NET 4.5 SignalR argomento 2 nelle versioni precedenti di questa versione di 1. x SignalR per l'argomento di questo argomento,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 857fc8baa61549e2fabbb8da012b1fa23950237d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="b06ad-103">Scalabilità orizzontale SignalR con il Bus di servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="b06ad-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="b06ad-104">da [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b06ad-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="b06ad-105">In questa esercitazione, si distribuirà un'applicazione di SignalR per un ruolo Web di Azure di Windows, con il backplane del Bus di servizio per distribuire i messaggi per ogni istanza del ruolo.</span><span class="sxs-lookup"><span data-stu-id="b06ad-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="b06ad-106">(È anche possibile usare il backplane del Bus di servizio con [web App in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="b06ad-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="b06ad-107">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="b06ad-107">Prerequisites:</span></span>

- <span data-ttu-id="b06ad-108">Un account di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b06ad-108">A Windows Azure account.</span></span>
- <span data-ttu-id="b06ad-109">Il [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="b06ad-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="b06ad-110">Visual Studio 2012 o 2013.</span><span class="sxs-lookup"><span data-stu-id="b06ad-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="b06ad-111">Backplane di bus di servizio è inoltre compatibile con [Service Bus per Windows Server](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx), versione 1.1.</span><span class="sxs-lookup"><span data-stu-id="b06ad-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="b06ad-112">Non è tuttavia compatibile con la versione 1.0 di Service Bus per Windows Server.</span><span class="sxs-lookup"><span data-stu-id="b06ad-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="b06ad-113">Pricing</span><span class="sxs-lookup"><span data-stu-id="b06ad-113">Pricing</span></span>

<span data-ttu-id="b06ad-114">Bus di servizio backplane utilizza argomenti per inviare messaggi.</span><span class="sxs-lookup"><span data-stu-id="b06ad-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="b06ad-115">Per informazioni più recenti sui prezzi, vedere [Bus di servizio](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="b06ad-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="b06ad-116">Al momento della redazione del presente documento, è possibile inviare 1.000.000 messaggi al mese per meno di $1.</span><span class="sxs-lookup"><span data-stu-id="b06ad-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="b06ad-117">Backplane invia un messaggio del bus di servizio per ogni chiamata di un metodo dell'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="b06ad-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="b06ad-118">Esistono inoltre alcuni messaggi di controllo per le connessioni, disconnessioni, unione o uscire da gruppi e così via.</span><span class="sxs-lookup"><span data-stu-id="b06ad-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="b06ad-119">Nella maggior parte delle applicazioni, la maggior parte del traffico dei messaggi sarà chiamate del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="b06ad-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="b06ad-120">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b06ad-120">Overview</span></span>

<span data-ttu-id="b06ad-121">Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.</span><span class="sxs-lookup"><span data-stu-id="b06ad-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="b06ad-122">Utilizzare il portale Windows Azure per creare un nuovo spazio dei nomi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b06ad-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="b06ad-123">Aggiungere questi pacchetti NuGet per l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="b06ad-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="b06ad-124">Microsoft. Aspnet</span><span class="sxs-lookup"><span data-stu-id="b06ad-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="b06ad-125">Italiano</span><span class="sxs-lookup"><span data-stu-id="b06ad-125">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="b06ad-126">Creare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="b06ad-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="b06ad-127">Aggiungere il codice seguente al Startup.cs configurare backplane:</span><span class="sxs-lookup"><span data-stu-id="b06ad-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="b06ad-128">Questo codice consente di configurare backplane con i valori predefiniti per [TopicCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="b06ad-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="b06ad-129">Per informazioni sulla modifica di questi valori, vedere [delle prestazioni di SignalR: metriche di scalabilità orizzontale](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="b06ad-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="b06ad-130">Per ogni applicazione, selezionare un valore diverso per "NomeApp".</span><span class="sxs-lookup"><span data-stu-id="b06ad-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="b06ad-131">Non utilizzare lo stesso valore in più applicazioni.</span><span class="sxs-lookup"><span data-stu-id="b06ad-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="b06ad-132">Creare i servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="b06ad-132">Create the Azure Services</span></span>

<span data-ttu-id="b06ad-133">Creare un servizio Cloud, come descritto in [come creare e distribuire un servizio Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="b06ad-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="b06ad-134">Seguire i passaggi nella sezione "procedura: creare un servizio cloud utilizzando Creazione rapida".</span><span class="sxs-lookup"><span data-stu-id="b06ad-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="b06ad-135">Per questa esercitazione, non è necessario caricare un certificato.</span><span class="sxs-lookup"><span data-stu-id="b06ad-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="b06ad-136">Creare un nuovo spazio dei nomi Service Bus, come descritto in [come al Bus di servizio utilizzare argomenti/sottoscrizioni](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="b06ad-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="b06ad-137">Seguire i passaggi nella sezione "Creare un servizio Namespace".</span><span class="sxs-lookup"><span data-stu-id="b06ad-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="b06ad-138">Assicurarsi di selezionare la stessa area per il servizio cloud e lo spazio dei nomi del Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="b06ad-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="b06ad-139">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b06ad-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="b06ad-140">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b06ad-140">Start Visual Studio.</span></span> <span data-ttu-id="b06ad-141">Dal **File** menu, fare clic su **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="b06ad-142">Nel **nuovo progetto** finestra di dialogo espandere **Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="b06ad-143">In **modelli installati**selezionare **Cloud** e quindi selezionare **servizio Cloud Azure**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="b06ad-144">Mantenere il valore predefinito di .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="b06ad-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="b06ad-145">L'applicazione ChatService e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="b06ad-146">Nel **nuovo servizio Cloud Azure** finestra di dialogo, selezionare i ruoli Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b06ad-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="b06ad-147">Fare clic sul pulsante freccia destra (**&gt;**) per aggiungere il ruolo alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="b06ad-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="b06ad-148">Posizionare il mouse sul nuovo ruolo, quindi sull'icona della matita visibile.</span><span class="sxs-lookup"><span data-stu-id="b06ad-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="b06ad-149">Fare clic su questa icona per rinominare il ruolo.</span><span class="sxs-lookup"><span data-stu-id="b06ad-149">Click this icon to rename the role.</span></span> <span data-ttu-id="b06ad-150">Il ruolo "SignalRChat" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="b06ad-151">Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona **MVC**, fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="b06ad-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="b06ad-152">La creazione guidata progetto crea due progetti:</span><span class="sxs-lookup"><span data-stu-id="b06ad-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="b06ad-153">ChatService: Questo progetto è l'applicazione di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b06ad-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="b06ad-154">Definisce i ruoli Azure e altre opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="b06ad-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="b06ad-155">SignalRChat: Questo progetto è il progetto ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="b06ad-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="b06ad-156">Creare l'applicazione di Chat di SignalR</span><span class="sxs-lookup"><span data-stu-id="b06ad-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="b06ad-157">Per creare l'applicazione di chat, seguire i passaggi nell'esercitazione [Guida introduttiva a SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="b06ad-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="b06ad-158">Utilizzare NuGet per installare le librerie necessarie.</span><span class="sxs-lookup"><span data-stu-id="b06ad-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="b06ad-159">Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-159">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b06ad-160">Nel **Package Manager Console** finestra, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b06ad-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="b06ad-161">Utilizzare il `-ProjectName` opzione per installare i pacchetti del progetto ASP.NET MVC, piuttosto che il progetto di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b06ad-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="b06ad-162">Configurare il Backplane</span><span class="sxs-lookup"><span data-stu-id="b06ad-162">Configure the Backplane</span></span>

<span data-ttu-id="b06ad-163">Nel file Startup.cs dell'applicazione, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b06ad-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="b06ad-164">È necessario ottenere una stringa di connessione del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="b06ad-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="b06ad-165">Nel portale di Azure, selezionare i nomi del bus di servizio creato e fare clic sull'icona di tasto di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="b06ad-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="b06ad-166">Copiare la stringa di connessione negli Appunti, quindi incollarlo nella *connectionString* variabile.</span><span class="sxs-lookup"><span data-stu-id="b06ad-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="b06ad-167">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="b06ad-167">Deploy to Azure</span></span>

<span data-ttu-id="b06ad-168">In Esplora soluzioni, espandere il **ruoli** cartella all'interno del progetto ChatService.</span><span class="sxs-lookup"><span data-stu-id="b06ad-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="b06ad-169">Il ruolo SignalRChat di mouse e scegliere **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="b06ad-170">Selezionare il **configurazione** scheda. In **istanze** selezionare 2.</span><span class="sxs-lookup"><span data-stu-id="b06ad-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="b06ad-171">È inoltre possibile impostare le dimensioni della VM su **molto piccolo**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="b06ad-172">Salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="b06ad-172">Save the changes.</span></span>

<span data-ttu-id="b06ad-173">In Esplora soluzioni, fare clic sul progetto ChatService.</span><span class="sxs-lookup"><span data-stu-id="b06ad-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="b06ad-174">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="b06ad-175">Se questa è la prima ora la pubblicazione in Windows Azure, è necessario scaricare le credenziali.</span><span class="sxs-lookup"><span data-stu-id="b06ad-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="b06ad-176">Nel **pubblica** procedura guidata, fare clic su "Accedi scaricare le credenziali".</span><span class="sxs-lookup"><span data-stu-id="b06ad-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="b06ad-177">Verrà chiesto di accedere al portale di Windows Azure e scaricare un file di impostazioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="b06ad-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="b06ad-178">Fare clic su **importazione** e selezionare il file di impostazioni di pubblicazione che è stato scaricato.</span><span class="sxs-lookup"><span data-stu-id="b06ad-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="b06ad-179">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-179">Click **Next**.</span></span> <span data-ttu-id="b06ad-180">Nel **impostazioni di pubblicazione** finestra di dialogo, in **servizio Cloud**, selezionare il servizio cloud creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b06ad-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="b06ad-181">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="b06ad-181">Click **Publish**.</span></span> <span data-ttu-id="b06ad-182">Può richiedere alcuni minuti per distribuire l'applicazione e avviare le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="b06ad-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="b06ad-183">A questo punto quando si esegue l'applicazione di chat, le istanze del ruolo comunicano tramite Bus di servizio di Azure, mediante un argomento del Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="b06ad-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="b06ad-184">Un argomento è una coda di messaggi che consente a più sottoscrittori.</span><span class="sxs-lookup"><span data-stu-id="b06ad-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="b06ad-185">Backplane crea automaticamente gli argomenti e le sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="b06ad-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="b06ad-186">Per visualizzare le sottoscrizioni e attività del messaggio, aprire il portale di Azure, selezionare lo spazio dei nomi del Bus di servizio e fare clic su "Argomenti".</span><span class="sxs-lookup"><span data-stu-id="b06ad-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="b06ad-187">Può richiedere alcuni minuti per l'attività di messaggio da visualizzare nel dashboard.</span><span class="sxs-lookup"><span data-stu-id="b06ad-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="b06ad-188">SignalR gestisce la durata di argomento.</span><span class="sxs-lookup"><span data-stu-id="b06ad-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="b06ad-189">Fino a quando l'applicazione viene distribuita, non tentare di eliminare gli argomenti manualmente o modificare le impostazioni per l'argomento.</span><span class="sxs-lookup"><span data-stu-id="b06ad-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b06ad-190">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="b06ad-190">Troubleshooting</span></span>

<span data-ttu-id="b06ad-191">**System. InvalidOperationException "Solo IsolationLevel supportato è 'IsolationLevel.Serializable'".**</span><span class="sxs-lookup"><span data-stu-id="b06ad-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="b06ad-192">Questo errore può verificarsi se il livello di transazione per un'operazione è impostato su un valore diverso da `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="b06ad-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="b06ad-193">Verificare che nessuna operazione sono viene eseguita con altri livelli delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="b06ad-193">Verify that no operations are being performed with other transaction levels.</span></span>