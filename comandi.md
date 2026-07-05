cp -r ./esperto-alberi-foto ./esperto-alberi-avanzata

cd esperto-alberi-avanzata/

rm -rf .git

git init

git branch -M main

git remote add origin git@github.com:RobeOl/esperto-alberi-avanzata.git

git add .

git commit -m "Fork: base per sviluppo funzionalità avanzate"

git push -u origin main

npm install @capacitor/browser

---

# cancellare un file dal progetto:
rm index.html 

git add -A

git commit -m "Rimuovo index.html dalla radice (duplicato non necessario)"

git push

---
# Aggiornamenti qualsiasi
Dopo ogni aggiornamento:

npx cap sync android

prima di:

git add .

git status

git commit -m "commento"

git push

---
# DOMANDA: ho una app html/javascript portata in android con Capacitor. Tutta la compilazione viene fatta su GitHub. In www la app ha il file html, un database CSV e una cartella con immagini. La domanda è: devo fare "npx cap sync android" anche se sostituisco solo le immagini?

Dipende da cosa fa esattamente la tua pipeline su GitHub, ma in generale:

**Cosa fa `cap sync`**
`npx cap sync android` è equivalente a:
```
npx cap copy android   (copia i file da www/ dentro android/app/src/main/assets/public/)
+ npx cap update android (aggiorna plugin nativi e dipendenze)
```

**Se cambi solo immagini in www/**
Non hai toccato plugin nativi, `capacitor.config`, né aggiunto/rimosso pacchetti npm. Quindi la parte "update" di `sync` è inutile in questo caso.

Però ti serve comunque che i file aggiornati vengano copiati nella cartella nativa Android, altrimenti il build userà le immagini vecchie che sono già dentro `android/app/src/main/assets/public/`. Quindi:

- **Basta `npx cap copy android`** — più leggero e veloce, copia solo gli asset web (html, css, js, csv, immagini) senza toccare plugin/dipendenze native.
- **`npx cap sync android`** funziona lo stesso (copy è incluso), semplicemente fa anche lavoro in più che non serve, quindi impiega più tempo.

**Cosa serve sul serio**
L'importante non è "sync vs copy" in sé, ma che *uno dei due* venga eseguito prima della build Android nella tua pipeline CI. Se il tuo workflow GitHub Actions fa qualcosa tipo:

```yaml
- run: npx cap copy android
- run: cd android && ./gradlew assembleRelease
```

allora sei a posto anche solo con `copy`. Se invece il tuo script gira solo `npx cap sync` a prescindere da cosa cambia (approccio più semplice da mantenere, meno rischio di dimenticare un update quando serve), va bene lasciarlo così: è solo un filo più lento, non sbagliato.

**In sintesi**: per sole immagini, `cap copy` sarebbe sufficiente e più veloce, ma `cap sync` funziona comunque correttamente — non è un errore usarlo, è solo ridondante in questo caso specifico.