2026-04-05 14:24

Status:  #new

Tags: [[Tags/testing|testing]]
# Testing 

La fase di testing è fondamentale in ogni disciplina ingegneristica per validare il prodotto intermedio e finale dello sviluppo di un progetto.
Il testing viene fatto sia su prodotti sviluppati in serie, sia per prodotti unici nel loro genere, come appunto i software. 
Se nei prodotti di massa esiste di solito una suite di test da effettuare per determinare la qualità di un prodotto, per gli sviluppi singoli sono utilizzati test specializzati per il prodotto. 

> Quando test generici non forniscono abbastanza conferme riguardo la validità di un elaborato, si ricorre a test specialistici. Questo vale per tutte le discipline ingegneristiche.
## Testing nell' ingegneria del software

La costruzione di software è una disciplina ingegneristica e, come tale, ha la necessità di test. La minuziosità ed il livello di attenzione necessari per scrivere **buoni test** è molto elevata vista l'intrinseca non-linearità della correttezza del codice: 

> Se una procedura funziona correttamente con 256 elementi, nessuno mi assicura che questa funzionerà con 255, 57, 0, -1, eccetera.

E' per questo motivo che il tempo occupato alla definizione ed all'implementazione di una suite di test efficace possono normalmente superare i tempi allocati per la costruzione del software da testare. 

### Tipologie

Il processo di definizione di test è molto ampio e variegato. Esistono molte tipologie diverse di test che possono essere implementate, ognuna di queste importante a modo suo per poter consegnare un software affidabile. Abbiamo ad esempio:
- Unit testing
- Integration testing
- System testing (o *end-to-end* testing)
- Alpha & Beta testing
#### Unit test

Gli **unit test** si occupano di verificare la correttezza di una unità atomica di un software. Cosa si intende per "unità atomica"? Ad esempio in un linguaggio orientato agli oggetti potrebbe essere una classe, o ancora più nel dettaglio, un singolo metodo. 
Gli unit test sono di norma **automatizzati** e sono implementati con l'ausilio di apposite librerie che velocizzano la fase di scaffolding e di creazione degli oracoli.

Gli unit test funzionano egregiamente come **regression test**, dal momento che la scrittura di una suite esaustiva (ma non troppo!) di casi di test per una unità di software permette a seguito di modifiche a quest'ultima di verificare la correttezza del software aggiornato rispetto ai test esistenti.

Le unità software da testare sono solitamente accoppiate ad altre componenti; per fare in modo che gli unit test si occupino solo di verificare la correttezza *della classe* e non delle sue dipendenze, si creano **oggetti di mock** che implementano la stessa API delle dipendenze (es. classe DAO per accesso a database) e per cui la logica dei metodi è configurabile nella fase di scaffolding per testare il funzionamento dell'unità sotto *quei valori restituiti* dalle dipendenze utilizzate.

> La collaborazione "reale" tra le diverse componenti software sarà testato negli integration test e system test.
#### Integration test

L'obiettivo dichiarato dei **test d'integrazione** è quello di assicurarsi che le parti e i sistemi di cui è composto il software lavorino in maniera corretta ed affidabile.
Non è necessario effettuare il mock delle dipendenze in questa fase, dal momento che si usano i moduli reali, le implementazioni realistiche.
#### System test 

I **test di sistema** (o *end-to-end test*) verificano la correttezza e validità dell'intero software. E' il test di livello più elevato e quello più complesso da implementare. I casi di test di questo livello devono essere costruiti con la prerogativa che **le singole componenti e le loro comunicazioni funzionino come richiesto**; è per questo motivo che i test E2E sono di norma effettuati dopo gli unit ed gli integration test
#### Acceptance testing

I **test di accettazione** hanno come obiettivo quello di verificare la correttezza del sistema a livello funzionale e non, ovvero di verifica dell'implementazione dei requisiti definiti in fase di analisi e progettazione.
Una forma di acceptance testing è l'**alpha testing**, effettuato dagli sviluppatori stessi per verificare nel normale utilizzo del software da loro progettato eventuali bug. 
Dopo l'Alpha testing si procede con il **beta testing**: il software viene dato in mano ad un numero ristretto di reali utilizzatori del software, anche in questo caso per verificare la validità del software.
## I casi di test

La fase di testing in tutte le sue forme include una parte di definizione dei **casi di test**. Questi sono identificati dai seguenti elementi:
- *Input*: non solamente gli effettivi dati in ingresso di una procedura, ma tutto ciò che contribuisce a determinare il comportamento della procedura.
- *Criteri di successo/fallimento*: definiscono l'**oracolo** del caso di test. Ci permette di definire i criteri secondo cui una componente software si comporta nella maniera corretta.

> I casi di test sono generati partendo dalle **specifiche di test**. Queste specifiche definiscono input, condizioni di successo e, se necessario, comportamenti peculiari da approfondire con casi di test specializzati.

I casi di test generati dalla medesima specifica di test o riguardanti la stessa componente software sono organizzati in **suite di test**.
#### Black-box testing

La specifica di test può essere organizzata come macchina a stati finiti, e come tale è possibile testarla in maniera approfondita ed efficace creando casi di test per ogni *cammino* all'interno della macchina.
Questa forma di testing è detta **testing funzionale** (o **black-box testing**). 
#### White-box testing

Al contrario, la scrittura di una suite di test basata sulla struttura del componente software da testare è detta **testing strutturale** (o **white-box testing**).

#### Fault-based testing

Partendo da conoscenze pregresse e da errori del passato è possibile creare suite di test che verificano la correttezza del componente software in cui sono state iniettate volontariamente 
delle falle di sicurezza o dei semplici bachi per vedere se questi problemi sono individuati correttamente.
Questa forma di testing, detta **fault-based testing**, si basa sulla teoria che un codice robusto capace di individuare degli errori sia in grado di individuarne altri non considerati in fase di test.

### Criteri di adeguatezza

Diciamo che una suite di test soddisfa i suoi **criteri di adeguatezza** se ogni criterio è testato e soddisfatto da almeno un caso di test. 

> Un possibile esempio nei test strutturali riguarda lo **statement coverage**, dove si richiede che ogni statement (riga di codice) eseguibile sia eseguita da almeno un caso di test.

E' importante ribadire che i criteri di adeguatezza devono essere necessariamente trattati come **linee guida** e non come regole rigide da seguire, dal momento che molti di questi criteri si dimostrano essere insoddisfacibili nell'ambito della implementazione concreta dei requisiti; al ché è molto più importante misurare il grado (solitamente in percentuale) di quanto una suite di test si avvicina a soddisfare un determinato criterio di adeguatezza.

> Seguendo il caso presentato sopra, in caso di presenza di statement non eseguibili è possibile accontentarsi di avere uno statement coverage superiore ad una certa percentuale (es. 90%).

