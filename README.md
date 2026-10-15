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

```json

{
  "tournamentId": "tour_1792064132854",
  "tournamentName": "STEPH CHAMPIONS CUP 2026",
  "stages": [
    {
      "stageName": "Ottavi di Finale",
      "matches": [
        {
          "matchCode": "OTT1",
          "label": "Ottavo 1",
          "homeName": "Solaris FC",
          "awayName": "Squadra A",
          "scheduledDate": "Lunedì 05/10/2026 - Ore 21:00",
          "locationName": "Centro Sportivo Lambrate - Campo 1",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "OTT2",
          "label": "Ottavo 2",
          "homeName": "Oceanic Blu",
          "awayName": "Squadra B",
          "scheduledDate": "Lunedì 05/10/2026 - Ore 22:00",
          "locationName": "Centro Sportivo Lambrate - Campo 1",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "OTT3",
          "label": "Ottavo 3",
          "homeName": "Aura Viola",
          "awayName": "Squadra C",
          "scheduledDate": "Martedì 06/10/2026 - Ore 21:00",
          "locationName": "Sporting Club San Siro - Campo A",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "OTT4",
          "label": "Ottavo 4",
          "homeName": "Real Bianco",
          "awayName": "Squadra D",
          "scheduledDate": "Martedì 06/10/2026 - Ore 22:00",
          "locationName": "Sporting Club San Siro - Campo A",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "OTT5",
          "label": "Ottavo 5",
          "homeName": "Vulcanica",
          "awayName": "Squadra E",
          "scheduledDate": "Mercoledì 07/10/2026 - Ore 21:00",
          "locationName": "Centro Sportivo Lambrate - Campo 2",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "OTT6",
          "label": "Ottavo 6",
          "homeName": "Smeraldo FC",
          "awayName": "Squadra F",
          "scheduledDate": "Mercoledì 07/10/2026 - Ore 22:00",
          "locationName": "Centro Sportivo Lambrate - Campo 2",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "OTT7",
          "label": "Ottavo 7",
          "homeName": "Squadra G",
          "awayName": "Squadra H",
          "scheduledDate": "Giovedì 08/10/2026 - Ore 21:00",
          "locationName": "Centro Sportivo Lambrate - Campo 1",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "OTT8",
          "label": "Ottavo 8",
          "homeName": "Squadra I",
          "awayName": "Squadra L",
          "scheduledDate": "Giovedì 08/10/2026 - Ore 22:00",
          "locationName": "Centro Sportivo Lambrate - Campo 1",
          "mapsUrl": "https://maps.google.com"
        }
      ]
    },
    {
      "stageName": "Quarti di Finale",
      "matches": [
        {
          "matchCode": "QF1",
          "label": "Quarto 1",
          "homeFromWinner": "OTT1",
          "awayFromWinner": "OTT2",
          "scheduledDate": "Lunedì 12/10/2026 - Ore 21:00",
          "locationName": "Centro Sportivo Lambrate - Campo 1",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "QF2",
          "label": "Quarto 2",
          "homeFromWinner": "OTT3",
          "awayFromWinner": "OTT4",
          "scheduledDate": "Martedì 13/10/2026 - Ore 21:00",
          "locationName": "Sporting Club San Siro - Campo A",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "QF3",
          "label": "Quarto 3",
          "homeFromWinner": "OTT5",
          "awayFromWinner": "OTT6",
          "scheduledDate": "Mercoledì 14/10/2026 - Ore 21:30",
          "locationName": "Centro Sportivo Lambrate - Campo 2",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "QF4",
          "label": "Quarto 4",
          "homeFromWinner": "OTT7",
          "awayFromWinner": "OTT8",
          "scheduledDate": "Giovedì 15/10/2026 - Ore 21:30",
          "locationName": "Centro Sportivo Lambrate - Campo 1",
          "mapsUrl": "https://maps.google.com"
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
          "awayFromWinner": "QF2",
          "scheduledDate": "Lunedì 19/10/2026 - Ore 21:00",
          "locationName": "Centro Sportivo Lambrate - Campo 1",
          "mapsUrl": "https://maps.google.com"
        },
        {
          "matchCode": "SF2",
          "label": "Semifinale 2",
          "homeFromWinner": "QF3",
          "awayFromWinner": "QF4",
          "scheduledDate": "Martedì 20/10/2026 - Ore 21:30",
          "locationName": "Sporting Club San Siro - Campo A",
          "mapsUrl": "https://maps.google.com"
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
          "mapsUrl": "https://maps.google.com"
        }
      ]
    }
  ]
}

```

---

