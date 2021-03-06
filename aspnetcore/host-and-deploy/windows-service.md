---
title: Ospitare ASP.NET Core in un servizio Windows
author: rick-anderson
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153529"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Ospitare ASP.NET Core in un servizio Windows

[Tom Dykstra](https://github.com/tdykstra)

Il modo consigliato per ospitare un'app ASP.NET Core in Windows senza usare IIS è eseguirla in un [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quando è ospitata come servizio Windows, l'app può essere avviata automaticamente dopo riavvii e arresti anomali senza alcun intervento manuale.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)). Per istruzioni su come eseguire l'app di esempio, vedere il file *README.md* dell'esempio.

## <a name="prerequisites"></a>Prerequisiti

* L'app deve essere eseguita nel runtime di .NET Framework. Nel file *.csproj* specificare i valori appropriati per [TargetFramework](/nuget/schema/target-frameworks) e [RuntimeIdentifier](/dotnet/articles/core/rid-catalog). Di seguito è riportato un esempio:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Quando si crea un progetto in Visual Studio, usare il modello **Applicazione di ASP.NET Core (.NET Framework)**.

* Se l'app riceve richieste da Internet (non solo da una rete interna), è necessario usare il server Web [HTTP.sys](xref:fundamentals/servers/httpsys) (precedentemente noto come [WebListener](xref:fundamentals/servers/weblistener) per le app ASP.NET Core 1. x) anziché [Kestrel](xref:fundamentals/servers/kestrel). IIS è consigliato per l'uso come server proxy inverso con Kestrel per le distribuzioni perimetrali. Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).

## <a name="get-started"></a>Introduzione

In questa sezione vengono illustrate le modifiche minime necessarie per configurare un progetto ASP.NET Core esistente per l'esecuzione in un servizio.

1. Installare il pacchetto NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Modificare `Program.Main` nel modo seguente:

   * Chiamare `host.RunAsService` anziché `host.Run`.

   * Se il codice chiama `UseContentRoot`, usare il percorso di pubblicazione anziché `Directory.GetCurrentDirectory()`.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. Pubblicare l'app in una cartella. Usare [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) che esegue la pubblicazione in una cartella.

4. Eseguire il test creando e avviando il servizio.

   Aprire una shell dei comandi con privilegi di amministratore per usare lo strumento da riga di comando [sc.exe](https://technet.microsoft.com/library/bb490995) per creare e avviare un servizio. Se il servizio è denominato MyService, pubblicato in `c:\svc` e denominato AspNetCoreService, i comandi sono:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   Il valore `binPath` rappresenta il percorso dell'eseguibile dell'app, che include il nome del file eseguibile.

   ![Esempio di creazione e avvio nella finestra della console](windows-service/_static/create-start.png)

   Al termine di questi comandi, passare allo stesso percorso usato per l'esecuzione di un'app console (per impostazione predefinita, `http://localhost:5000`):

   ![Esecuzione in un servizio](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Fornire un modo per l'esecuzione all'esterno di un servizio

È più semplice testare ed eseguire il debug durante l'esecuzione all'esterno di un servizio, pertanto può essere utile aggiungere codice che chiama `RunAsService` solo in determinate condizioni. Ad esempio, l'app può essere eseguita come un'app console con un argomento della riga di comando `--console` o se il debugger è collegato:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>Gestire gli eventi di arresto e avvio

Per gestire gli eventi `OnStarting`, `OnStarted` e `OnStopping`, apportare le modifiche aggiuntive seguenti:

1. Creare una classe che deriva da `WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Creare un metodo di estensione per `IWebHost` che passa l'oggetto `WebHostService` personalizzato a `ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. In `Program.Main` chiamare il nuovo metodo di estensione, `RunAsCustomService`, anziché `RunAsService`:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

Se il codice `WebHostService` personalizzato richiede un servizio dall'inserimento di dipendenze (ad esempio un logger), ottenerlo dalla proprietà `Services` di `IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenari con server proxy e servizi di bilanciamento del carico

I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva. Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Ringraziamenti

In questo articolo è stato scritto con l'aiuto delle fonti pubblicate seguenti:

* [Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074) (Hosting di ASP.NET Core come servizio Windows)
* [How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/) (Come ospitare ASP.NET Core in un servizio Windows)
