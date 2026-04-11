2026-04-07 23:22

Status:  #new

Tags: [[Tags/testing|testing]] 

# Test combinatorio

Il **testing combinatorio** nei test funzionali consiste nella trasformazione delle specifiche in insiemi di proprietà ed attributi i cui valori possono essere assegnati in maniera automatizzata per produrre **combinazioni di casistiche**.

## Tecniche

Sono presenti diverse euristiche utili da eseguire per derivare efficacemente i casi di test partendo da specifiche. Di seguito una breve carrellata delle più importanti:
### Category-partition

Il **category-partition** testing è un metodo per generare test funzionali partendo da specifiche informali molto utilizzato. Si può sommariamente suddividere in tre fasi:
1. Decomposizione delle specifiche in componenti **funzionali** testabili in maniera indipendente.
2. Identificazione valori significativi dei parametri che influenzano l'esecuzione della funzionalità in analisi. 
3. Generazione casi di test e scrematura di questi ultimi tenendo conto di vincoli semantici e combinazioni invalide.

#### Scelta di valori significativi

La scelta dei valori significativi è eseguita applicando una serie di regole comuni, come il *boundary value* ed il *erroneous condition*, rispettivamente valori al limite del dominio del parametro e la scelta di errori volontariamente errati per testare la gestione degli errori del software.

> La scelta delle classi significative di valori deve essere condotta in maniera indipendente per ogni input, trascurando per ora le relazioni tra di essi. 

Questa operazione è tediosa e prona ad errori umani nella determinazione dei casi limite; è possibile automatizzare parzialmente questa fase utilizzando i **cataloghi**, che permettono una individuazione sistematica delle classi di input normalmente significative.
### Pairwise & k-way

Grazie alla tecnica del category partition è possibile ottenere un numero esaustivo di casi di test per soddisfare tutti i criteri di adeguatezza del caso. Alle volte sono *fin troppi* i casi di test generati, anche a seguito della scrematura di casi di test invalidi o errati.
E' qui che entrano in gioco il **pairwise testing** e l'**k-way testing**, due euristiche atte a 
ridurre in maniera talvolta drastica il numero di casi di test individuati. 

> Come funzionano? I casi di test vengono generati per provare solamente **interazioni tra coppie, terne, o in generale insiemi di k < n elementi** [^1].

### Catalogs

Come già anticipato prima, la fase di individuazione delle classi rappresentative di tutti gli input della funzionalità è lunga e richiede molta attenzione, caratteristiche che la rendono molto suscettibile ad errori umani.
E' possibile rendere questo processo molto più sistematico raccogliendo l'esperienza fatta creando casi di test e organizzandola in modo tale da associare per ogni tipo di input un **catalogo** di casi che sarebbe (solitamente) buona prassi testare.

Ad esempio per un input intero limitato inferiormente e superiormente un catalogo permetterebbe di individuare a costo zero queste classi:
- L'elemento precedente al limite inferiore
- Il limite inferiore
- Un numero intero all'intervallo
- Il limite superiore
- L'elemento subito dopo il limite superiore

Il catalogo di cui sopra, così com'è strutturato, permette sia di provare il sistema di gestione di input semanticamente errati, sia di provare che in casi limite il sistema funzioni come previsto. 
#### Analisi specifica

Per definire un catalogo si deve suddividere la specifica in un insieme di oggetti elementari, ognuno dei quali appartiene ad una di queste categorie:
- Pre-condizioni
- Post-condizioni
- Operazioni
- Variabili
- Definizioni

Le **pre-condizioni** definiscono tutto ciò che deve essere vero affinché la funzionalità possa essere eseguita in un contesto corretto. Se anche solo una delle pre-condizioni non è rispettata, allora la funzionalità risulta instabile ed è molto probabile che provochi un errore.
Le pre-condizioni si possono dividere a grandi linee in due categorie:
1. Validate: devono essere controllate dal SUT
2. Assunte: si assume siano state controllate dal chiamante

> Come vedremo, le pre-condizioni che richiedono maggiore attenzione sono quelle validate

Le **post-condizioni** riguardano tutti i possibili esiti del SUT; qualora ci siano molteplici risultati è necessario elencarli tutti in una unica gigantesca post-condizione o è possibile creare un insieme di elementi che catturano solo un caso a testa.

Il collante tra pre e post-condizioni sono le **operazioni**, che aiutano a descrivere il comportamento del SUT e forniscono supporto nella costruzione dei casi di test. Differiscono dalle definizioni dal momento che queste ultime sono legate alle variabili, quindi allo **stato del sistema**, mentre le operazioni sono riferite al **comportamento** del SUT.

> Come si procede poi? In che modo queste informazioni mi sono d'aiuto nella definizione dei casi di test?

#### Generazioni specifiche di test

Il prossimo step naturale è, ovviamente, quello di **generare specifiche dei casi di test** partendo da queste informazioni.
Per le pre-condizioni  **validate**  modo canonico di procedere è quello di creare un caso di test nel caso in cui questa è rispettata ed il caso errato dove accade il contrario.  Non viene derivato nessun caso di test per controllare la validità di pre-condizioni assunte, dal momento che non vi è un comportamento definito nel caso in cui questa non sia soddisfatta.

Vengono poi generati due casi di test per ogni post-condizione: uno in cui questa è verificata e un altro quando non è così. 

Per le definizioni delle variabili si procede nel seguente modo: nel momento in cui una definizione è presentata con dei condizionali, per la variabile viene generato un caso di test per quando rispetta la definizione ed un altro dove invece avviene il contrario.

> Nel momento in cui un caso di test ne contiene un altro definito, quest'ultimo diventa ridondante e può essere eliminato [^2] .

#### Integrazione con catalogo

Dopo aver individuato tutte le specifiche di casi di test utili è tempo di coinvolgere il catalogo dei casi di test standard per le variabili e le operazioni individuate nella specifica.

> Per ogni elemento viene consultato il catalogo relativo (se esiste), e vengono creati nuovi casi di test. Come per lo step precedente, casi di test ridondanti possono essere eliminati senza problemi.

Ecco alcuni esempi di catalogo:
- *Variabile Booleana*: caso di test quando questa assume valore di verità vero ed uno quando è falso.
- *Collezione di elementi*: proviamo i seguenti valori con casi di test ad-hoc 
	- Vuota
	- Singolo elemento
	- Più di un elemento
	- Valore massimo di elementi (se la collezione ha capienza massima)
	- Più lunga del numero massimo di elementi (sempre se la collezione è limitata)
	- Non terminata correttamente (es. una stringa senza `\0`)
- *Oggetto preso da una collezione*. Cosa provo:
	- Ogni elemento
	- Elemento al di fuori della collezione


Per le variabili i cataloghi possono essere applicati sia quando queste sono variabili di input, sia quando sono di output. 

> I casi di valori errati sono altresì testati solo per le variabili di input, dal momento che è di norma impossibile generate valori errati per le variabili di output.



[^1]: Se il numero di casi di test cresce in maniera esponenziale quando si provano tutte le combinazioni, lo stesso numero per provare insiemi di *k-elementi* cresce in maniera logaritmica.

[^2]: Ad esempio, se una specifica prova il comportamento di un sistema quando un array non è vuoto, ed un altro caso di test prova che il sistema funzioni correttamente quando il medesimo array contiene un numero di elementi dispari, è chiaro che secondo caso di test ingloba il primo, e quest'ultimo può essere eliminato senza problemi.
