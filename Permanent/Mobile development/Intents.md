# Intents

Comunicazione tra applicazioni via chiamate per l'attivazione di activity, service e broadcast receiver. 

>Le componenti possono essere attivate dall'applicazione stessa, o da altre applicazioni. 

Ad esempio, l'app di posta elettronica rende la vista "invio mail" **pubblica**, così che una applicazione possa fornire un'azione di invio mail (che fa partire l'activity del client di posta).

> Intent = intenzione di avviare un particolare componente.

Gli intent lavorano grazie alla mediazione del sistema Android, che funziona a tutti gli effetti come un **dispatcher** di eventi (o di *intenti*).
## Intent esplicito ed implicito

Abbiamo due tipologie di intent:
- Intent esplicito
- Intent implicito

L'**intent esplicito** va ad attivare un componente specifico. Questo implica che una applicazione deve conoscere il nome del componente da attivare, anche di altre applicazioni. 

>[!warning] 
>L'intent esplicito si utilizza con i service per motivi di sicurezza. Gli intent impliciti potrebbero essere captati da servizi malevoli.

Gli **intent impliciti**, d'altra parte, sono messaggi indirizzati non tanto ad uno specifico componente, quanto ad un **tipo** di componenti. 
Il tipo di componente viene specificato attraverso una action. Il sistema operativo si occuperà di far partire il componente di default per quel tipo o di mostrare un menu di scelta dove l'utente può scegliere quale applicazione in grado di eseguire l'intent avviare 

Una applicazione specifica quali tipi di intent impliciti ricevere nel file `manifest.xml`. 

>[!information]
>Android, e più nello specifico la classe `Intent`, sono distribuite con un insieme di categorie di componenti di default: `CATEGORY_BROWSABLE`, `CATEGORY_LAUNCHER`, ecc.

