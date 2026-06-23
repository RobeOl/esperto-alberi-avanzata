## Dove siamo

Stiamo lavorando sul repository **`esperto-alberi-avanzato`**, che contiene l'app funzionante con:

- Wrapper Capacitor 8.x per Android
- Build CI via GitHub Actions (JDK 21, keystore fisso, indicatore di build in fondo alla pagina)
- Plugin installati: `@capacitor/filesystem`, `@capacitor/share`, `@capacitor/camera`, `@capacitor/browser` (appena installato)
- Funzionalità operative: identificazione specie con filtri, confronto diretto tra specie, fotocamera con miniature, report PDF con foto di campo

---

## Cosa abbiamo deciso di implementare

**1. Schermata iniziale**
Logo + titolo + breve descrizione dell'app + due bottoni grandi ("Confronto Diretto" e "Identificazione") — l'app si apre qui invece di mostrare subito i tab.

**2. Ricerca testuale nella lista specie**
Un campo di testo nell'Identificazione che filtra le specie per nome mentre si digita.

**3. Note libere per rilevamento**
Un campo testo libero nella scheda del rilevamento, incluso nel PDF finale.

**4. Scala di certezza**
3 livelli selezionabili: **Certo / Probabile / Dubbio** — inclusi nel PDF del report.

**5. Tag aggiuntivi nel CSV**
Da definire ancora (avevo fatto una domanda rimasta senza risposta).

**6. Link a risorse esterne**
Si aprono in una WebView interna all'app tramite `@capacitor/browser` — da definire ancora quali risorse (Wikipedia, Flora Italiana, o entrambe).

---

