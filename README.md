# 🏆 Football Steph - Knockout Edition (Eliminazione Diretta)

Sito web companion per il gestionale Flutter **Football Steph**, ottimizzato specificamente per **Tornei a Eliminazione Diretta, Coppe e Playoff**. Questo template legge i file JSON esportati dall'app e compila **automaticamente** il tabellone di gara: tu inserisci le squadre di partenza un'unica volta nel calendario, e il sistema fa avanzare da solo le vincitrici (e le perdenti in caso di finalina) turno dopo turno in base ai risultati reali registrati nell'app!

---

## 🚀 Guida Rapida: Dall'App al Web

### 1. Gestire la coppa nell'App
1. Apri l'app **Football Steph** e crea un torneo dedicato alla coppa.
2. Gioca le partite della fase a eliminazione diretta (Ottavi, Quarti, Semifinali, Finale) registrandole regolarmente nella modalità live dell'app (il sistema riconosce anche eventuali tempi supplementari e calci di rigore `dcr`).

### 2. Esportare i database JSON dall'App
Esporta dall'app i file di backup necessari:
* **Database Giocatori** (`database_giocatori.json`)
* **Squadre** (`squadre.json`)
* **Storico Partite** (`storico_partite.json`)

### 3. Posizionare e rinominare i file nel Template Web
Crea la cartella **`data/`** nella root del progetto e inserisci i file con questi nomi standard:
* `data/player.json`
* `data/teams.json`
* `data/match.json`
* `data/scheduled.json` (il tabellone master - vedi guida sotto)

---

## ⚔️ Come configurare il file `scheduled.json` (Knockout con Auto-Avanzamento)

A differenza dei campionati a girone, nel file `data/scheduled.json` della versione Knockout configuri la struttura dell'albero ad eliminazione **una sola volta all'inizio**. 

Le partite successive pescano automaticamente i contendenti usando i codici univoci delle partite precedenti (`"homeFromWinner": "QF1"` o `"homeFromLoser": "SF1"`).

### 📋 Casi d'uso pronti da copiare:

#### Caso 1: Torneo con Quarti, Semifinali e Finalissima (con o senza Finalina 3°/4° posto)
Crea il file `data/scheduled.json` incollando questa struttura:

```json
{
  "tournamentId": "tour_1792064132854",
  "tournamentName": "STEPH KNOCKOUT CUP 2026",
  "stages": [
    {
      "stageName": "Quarti di Finale",
      "matches": [
        {
          "matchCode": "QF1",
          "label": "Quarto 1",
          "homeName": "Solaris FC",
          "awayName": "Vulcania",
          "scheduledDate": "Lunedì 12/10/2026 - Ore 21:00",
          "locationName": "Centro Sportivo Lambrate - Campo 1",
          "mapsUrl": "[https://maps.google.com](https://maps.google.com)"
        },
        {
          "matchCode": "QF2",
          "label": "Quarto 2",
          "homeName": "Aura Viola",
          "awayName": "Smeraldo FC",
          "scheduledDate": "Mercoledì 14/10/2026 - Ore 21:30",
          "locationName": "Centro Sportivo Lambrate - Campo 2",
          "mapsUrl": "[https://maps.google.com](https://maps.google.com)"
        }
      ]
    },
    {
      "stageName": "Semifinali",
      "matches": [
        {
          "matchCode": "SF1",
          "label": "Semifinale 1",
          "homeFromWinner": "QF1",
          "awayName": "Oceanic Blu",
          "scheduledDate": "Lunedì 19/10/2026 - Ore 21:00",
          "locationName": "Centro Sportivo Lambrate - Campo 1",
          "mapsUrl": "[https://maps.google.com](https://maps.google.com)"
        },
        {
          "matchCode": "SF2",
          "label": "Semifinale 2",
          "homeFromWinner": "QF2",
          "awayName": "Real Bianco",
          "scheduledDate": "Martedì 20/10/2026 - Ore 21:30",
          "locationName": "Sporting Club San Siro - Campo A",
          "mapsUrl": "[https://maps.google.com](https://maps.google.com)"
        }
      ]
    },
    {
      "stageName": "Finalissima 1° / 2° Posto",
      "isFinal": true,
      "matches": [
        {
          "matchCode": "FIN",
          "label": "Finale",
          "homeFromWinner": "SF1",
          "awayFromWinner": "SF2",
          "scheduledDate": "Domenica 25/10/2026 - Ore 21:00",
          "locationName": "Arena Centrale San Siro",
          "mapsUrl": "[https://maps.google.com](https://maps.google.com)"
        }
      ]
    },
    {
      "stageName": "Finalina 3° / 4° Posto",
      "isThirdPlace": true,
      "matches": [
        {
          "matchCode": "FIN34",
          "label": "Finale 3° Posto",
          "homeFromLoser": "SF1",
          "awayFromLoser": "SF2",
          "scheduledDate": "Domenica 25/10/2026 - Ore 19:30",
          "locationName": "Arena Centrale San Siro",
          "mapsUrl": "[https://maps.google.com](https://maps.google.com)"
        }
      ]
    }
  ]
}
```

#### Gestione di tornei più grandi

Il motore supporta comunque qualsiasi numero di turni ad eliminazione. Possiamo aggiungere Ottavi di Finale ed estendere l'albero. Basta defiire i blocchi precedenti con i matchCode (""OTT1", "OTT2", ..) e richiamarli nel turno successivo ("homeFromWinner": "OTT1"). 

Si può rimuovere il blocco della Finalina (3/4 posto) ed il sistema lo ignorerà ed a schermo scomparirà il blocco analogo.

---

### ✨ Caratteristiche Principali (UI)
- Auto-Avanzamento Intelligente: Riconosce la squadra vincitrice (anche in caso di vittoria ai calci di rigore) e compila da sola il turno successivo.
- Badge e Temi Dinamici per Turno:
  - Quarti/Ottavi/Sedicesimi $\rightarrow$ Sfondi Verdi con spade (⚔️).
  - Semifinali $\rightarrow$ Sfondi Blu con scudo (🛡️).
  - Finalissima $\rightarrow$ Bordo dorato e Banner Celebrativo d'oro con coppa (🏆) per il Campione.
  - Finalina 3°/4° Posto $\rightarrow$ Sfondi Rossi con emoji (🎭).