### ✨ Caratteristiche Principali (UI)
- Auto-Avanzamento Intelligente: Riconosce la squadra vincitrice (anche in caso di vittoria ai calci di rigore) e compila da sola il turno successivo.
- Badge e Temi Dinamici per Turno:
  - Quarti/Ottavi/Sedicesimi $\rightarrow$ Sfondi Verdi con spade (⚔️).
  - Semifinali $\rightarrow$ Sfondi Blu con scudo (🛡️).
  - Finalissima $\rightarrow$ Bordo dorato e Banner Celebrativo d'oro con coppa (🏆) per il Campione.
  - Finalina 3°/4° Posto $\rightarrow$ Sfondi Rossi con emoji (🎭).
 
---

## 🔗 Collegamento con la Fase a Gironi (Campionato)
Se questo tabellone rappresenta la fase finale di un torneo misto che è partito con un girone all'italiana:

1. Tieni il portale della *League Edition* (Campionato) attivo nella cartella principale o in un percorso dedicato (es. `/campionato/`).
2. Nel file `index.html` della Knockout Edition, inserisci un link rapido in alto per permettere ai partecipanti di saltare subito alla classifica della regular season:
   ```html
   <div style="text-align: center; padding: 6px; background: #1E293B;">
     <a href="../campionato/index.html" style="color: #10B981; font-weight: bold; font-size: 12px; text-decoration: none;">
       &larr; Torna alla Classifica del Campionato
     </a>
   </div>
   ```

---

## MIX (Hub + Gironi + Playoff)

```text
Mondiale_2026/
├── index.html               <-- L'HUB CENTRALE (Pagina iniziale con i link a tutto)
├── girone-a/                <-- Sito Campionato (Template League Edition)
│   ├── index.html
│   └── data/ (player, teams, match, scheduled del Girone A)
├── girone-b/                <-- Sito Campionato (Template League Edition)
│   ├── index.html
│   └── data/ (player, teams, match, scheduled del Girone B)
└── playoff/                 <-- Sito Tabellone (Template Knockout Edition)
    ├── index.html
    └── data/ (player, teams, match, scheduled dei Playoff)
```

In `index.html` generale ad esempio si potrebbe impostare così:

```html

<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hub Torneo - Football Steph</title>
  <link rel="stylesheet" href="girone-a/style.css"> <!-- Puoi usare lo stesso stile dei template -->
</head>
<body style="background: #090D16; color: #FFFFFF; font-family: sans-serif; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0;">

  <div style="width: 100%; max-width: 450px; padding: 20px; text-align: center;">
    <div style="font-size: 40px; margin-bottom: 10px;">🏆</div>
    <h1 style="font-size: 22px; font-weight: 900; margin-bottom: 6px;">MONDIALE / TORNEO 2026</h1>
    <p style="color: #94A3B8; font-size: 13px; margin-bottom: 24px;">Seleziona la sezione del torneo che vuoi consultare:</p>

    <!-- SEZIONE GIRONI -->
    <div style="background: #111827; border: 1px solid #1E293B; border-radius: 14px; padding: 16px; margin-bottom: 14px;">
      <h3 style="font-size: 12px; font-weight: 900; color: #10B981; letter-spacing: 1px; margin-bottom: 12px;">FASE A GIRONI</h3>
      <div style="display: flex; flex-direction: column; gap: 8px;">
        <a href="girone-a/index.html" style="background: #1E293B; color: #FFFFFF; padding: 12px; border-radius: 8px; text-decoration: none; font-weight: bold; font-size: 14px; display: block;">
          🔵 Girone A (Classifica & Calendario)
        </a>
        <a href="girone-b/index.html" style="background: #1E293B; color: #FFFFFF; padding: 12px; border-radius: 8px; text-decoration: none; font-weight: bold; font-size: 14px; display: block;">
          🔴 Girone B (Classifica & Calendario)
        </a>
      </div>
    </div>

    <!-- SEZIONE FASE FINALE -->
    <div style="background: #111827; border: 1px solid #1E293B; border-radius: 14px; padding: 16px;">
      <h3 style="font-size: 12px; font-weight: 900; color: #EAB308; letter-spacing: 1px; margin-bottom: 12px;">FASE FINALE</h3>
      <a href="playoff/index.html" style="background: linear-gradient(135deg, #EAB308, #D97706); color: #042F22; padding: 14px; border-radius: 8px; text-decoration: none; font-weight: 900; font-size: 14px; display: block; box-shadow: 0 4px 15px rgba(234, 179, 8, 0.2);">
        🏆 Tabellone Playoff & Eliminazione
      </a>
    </div>

  </div>

</body>
</html>

```
