[[mobile development]]
# Programmazione di dispositivi mobili

## Caratteristiche
### Idea & modello di business

Una grande idea deve essere coltivata con una applicazione innovativa, usabile e che *rimanga* nel tempo. 
Ad esempio, è importante tener conto dei **costi**; creare una applicazione è un costo continuo, comprensivo di:
- Costi diretti: sviluppo, backend, provisioning infrastruttura
- Costi indiretti: aggiornamenti, design UI/UX, testing

Sicuramente non aiuta lo sviluppo per più piattaforme....l'infrastruttura rimane comunque la stessa.
#### Modelli di profitto

>[!important] 
>Il modo con cui si intende guadagnare attraverso un'app deve essere chiaro fin da subito, dal momento che ciò può impattare l'infrastruttura e l'utilizzo dell'applicazione.

Ai costi dovrebbero far fronte i **guadagni**; la fonte principale di guadagni dipende fortemente dal modello di business applicato. Ad esempio è possibile:
- In-app revenue
	- In-app purchase (paga l'utente)
	- Ads (paga l'inserzionista)
- App a pagamento
	- Subscription
	- Freemium
	- Pagamento upfront

Il modello di app a pagamento dà una percezione di maggior valore e qualità all'applicazione; bisogna comunque stare attenti a scegliere un prezzo competitivo ed offrire se possibile una versione 'lite' gratuita con funzionalità ridotte.

>[!warning]
>Bisogna essere molto accorti sul modo con cui si guadagna attraverso una applicazione mobile. L'obiettivo di massima è quello di **massimizzare i guadagni mantenendo alto il livello di ritenzione (attrattività) dell'app**.
>

## Tipologie 

Nativa, web, ibrida, cross-plaform
Qual'è la migliore? **dipende**.
### App nativa

 Se si vuole utilizzare al meglio un dispositivo e il sistema operativo sottostante, conviene andare di app nativa. (Java per Android, Swift per IOS).
 L'app fa uso delle API del sistema operativo (SDK), perciò ha maggiori potenzialità ed un look-and-feel che segue le guidelines della piattaforma.

>[!important]
>Una app nativa può essere eseguita su un solo sistema operativo. Ciò significa che per poter distribuire l'app su piattaforme diverse bisogna mantenere **progetti separati**.

Lo sviluppo di app native si presenta quindi come **costoso** in termini di tempo e di denaro. E' da questi difetti che nasce l'esigenza di altre tipologie di applicazioni.
### Web App

Una web app è una applicazione accessibile tramite browser. Sono divenute molto popolari negli ultimi anni, **solamente su computer**. 
Sugli schermi piccolini di telefoni e tablet avere una applicazione all'interno di un browser peggiora l'esperienza utente, oltre che ad avere tanti limiti su quanto è possibile accedere alle funzionalità del dispositivo.
##### PWA

Le **progressive web app** (PWA) sono una tipologia di web app che si comportano come app native. Si installano dal browser e possono usufruire di alcune funzionalità del sistema operativo (notifiche push, sync in background, offline-ready).
Elementi chiave delle PWA sono i **service workers**, script che consentono l'uso di funzionalità vicine all'OS.
#### Capacitor

E' possibile fare uso all'intero di web apps di **plugins** per utilizzare con le componenti native del sistema operativo. Collegano a tutti gli effetti il mondo web a quello mobile, tant'è che sono chiamate anche **app-ibride**.
### Web-native app

Le **web-native app** sono delle applicazioni sviluppate con tecnologie tipiche delle web app tradizionali (HTML, CSS, JS), che sono poi convertite in codice nativo. 
Viene fatto uso di un **bridge**, che traduce codice JS in codice nativo, a discapito di un rallentamento nelle performance. Anche le performance non sono delle migliori, specie se confrontante con una controparte nativa.
### Cross-compiled app

Le **cross-compiled app** sono scritte in un linguaggio unico, che poi sono compilate in codice nativo per Android ed iOS. 
Un caso particolare è **Flutter**, che utilizza i propri widget e canali di comunicazione con dispositivi I/O, bypassando quelli propri dei sistema operativi Android ed iOS.

> [!important]
> Il grande vantaggio delle app cross-compiled è che per più piattaforma basta gestire **una sola codebase**.


