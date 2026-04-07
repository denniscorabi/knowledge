# Testing funzionale

Il **testing funzionale** prende il suo nome dalla fonte di ispirazione per la costruzione di questi test: le specifiche funzionali. Prende anche il nome di **black-box testing** poiché si effettua la verifica della correttezza di un software "a scatola chiusa", ovvero senza avere sottomano una implementazione specifica del prodotto.

I test funzionali sono definiti a partire dalla fase di analisi delle specifiche e sono tanto importanti quando i **test strutturali** dal momento che una loro definizione completa può far portare alla luce delle problematiche invisibili ad altre forme di verifica del software, compresi i test strutturali.

## Partizionamento

Alla base del testing funzionale vi è il concetto di **partizionamento** dello spazio dei valori in input ([^1])  in classi non necessariamente disgiunte ma che producono comportamenti verosimilmente identici quando dati in pasto al programma.

Il partizionamento si rende necessario quando lo spazio virtuale di tutti gli input da testare è troppo grande ([^2]), al ché risulta necessario raggruppare valori in input che (dovrebbero) comportarsi in maniera identica e creare un caso di test solo per uno di questi, che svolge il ruolo di "rappresentante" dell'intera classe di input.

> Il partizionamento viene effettuato proprio sulla base delle specifiche di test e dei requisiti funzionali e non.

>[!Important]
>La tecnica di dividere i pressoché infiniti casi di test possibili in un insieme finito di classi, e per ognuno di essere scrivere uno o più finiti casi di test, è detta **partition testing**.

E' bene ricordare però che nella fase di partizionamento dello spazio degli input è normale (se non inevitabile) compiere errori di giudizio, ovvero racchiudere due casi di test nella stessa classe quando però questi provocano due comportamenti diversi del sistema, il ché nella peggiore delle ipotesi si traduce in un test corretto ed uno errato.

> Ed è proprio in questi casi che vi è la possibilità di trattare erroneamente una classe come "corretta", ovvero nel momento in cui si sceglie come rappresentante di quest'ultima il caso di test corretto e si è escluso quello errato.

## Sampling

Alle volte non è possibile dare un maggior peso a certi input che ad altri, quindi la tecnica di partizionamento risulta essere inefficiente e rischia di tralasciare dei casi di input invece molto significativi. 
Si può quindi ricorrere alla scelta di casi di test in maniera randomica, che risulta essere alle volte molto, molto efficiente. 

> Ad esempio, una procedura per dividere una stringa in sequenze da X caratteri può essere testata contro milioni di casi di test generati a random durante la notte, piuttosto che occupare tempo a partizionare lo spazio di input e creare 1-2 test per classe.

Molto spesso però i **casi limite** di una procedura sono una frazione infinitesimale dello spazio di input, ed è quindi molto probabile che vengano generati casualmente. E' perciò prassi comune quella di comunque fare un partizionamento degli input in classi, ed in seguito generare randomicamente i casi di test in modo tale che ogni classe sia almeno trattata da uno di essi.

## Definizione casi di test

L'individuazione dei casi di test è una operazione che richiede molta attenzione: le specifiche possono essere lunghe e intrecciate tra di loro, ed individuare la tecnica di test più adeguata non è sempre un lavoro immediato.
Per questo motivo un **approccio sistematico** alla fase di individuazione dei casi di test è fondamentale; ecco qui alcuni dei punti principali:
### Decomposizione della specifica

Decomporre la specifica in feature che è possibile testare in maniera indipendente è uno dei primi passi fondamentali per individuare in maniera sistematica i casi di test importanti. 
Si effettua a questo livello un **partizionamento funzionale** dell'applicativo, in base a cosa può essere percepito dall'utente del SUT (*system under test*) come un modulo unico.
### Scelta dei rappresentanti degli input

Nelle specifiche è anche importante ricercare riferimenti a valori significativi per i vari input del sistema. A questo livello non deve essere ancora fatta la combinazione di questi valori per ottenere i casi di test (che dovrà essere fatta in seguito).

> Molto spesso basta individuare classi di input, piuttosto che valori specifici. Ad esempio un caso limite può essere quello di *lista con un solo elemento*, piuttosto che una lista con un valore specifico.

### Selezione dei casi di test

A seguito della decomposizione della specifica e dell'individuazione dei rappresentati degli input, è necessario scrivere i casi di test. D'altro canto creare un caso di test per ogni combinazione delle classi di input del sistema può rivelarsi impraticabile e talvolta errato.

> Bisogna quindi **filtrare i possibili i casi di test**, tenendo buoni solamente quelli realmente fattibili.

Un'altra strategia per ridurre i casi di test mantenendo alto il livello di affidabilità della suite di test è quello di testare a coppie gli input. Questa tecnica funziona molto bene quando input diversi sono utilizzati assieme nel programma e il valore di uno influenza la valutazione del valore dell'altro. 

Combinando queste due ed altre tecniche di scrematura dei casi di test è possibile ottenere una suite di test con un alto coefficiente di affidabilità ed al tempo stesso rapida da eseguire e di facile comprensione e mantenimento.

[^1]: Per input in questo caso ci riferisce a tutto ciò che determina il comportamento di un componente software, non strettamente gli argomenti in ingresso di un metodo.

[^2]: Si pensi che in una procedura che accetta due interi a 32 bit come argomenti si hanno circa $10^{54}$ input validi; ovviamente non è possibile testarli tutti...
