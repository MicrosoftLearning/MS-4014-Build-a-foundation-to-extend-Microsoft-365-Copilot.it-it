# Lab pratico 1: Configurare le connessioni per il connettore Microsoft Graph

## Tenant WWL: condizioni per l'utilizzo

Se, nell'ambito di una consegna di un corso con istruttore, viene fornito un tenant, tenere presente che questi viene messo a disposizione per supportare i laboratori pratici di tale corso con istruttore.

I tenant non devono essere condivisi o utilizzati per scopi esterni ai laboratori pratici. Il tenant utilizzato in questo corso è un tenant di prova e non può essere utilizzato o reso accessibile dopo il termine della lezione e non è idoneo per l'estensione.

I tenant non devono essere convertiti in un abbonamento a pagamento. I tenant ottenuti nell'ambito di questo corso rimangono di proprietà di Microsoft Corporation, che si riserva il diritto di accedervi e di recuperarli in qualsiasi momento.

## Riepilogo

In questo lab verrà utilizzata un'interfaccia di amministrazione di Microsoft 365 per creare una connessione ai file del cliente mediante il connettore di condivisione file Microsoft.

Si supponga di essere un responsabile del servizio clienti di Contoso, una società di medie dimensioni. Il team ha lottato con i tempi di risposta e l'efficienza complessiva durante la gestione delle richieste dei clienti. Viene deciso di sfruttare l'interfaccia di amministrazione di Microsoft 365 per creare una connessione FileShare che consente agli agenti del servizio clienti di accedere e fare riferimento rapidamente ai file della clientela.

## Esercizio 1: configurare le connessioni nell'interfaccia di amministrazione di Microsoft 365

### Accedere alla macchina virtuale

Il provider di hosting del lab fornisce una password per l'account amministratore MOD, ovvero l'amministratore tenant predefinito. Ai fini della sicurezza, Microsoft ha configurato il tenant di valutazione in modo che tutti gli utenti predefiniti debbano modificare la password al successivo accesso. Accedere a **LON-CL1** come account **amministratore** locale creato dal provider di hosting lab con la password **Pa55w.rd**.

### Attività 1: Concedere le autorizzazioni nel portale di Azure

1. Passare al portale di Azure **https://www.portal.azure.com** e accedere con le credenziali amministrative. Non salvare la password e selezionare **Sì** per **Rimanere connessi?**.
2. Nella schermata **Benvenuto in Microsoft Azure**, selezionare **Annulla**.
1. Selezionare l'icona del menu a discesa sul lato sinistro della schermata per visualizzare il menu Portale. Selezionare **Microsoft Entra ID -> Gestisci -> Registrazioni app**.
1. Nella barra dei menu in alto selezionare **Nuova registrazione**. Viene visualizzata la **pagina Registra un'applicazione**. In questa pagina, specificare un nome per l'app; assegnare all'app il nome **Contoso Fileshare**. Lasciare l'opzione per i tipi di account supportati predefiniti come **Account solo in questa directory dell'organizzazione (Solo Contoso - Tenant singolo).** Non selezionare un ** URI di reindirizzamento** facoltativo.
1. Selezionare **Registra**. L'applicazione viene creata e viene assegnato un ID applicazione. Queste informazioni verranno usate quando si crea l'agente del connettore Graph /GCA) nei passaggi seguenti. Prima di creare il GCA, tuttavia, è necessario configurare le impostazioni necessarie.

Il passaggio successivo consiste nel concedere le autorizzazioni per l'agente del connettore Graph nel portale di Azure:

1. Selezionare l'opzione **Gestisci -> Autorizzazioni API** dal menu a sinistra.
1. Selezionare **Aggiungi un'autorizzazione -> Microsoft Graph -> Autorizzazioni dell'applicazione** e consentire le autorizzazioni per le API seguenti:

    - Directory -> Directory.Read.All
    - ExternalConnection -> ExternalConnection.ReadWrite.OwnedBy
    - ExternalItem -> ExternalItem.ReadWrite.All
      
1. Selezionare il pulsante **Aggiungi autorizzazioni**.
1. Selezionare **Concedi consenso amministratore per Contoso** e confermare selezionando **Sì**.

**Nota:** non chiudere questa scheda del browser Edge. Per le attività seguenti è necessario copiare e incollare le informazioni dal portale di Azure.

### Attività 2 - Installazione di GCA

1. Aprire una nuova scheda del browser Microsoft Edge. Passare all'URL seguente per scaricare l'agente del connettore Graph: **https://www.microsoft.com/en-us/download/details.aspx?id=104045**. Seleziona il pulsante **Scarica**. 
1. Aprire il file **GcaInstaller_3.1.1.0.msi** e seguire le richieste nell'installazione guidata. 
2. Nella barra di **ricerca** nella parte inferiore della schermata immettere la **configurazione dell'agente del connettore Graph** e selezionare l'app dal menu quando viene visualizzata.
3. Consentire all'app di apportare modifiche al dispositivo selezionando **Sì**.
4. Accedere e registrare GCA usando l'account **amministrativo MOD**. Nel browser Edge viene visualizzata una conferma del completamento dell'autenticazione. È possibile chiudere questa finestra.
5. Aprire l'app GCA selezionando l'icona nella parte inferiore della schermata.
1. Assegnare all'agente il nome **ContosoFiles**.
1. Selezionare la scheda di Microsoft Edge del portale di Azure (dovrebbe esserci scritto **Contoso Fileshare - Microsoft Azure**), passare alla schermata **Panoramica** e copiare l'**ID applicazione (client)**. Incollarlo nell'app di installazione GCA.

