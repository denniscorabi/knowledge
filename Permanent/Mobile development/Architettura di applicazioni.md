# Architettura a strati

Una tipica applicazione mobile è suddivisa in tre strati, ognuno dei quali ha responsabilità ben definite:
- view layer
- domain layer
- data layer

## UI layer

L'interfaccia utente (UI) ha due compiti: **visualizzare dati** e **catturare le interazioni degli utenti**. I dati dell'applicazione devono essere formattati per essere visualizzati correttamente negli widget che compongono l'applicazione.

Una interfaccia può essere aggiornata in due modi:
- dal basso, attraverso ViewModel e cambio di stato
- dall'alto, dall'utente che interagisce con l'applicazione

## Data layer

Il data layer contiene i dati dell'applicazione. E' importante tenere separata la fonte dei dati dall'interfaccia con cui si accede a questi ultimi. 
Una **repository** scherma una o più **data source**. Esse hanno le seguenti responsabilità:
- Esporre i dati attraverso una interfaccia nota
- Centralizzare le modifiche dei dati
- Risolvere confliti tra le fonti di dati
- Astrarre le fonti dei dati. La fonte dei dati, così come la loro rappresentazione, non deve essere nota al resto dell'applicazione.

> [!important]
> La UI NON deve sapere quale sia la reale fonte dei dati. Tutto deve passare da delle **repository**.

---
# Android Jetpack per l'architettura

Android Jetpack è un insieme di classi che ci permettono di realizzare la nostra applicazione attraverso le guidelines Android, riducendo il codice *boilerplate* e permettendoci di concentrarci sul codice specifico per la applicazione.