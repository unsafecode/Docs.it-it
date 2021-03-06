---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Supporto di azioni OData in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: 'In OData, le azioni sono un modo per aggiungere comportamenti sul lato server che non possono essere definiti facilmente come operazioni CRUD su entità. Alcuni tipi di utilizzo per le azioni includono: implementare...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Supporto di azioni OData in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> In OData, *azioni* rappresentano un modo per aggiungere comportamenti sul lato server che non possono essere definiti facilmente come operazioni CRUD su entità. Alcuni tipi di utilizzo per le azioni includono:
> 
> - Implementare le transazioni complesse.
> - Modifica di alcune entità in una sola volta.
> - Consentire aggiornamenti solo a determinate proprietà di un'entità.
> - L'invio di informazioni al server che non è definito in un'entità.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Web API 2
> - OData versione 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Esempio: Valutazione di un prodotto

In questo esempio, si vuole consentire agli utenti di valutare i prodotti e quindi esporre le valutazioni medie per ogni prodotto. Il database, verrà archiviato un elenco di classificazioni, con chiave fornita per i prodotti.

Di seguito è riportato il modello è possibile che venga utilizzata per rappresentare le classificazioni di Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Ma non si desidera che i client registra un `ProductRating` oggetto da una raccolta di "Restrizioni". In modo intuitivo, la classificazione è associata alla raccolta di prodotti e il client è necessario solo registrare il valore delle classificazioni.

Pertanto, anziché utilizzare le normali operazioni CRUD, è possibile definire un'azione che un client può essere richiamato su un prodotto. Nella terminologia di OData, l'azione è *associato* alle entità del prodotto.

>Azioni hanno effetti collaterali sul server. Per questo motivo, vengono richiamati tramite richieste HTTP POST. Azioni possono avere parametri e tipi restituiti, che sono descritti nei metadati del servizio. Il client invia i parametri nel corpo della richiesta e il server invia il valore restituito nel corpo della risposta. Per richiamare l'azione "Frequenza Product", il client invia un POST a un URI simile al seguente:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

I dati nella richiesta POST sono semplicemente la valutazione del prodotto:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Dichiarare l'azione in Entity Data Model

Nella configurazione dell'API Web, aggiungere l'azione a entity data model (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Questo codice definisce "RateProduct" come un'azione che può essere eseguita su entità Product. Dichiara che l'azione che utilizza un **int** parametro denominato "Livello" e restituisce un **int** valore.

## <a name="add-the-action-to-the-controller"></a>Aggiungere l'azione nel controller

L'azione "RateProduct" è associato a entità Product. Per implementare l'azione, aggiungere un metodo denominato `RateProduct` al controller di prodotti:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Si noti che il nome del metodo corrisponde al nome dell'azione in EDM. Il metodo presenta due parametri:

- *chiave*: la chiave per il prodotto alla velocità.
- *i parametri*: un dizionario dei valori di parametro di azione.

Se si utilizza le convenzioni di routing predefinita, il parametro della chiave deve essere denominato "chiave". È anche importante includere il **[FromOdataUri]** attributo, come illustrato. Questo attributo indica l'API Web per utilizzare le regole di sintassi di OData quando analizza la chiave dall'URI della richiesta.

Utilizzare il *parametri* dizionario per recuperare i parametri di azione:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Se il client invia i parametri dell'azione in corrette di formato, il valore di **ModelState.IsValid** è true. In tal caso, è possibile utilizzare il **ODataActionParameters** dizionario per ottenere i valori dei parametri. In questo esempio, il `RateProduct` azione accetta un singolo parametro denominato "Livello".

## <a name="action-metadata"></a>Metadati di azione

Per visualizzare i metadati del servizio, inviare una richiesta GET a /odata/$ metadata. Di seguito è la parte dei metadati che dichiara il `RateProduct` azione:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

Il **FunctionImport** elemento dichiara l'azione. La maggior parte dei campi sono di chiara interpretazione, ma non degni di nota sono due:

- **È IsBindable** significa che l'azione può essere richiamato almeno su entità di destinazione, parte del tempo.
- **IsAlwaysBindable** significa che l'azione può sempre essere richiamato su entità di destinazione.

La differenza è che alcune azioni sono sempre disponibili ai client, ma altre azioni potrebbero dipendono dallo stato dell'entità. Ad esempio, si supponga di che definisce un'azione "Acquisto". È possibile acquistare solo un elemento che è disponibile in magazzino. Se l'elemento è esaurito, un client non può richiamare tale azione.

Quando si definisce EDM, il **azione** metodo crea un'azione sempre associabile:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Parlerò azioni non sempre-associabile (detto anche *temporanei* azioni) più avanti in questo argomento.

## <a name="invoking-the-action"></a>Richiamare l'azione

Ora di seguito viene illustrato come un client richiama l'azione. Si supponga che il client desidera assegnare una classificazione pari a 2 per il prodotto con ID = 4. Di seguito è riportato un messaggio di richiesta di esempio, nel formato JSON per il corpo della richiesta:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Di seguito è riportato il messaggio di risposta:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Associazione di un'azione a un Set di entità

Nell'esempio precedente, l'azione è associata a una singola entità: velocità di trasferimento dei client di un singolo prodotto. È anche possibile associare un'azione a una raccolta di entità. Consente di eseguire solo le modifiche seguenti:

In EDM, aggiungere l'azione per l'entità **raccolta** proprietà.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

Nel metodo del controller, omettere il *chiave* parametro.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

A questo punto il client richiama l'azione sul set di entità Products:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Operazioni con i parametri della raccolta

Azioni possono avere parametri che accettano una raccolta di valori. In EDM, utilizzare **CollectionParameter&lt;T&gt;**  per dichiarare il parametro.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Si dichiara un parametro denominato "Restrizioni" che accetta una raccolta di **int** valori. Nel metodo del controller, è comunque ottenere il valore di parametro di **ODataActionParameters** oggetto, ma ora il valore è un **ICollection&lt;int&gt;**  valore:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Azioni temporanee

Nell'esempio "RateProduct", gli utenti possono sempre frequenza un prodotto, pertanto l'azione è sempre disponibile. Tuttavia, alcune azioni dipendono dallo stato dell'entità. Ad esempio, in un servizio di noleggio di video, l'azione "Checkpoint" non è sempre disponibile. (A seconda se è disponibile una copia di tale video.) Questo tipo di azione è definito un *temporanei* azione.

Nei metadati del servizio, è un'azione temporanea **IsAlwaysBindable** uguale a false. Che rappresenta il valore predefinito, i metadati avrà un aspetto simile al seguente:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Ecco perché questo è importante: se un'azione è temporanea, il server è necessario indicare al client quando l'azione è disponibile. Ciò avviene inserendo un collegamento all'azione nell'entità. Di seguito è riportato un esempio per un'entità filmato:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

La proprietà "#CheckOut" contiene un collegamento all'azione di estrazione. Se l'azione non è disponibile, il server viene omesso il collegamento.

Per dichiarare un'azione temporanea in EDM, chiamare il **TransientAction** metodo:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Inoltre, è necessario fornire una funzione che restituisce un collegamento all'azione per una determinata entità. Impostare questa funzione chiamando **HasActionLink**. È possibile scrivere la funzione come un'espressione lambda:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Se l'azione è disponibile, l'espressione lambda restituisce un collegamento all'azione. Il serializzatore OData include questo collegamento durante la serializzazione dell'entità. Quando l'azione non è disponibile, la funzione restituisce `null`.

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di azioni OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
