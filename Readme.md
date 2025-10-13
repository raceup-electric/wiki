# README

In questo *readme* è presente una veloce guida per interfacciarsi al download e all'utilizzo della Wiki di team.

Prima di tutto per scaricare questa cartella e poterla editare con obsidian è necessario copiare questo Vault/ repository in una cartella locale nel proprio computer. Per farlo apri un qualsiasi shell/terminale e entra in un percorso in cui desideri archiviare la wiki, dopodichè utilizza il seguente comando:

```zsh
git clone https://github.com/raceup-electric/wiki.git
```

Se non hai mai utilizzato Git o non lo hai installato, probailmente vedrai un errore. In tal caso, procedi all’installazione seguendo le istruzioni per il tuo sistema operativo:

- **Windows:** scarica e installa Git da [https://git-scm.com/download/win](https://git-scm.com/download/win)
- **macOS:** puoi installarlo tramite Homebrew con `brew install git`, oppure scaricarlo da [https://git-scm.com/download/mac](https://git-scm.com/download/mac)
- **Linux:** utilizza il gestore di pacchetti della tua distribuzione, ad esempio su Ubuntu: `sudo apt install git`

Dopo l’installazione, puoi ripetere il comando `git clone` riportato sopra per scaricare la wiki.
***
## Gestione Obsidian

Una volta fatto ciò sei finalmente pronto per editare la wiki del team tramite `Obsidian`.

A questo punto per automatizzare la sincronizzazione con la repository online, è necessario scaricare all' interno di `Obsidian` il comunity plugin chiamato "`Git`".  Per farlo vai sull'ingranaggio in basso a sinistra nella schermata principale di `Obsidian`, poi in `Community plugins > Browse` 

Una volta fatto ciò ricorda che prima di scrivere nuove sezioni della wiki ad ogni sessione di utilizzo sarà necessario aggiornarsi con la repository remota. Per gestire tutta questa procedura il plugin `Git` rende disponibile nella barra laterale sinistra un icona dove è possibile gestire questo tipo di operazioni. Solitamente apparirà come ultima. In genere è fondamentale eseguire le seguenti operazioni in quest'ordine :

- Fetch (allineamento con la repository online)
- Pull (download dei  nuovi file aggiunti da remoto)

Così facendo si è pronti per poter modificare/ aggiungere dei nuovi documenti/note, ma è fondamentale farlo per evitare conflitti con file che sono stati aggiunti/modificati da altre persone. 

Ora dopo aver fatto le proprie modifiche in locale per aggiornare vice versa la repository online sarà invece necessario eseguire le seguenti operazioni:

- Commit and sync 
- Push (upload delle modifiche fatte in locale)
***
## Regole d'uso

In genere una buona prassi per qualsiasi reparto del team sarebbe bello se per ogni topic specifico si facesse un breve paragrafo in cui si dia un overview generica dell'argomento, in modo tale che possa tornare utile anche a reparti diversi da quello di riferimento. 
Invece al di sotto di tale paragrafo l'idea è quella di scrivere i dettagli tecnici utili a persone interne al reparto. 

È inoltre fondamentale la modularità e la divisione della wiki del proprio reparto attraverso folder specifici per avere un organizzazione efficiente. Una linea guida di massima per poter ottenere ciò è dividere il folder del proprio reparto in base ai vari sottogruppi di interesse della propria divisione.

Un'altra linea guida è quella di documentare e dare un riferimento preciso quando si scrive un concetto teorico, ovvero è importante classificare se quel determinato fatto è relativo alla macchina dell'anno corrente o ad una sua versione precedente, anche qui magari dividendolo tramite paragrafi.