## Dove siamo

Stiamo lavorando sul repository **`esperto-alberi-avanzato`**, che contiene l'app funzionante con:

- Wrapper Capacitor 8.x per Android
- Build CI via GitHub Actions (JDK 21, keystore fisso, indicatore di build in fondo alla pagina)
- Plugin installati: `@capacitor/filesystem`, `@capacitor/share`, `@capacitor/camera`, `@capacitor/browser`
- Funzionalità operative: identificazione specie con filtri, confronto diretto tra specie, fotocamera con miniature, report PDF con foto di campo

---

## Cosa è stato fatto in questa sessione

**1. Schermata iniziale — ✅ COMPLETATA E TESTATA**
- Nuova vista `tab-home`, attiva di default all'apertura dell'app.
- Logo (cerca `logo.png` nella cartella dell'app; se assente mostra l'emoji 🌳 come fallback — basta aggiungere il file `logo.png` per sostituirlo, nessuna modifica al codice necessaria).
- Titolo "Sistema Esperto Botanico" + breve descrizione.
- Due bottoni grandi: **"Confronto Diretto"** (⚖️) e **"Identificazione"** (🔍), che richiamano `cambiaTab('confronto')` / `cambiaTab('identificazione')`.
- La vecchia barra dei tab (che mostrava insieme i due bottoni "Confronto Diretto" / "Identificazione (Chiave)") è stata **rimossa** perché ridondante rispetto alla home.
- In ciascuna delle due sezioni è stato aggiunto un bottone **"← Home"** ben visibile (pillola con bordo verde) per tornare alla schermata iniziale.
- **Bug risolto durante il test:** la regola CSS `.home-screen` impostava `display: flex` in modo incondizionato, scavalcando `.sezione { display: none }` quando la home non era attiva — per cui la home restava visibile "sopra" le altre sezioni. Corretto usando il selettore `.sezione.active.home-screen` (più specifico), così la home si nasconde correttamente come le altre sezioni.

**2. Lightbox per le foto — ✅ COMPLETATA (da testare via commit)**
- Aggiunto un overlay a schermo intero (`#lightbox`), nascosto di default, con sfondo nero semi-trasparente.
- Listener di click **delegato** su `document`: qualsiasi `<img>` con classe `img-botanica` (foto specie, foto confronto, foto foglia/dettaglio/tronco/frutto/chioma) o `img-zoomabile` (miniature scattate con la fotocamera) apre la foto a piena dimensione cliccandoci sopra.
- Click di nuovo in qualsiasi punto dell'overlay (anche sulla foto) → chiude la lightbox.
- Essendo basato su delega d'eventi, funziona automaticamente anche su eventuali immagini aggiunte in futuro, senza bisogno di altre modifiche al codice.

---

## Cosa resta da implementare (lista originale)

**3. Ricerca testuale nella lista specie — ⬜ DA FARE**
Un campo di testo nella sezione Identificazione che filtra le specie per nome mentre si digita (presumibilmente da integrare con la funzione `filtraSpecie()` già esistente).

**4. Note libere per rilevamento — ⬜ DA FARE**
Un campo testo libero nella scheda del rilevamento (sezione Identificazione, scheda specie), da includere anche nel PDF generato da `esportaReport()`.

**5. Scala di certezza — ⬜ DA FARE**
3 livelli selezionabili: **Certo / Probabile / Dubbio**, da includere nel PDF del report (stessa funzione `esportaReport()`).

**6. Tag aggiuntivi nel CSV — ⬜ DA DEFINIRE**
Non ancora specificato quali tag aggiungere né a cosa servano esattamente — punto rimasto in sospeso, da chiarire con l'utente prima di implementare.

**7. Link a risorse esterne — ⬜ DA DEFINIRE + DA FARE**
- Tecnicamente: apertura in WebView interna tramite `@capacitor/browser` (plugin già installato).
- Da decidere ancora: quali risorse linkare (Wikipedia, Flora Italiana, o entrambe) e da dove (es. un bottone nella scheda specie).

---

## Note tecniche per chi riprende il lavoro

- File principale: `index.html` (singolo file HTML/CSS/JS, nessun bundler).
- L'app carica i dati da `alberi.csv` via `fetch()`; **richiede un server** (anche locale, es. `python3 -m http.server`) per funzionare in test — non si apre come `file://` semplice. In produzione gira dentro Capacitor, che gestisce questo automaticamente.
- Le foto delle specie sono organizzate in sottocartelle sotto `immagini/` (es. `immagini/foglie/`, `immagini/tronchi/`, `immagini/frutti/`, `immagini/chiome/`, `immagini/foglie_dettaglio/`), nominate con l'id della specie + `.jpg`.
- Struttura navigazione attuale: `tab-home` (default) → `cambiaTab('confronto')` / `cambiaTab('identificazione')` → bottone "← Home" per tornare indietro. Non c'è più una tab bar persistente.
- Funzioni chiave da conoscere: `cambiaTab(tabId)`, `filtraSpecie()`, `eseguiConfronto()`, `esportaReport(alberoId)`, `apriLightbox(src)` / `chiudiLightbox()`.

---

## Prossimo passo suggerito

Riprendere dal punto **3 (ricerca testuale nella lista specie)**, oppure chiarire prima i punti in sospeso (6 e 7) se l'utente ha già le idee più chiare su tag CSV e risorse esterne da linkare.
