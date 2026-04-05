
[[mobile development]]
# Ciclo vita di una activity

## Activity

Una **activity** è la componente fondamentale di una app Android. La si può pensare come una singola schermata di una applicazione. Le activity sono di design *loosely connected*.

Una activity può avviare altre activity. Questo permette di poter navigare una applicazione o addirittura di avviare altre app.

>[!important]
>In una app mobile ci possono essere più schermate (activity) di avvio. Devono comunque essere esplicitate come tali dallo sviluppatore dell'app.

Una activity deve essere dichiarata nel file **manifest.xml** con il tag `<activity>`. Annidato nel tag vi è la dichiarazione degli intenti, definita all'interno di `<intent-filter>`.  Ad esempio posso visualizzare una vista aperta in un browser.
## Ciclo vita

Una activity può trovarsi in diversi stati. Lo stato di una activity muta velocemente nel tempo che l'utente utilizza l'applicazione.
L'API di Android fornisce all'interno della classe Activity una serie di **hooks**, metodi callback che possono essere arricchiti dagli sviluppatori con codice che verrà eseguito al seguito della entrata/uscita della activity in/da uno stato ben preciso.

[![Activity Lifecycle in Android with Demo App - GeeksforGeeks](https://media.geeksforgeeks.org/wp-content/uploads/20210303165235/ActivityLifecycleinAndroid-601x660.jpg)
### Esempi di callback

- `onCreate`: chiamata alla creazione dell'activity. Per operazioni da fare una sola volta e all'inizializzazione della finestra (es. chiamate HTTP)
- `onStart`: chiamato dopo. `onCreate` ed ogni qualvolta l'utente ritorna nella activity dopo averla stoppata (e.g. averla messa in background)
- `onPause`: chiamato quando l'utente naviga in un altra activity, che ora si trova in foreground.
### Salvare lo stato

Quando lo stato dell'activity cambia, abbiamo quasi sicuramente bisogno di salvare uno stato affinché possa essere ripristinato in seguito. 
Il metodo `onSaveInstanceState` è una callback definita dalla classe `Activity` utilizzata proprio a questo scopo, per memorizzare localmente informazioni.

> [!important]
Utilizzo la classe `Bundle` per memorizzare una stato **semplice** in un formato tale che l'activity sappia ripristinarlo in automatico. Per tipi di stato più complessi conviene utilizzare la classe `ViewModel`.

Sia `instanceState` che `viewModel` sono strumenti per salvare uno stato *in-memory*, che quindi viene cancellato alla chiusura dell'applicazione. E' possibile utilizzare la classe `PersistentStorage` per salvare sul disco delle informazioni in maniera persistente.

