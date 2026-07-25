# 🌍 Epic Route Generator

Un progetto in Python per generare mappe di viaggio interattive ad altissima resa grafica, ispirate allo stile dei classici atlanti geografici (National Geographic). 

Questo script non si limita a disegnare linee: interroga le API di OpenStreetMap (OSRM) per tracciare le **strade reali** e calcolare i **chilometri esatti**, gestisce automaticamente le icone per **evitare sovrapposizioni** e genera una **leggenda elegante e dinamica** in HTML/CSS. 
Inoltre, include un secondo script per esportare la mappa in un'immagine PNG in qualità Retina/4K.

---

## 🛠️ Guida alla Personalizzazione (Crea il TUO viaggio!)

Lo script principale , cella n.1 del file (`Epic_Route_Generator.ipynb`) è stato scritto per essere facilmente modificato anche da chi non ha grande esperienza con Python. Segui questi step per adattarlo al tuo itinerario (eventualmente consiglio l'utilizzo di Google Colab per il lancio del codice Python).

### 1. Cambiare le Tappe Principali
Cerca la variabile `tappe = [...]` all'inizio del codice. Qui è dove inserisci i tuoi stop. 
Il formato di ogni riga è:
`("Nome Tappa", [Latitudine, Longitudine], "Emoji", "colore")`
*   **Emoji:** Puoi incollare qualsiasi emoji di sistema (es. 🛵, 🏛️, 🍕).
*   **Colori:** Lo script gestisce in automatico `green` (Fase 1), `orange` (Fase 2) e `darkred` (Fase 3). Puoi estendere i colori modificando i cicli `if` successivi.

### 2. Modificare (o rimuovere) il Volo Iniziale
Cerca la sezione `# 4. TRACCIAMENTO VOLO`. 
Se il tuo viaggio prevede un volo, cambia le coordinate di partenza (`coord_jnb_reale`) e arrivo (`coord_vfa_reale`), e modifica i testi all'interno dei campi `tooltip="..."`. Se il tuo viaggio non prevede voli, puoi semplicemente cancellare questa intera sezione.

### 3. Sottotitoli e Tappe Intermedie
Nella sezione `# 8. Generazione della leggenda`, trovi un blocco di codice che inizia con `if i == 2: lista_html.append(...)`.
Questo serve per aggiungere piccole descrizioni o deviazioni sotto a specifiche tappe (es. "↳ Visita al museo..."). Cambia il numero `i` (che rappresenta l'indice della tappa, partendo da 0) e il testo, oppure cancella le righe `if/elif` se non ti servono.

### 4. Titoli, Fasi e Bandiere (Layout HTML)
Cerca la stringa `# 9. Layout HTML Finale`. Questo blocco contiene il design della mappa:
*   **Titoli:** Cambia il testo dentro `<h1>` e `<h2>` per dare un nome al tuo viaggio.
*   **Bandiere:** Nel blocco `<div class="flags-banner">`, cambia le sigle delle immagini (es. da `zm.png` a `fr.png` per la Francia) per mostrare le bandiere dei paesi che attraverserai.
*   **Nomi delle Fasi:** Cambia il testo dentro i div `<div class="fase-titolo">` (es. "Fase 1: ...").

### 5. ATTENZIONE: La gestione della Minimappa
Attualmente lo script utilizza una **minimappa SVG customizzata** in basso a destra (creata in linguaggio vettoriale SVG puro) pensata specificamente per evidenziare il sud dell'Africa. Mantenere questa SVG per un altro continente richiede di ricavare le complesse coordinate pixel dei nuovi poligoni.

**💡 Il metodo più semplice per il tuo viaggio:**
Se il tuo itinerario si svolge altrove, ti consigliamo di rimuovere la minimappa SVG e riattivare la comoda minimappa base di Folium:
1. Cancella l'intero blocco HTML che va da `<!-- CONTENITORE DELLA NUOVA MINIMAPPA -->` fino alla fine del tag `</svg>`.
2. All'inizio dello script, togli il cancelletto `#` da `from folium.plugins import MiniMap`.
3. Cerca la voce `# VECCHIA Minimappa in basso a destra` e de-commenta le due righe sottostanti:
   ```python
   minimap = MiniMap(tile_layer='CartoDB Positron', position='bottomright', width=160, height=160, zoom_level_offset=-5)
   mappa_tour.add_child(minimap)


## 📸 Esportare la mappa in Alta Risoluzione (Script 2)
Il file generato dalla prima cella dello script è un .html navigabile. Se vuoi trasformarlo in una bellissima immagine statica (PNG) in 4K da stampare o inserire in un PDF, utilizza il secondo script (generazione_screenshot.py).

Lo script presente nella cella n. 2 del file (`Epic_Route_Generator.ipynb`) è ottimizzato per Google Colab:

Installa in background un browser Chrome "Headless" (invisibile).

Usa Selenium per aprire la mappa.

Applica un trucco magico: --force-device-scale-factor=3, che inganna il browser facendogli credere di essere uno schermo Retina, moltiplicando i pixel x3 per una nitidezza assoluta.

Salva e scarica automaticamente l'immagine.
