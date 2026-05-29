2026-04-11 11:21

Status:  #new

Tags: [[Tags/testing|testing]] 

# Testing strutturale

Il **testing strutturale** è una forma di testing basata sulla verifica della correttezza del programma inteso come implementazione di una specifica. Questa forma di testing, detta *white-box* testing, svolge un ruolo complementare con il testing funzionale (il *black-box testing*, perché "non ci vedi dentro") per verificare che i criteri di adeguatezza definiti siano rispettati.

Il testing strutturale va ancora più in profondità nella ricerca di falle nella progettazione o nell'implementazione nella specifica; andando a testare il flusso di esecuzione del programma, riesce ad arrivare dove il testing funzionale non può mettere piede.

> E' bene ribadire che il testing funzionale e quello strutturale sono sinergici, nel senso che servono entrambi per avere una suite di test affidabile.

I test strutturali testano codice, e questa è una buonissima notizia: un qualsiasi linguaggio di programmazione è un linguaggio formale, con regole di sintassi, semantiche e lessicografiche note, e questo permette il supporto da parte di automatizzati durante il testing. 

## Statement testing

Uno dei criteri standard di adeguatezza del test strutturale riguarda lo **statement coverage**, ovvero la copertura delle istruzioni all'interno del programma: ogni istruzione deve essere "toccata" dal almeno un caso di test, altrimenti potrebbe essere portatrice di bachi.

> La copertura degli statement non è ovviamente una garanzia della correttezza della procedura. D'altra parte, se uno statement non è toccato, non vi è alcuna conferma riguardo la sua correttezza.

Lo *statement coverage* viene misurato con una formula semplice:
$$
C_{statement} = \frac{\text{no. executed statements}}{\text{no. statements}}
$$

## Branch testing

Il coverage degli statement non è il solo obiettivo dei test strutturali. Esistono infatti casistiche dove tutti gli statement sono coperti ma esistono alcuni rami nel flusso di esecuzione del programma non coperti. 
Questo è un bel problema: alcuni statement "coperti" potrebbero essere corretti in determinate circostanze, mentre in altre no, ed il criterio di **copertura dei branch** ci rassicura sul fatto che tutti i rami del grafo di esecuzione siano stati toccati da almeno un caso di test.

Il *branch coverage* viene misurato come segue:
$$
C_{branch} = \frac{\text{no. executed branches}}{no. branches}
$$
> Il branch coverage è strettamente legato allo statement coverage:  provando tutti i possibili cammini implicitamente si stanno provando tutti gli statement.

## Condition testing

Si può ancora migliorare. Il criterio di adeguatezza **condition coverage** afferma che **tutte le espressioni logiche di base** devono essere valutate sia `True` che `False` in due casi di test. 

Il condition logic è importante nei programmi in cui è presente una grande quantità di costrutti `if` con condizioni composte. 
In questi casi il *branch coverage* non basta più, dal momento che un solo predicato potrebbe con il suo valore di verità decidere l'esito della condizione, andando di fatto a lasciare scoperti i casi in cui invece siano gli altri predicati ad avere voce in giudizio.

> Nel caso di condizionali semplici, condition coverage e branch coverage coincidono.

Ad esempio per l'espressione
```
((a || b) && c) || d) && e
```
I casi di test sarebbero i seguenti: 

![[condition-coverage-naive.png]]

### Modified condition testing

Il condition testing ha una problematica di fondo: in programmi progettati in un certo modo (male!), nel caso peggiore la valutazione di tutti i possibili predicati di verità negli `if` può risultare esponenziale nel numero di casi di test.

Un' alternativa è **MC/DC**, o *modified condition adequacy criterion*, un criterio di adeguatezza che limita la valutazione a solamente quei predicati che dimostrano alterare la valutazione dell'intera condizione composta.

Con il criterio MC/DC, l'espressione booleana di cui sopra produce un numero minore di casi di test:

![[mc-dc-testing.png]]

## Path testing

Infine, a volte un problema con un branch, statement o condizione booleana esce fuori solo a seguito di una serie ben precisa di statement eseguiti o branch intrapresi; al ché risulta necessario in ultima battuta determinare un criterio di adeguatezza legato al **path coverage**, ovvero l'esistenza di almeno un caso di test che esegue un cammino prefissato.

Il path coverage si calcola come lo statement, il branch ed il condition coverage, ovvero con un semplice rapporto:

$$
C_{Path} = \frac{\text{no. executed paths}}{\text{no. paths}}
$$
> Vi è un piccolo accorgimento da fare: nel caso di programmi con loop, il numero di cammini tende ad infinito, il ché rende il coverage in questi programmi sempre pari a 0, a prescindere dal numero di cammini eseguiti.

Una soluzione utile a questo problema è di partizionare i cammini possibili, così da doverne provare solo uno per tipo. 

> [!important]
> Da rivedere l'ultima parte, dal *condition coverage* fino a questo punto.