Successivamente, è necessario impostare un segreto client per questa app nel portale di Azure.

1. Tornare alla scheda Edge del portale di Azure e passare a **Certificati e segreti**.
1. Selezionare **+ Nuovo segreto client**. Aggiungere una **Descrizione** immettendo **Contoso Files**. È possibile lasciare il campo della scadenza impostato sul valore predefinito di 180 giorni.
2. Selezionare **Aggiungi**.
3. Copiare il campo **Valore** del segreto invisibile all'utente.
1. Tornare all'app agente del connettore Graph e incollare il valore nel campo **Password applicazione (segreto client)** dell'app del programma di installazione GCA.
1. Selezionare **Registra**.
1. Al termine della registrazione, chiudere l'app del programma di installazione.

### Attività 3: Scaricare i file di risorse da Github

Per configurare il connettore usando GCA, sono necessari file locali per il sistema. 

1. Aprire un nuovo browser Edge e immettere **https://github.com/MicrosoftLearning/MS-4014-Build-a-foundation-to-extend-Microsoft-365-Copilot/tree/master/ResourceFiles** nella barra degli indirizzi.
2. Selezionare il primo file nella cartella, **Contoso Chai Tea market trends 2023.xls** per aprirlo.
3. Selezionare il pulsante con i **puntini di sospensione (altre azioni relative ai file)** in alto a destra nella schermata, quindi selezionare **Download**.
4. Ripetere il passaggio 3 per ognuno dei file rimanenti.
5. È possibile chiudere questa finestra al termine dei download.
6. Usare Esplora file e passare alla directory C:\. Creare una nuova cartella denominata **ResourceFiles**.
7. Aprire una nuova finestra di Esplora file e passare alla cartella **Download** per accedere ai file Contoso scaricati da GitHub. Copiare questi file nella directory **C:\ResourceFiles**.

Questi file verranno usati come contenuto di origine per la connessione creata nell'interfaccia di amministrazione Microsoft.

### Attività 4: aprire l'interfaccia di amministrazione di Microsoft

1. Aprire una nuova scheda del browser Microsoft Edge. Nel browser Microsoft Edge, passare alla **home page di Microsoft 365** immettendo l'URL seguente nella barra degli indirizzi: **https://portal.office.com**
1. Se viene richiesto di accedere, immettere il **nome utente** e la **password** LON-CL1 forniti dal provider di hosting del lab per il tenant di valutazione di Microsoft 365. Il nome utente deve essere nel formato **<admin@xxxxxZZZZZZ.onmicrosoft.com>**, dove xxxxxZZZZZZ è il prefisso del tenant assegnato dal provider di hosting del lab. 
1. La pagina **Ti diamo il benvenuto in Microsoft 365** viene visualizzata nel browser Microsoft Edge nella scheda **Home | Microsoft 365**. Si tratta della home page di Microsoft 365 dell'amministratore MOD.
1. Selezionare **Amministratore** nell'elenco delle icone dell'applicazione visualizzata nel riquadro di spostamento. Questa azione apre **l'interfaccia di amministrazione di Microsoft 365** in una nuova scheda del browser.
1. Selezionare **... Mostra tutto** per visualizzare il menu di spostamento completo. Selezionare **Impostazioni** -> **Ricerca e intelligence.**
1. Nella scheda **Panoramica** visualizzata, scorrere verso il basso. Selezionare **Aggiungi la prima connessione** nella casella **Connettori**.

Da qui è riportato un elenco delle origini dati disponibili. Si sta configurando una connessione usando il connettore Condivisione file. Questo connettore consente alla ricerca di Microsoft 365 Copilot di fare riferimento ai file della clientela all'interno di una ricerca Copilot e permette tempi di risposta più rapidi e risposte più accurate quando un agente del servizio clienti le sta cercando.

1. Selezionare **Condivisioni file** e quindi selezionare **Avanti**.
1. Immettere il **nome visualizzato**. Il nome visualizzato è un nome univoco visualizzato agli utenti. Immettere **File di Contoso** nel campo.
1. Immettere il **percorso della cartella.** I file di esempio sono configurati nella directory seguente:

   **C:\ResourceFiles**

Il percorso è la directory in cui è archiviato il file.

1. Selezionare la GCA creata nei passaggi precedenti (ContosoFiles) nel campo **Agente connettore Graph** se non è l'opzione predefinita.
1. Selezionare **Windows** come tipo di autenticazione, quindi immettere il **nome utente** e la **password** dell'inrefaccia della riga di comando LON per autenticazione di Windows.
1. Selezionare la casella di controllo per autorizzare Microsoft a creare un indice di dati di terze parti nel tenant.
1. Selezionare **Crea**.  Viene visualizzata una schermata relativa all'esito positivo dell'operazione e la connessione inizia a eseguire la sincronizzazione. È possibile immettere il tipo specifico di contenuto, i reparti dell'organizzazione che possono trarre vantaggio dal suo utilizzo o esempi di flussi di lavoro nella casella **Descrizione connettore**. Ad esempio:

    **Questo connettore contiene informazioni contenute nel server di condivisione file locale. Include i profili dei clienti e le domande dei clienti che possono trarre vantaggio dal supporto tecnico per le risposte alle query dei clienti.**
1. Selezionare **Fatto**. La connessione viene ora visualizzata nella scheda **Ricerca e intelligence** dell'interfaccia di amministrazione di Microsoft 365 e vi si può fare riferimento nei risultati della ricerca o quando si crea un agente di Microsoft 365 Copilot.
