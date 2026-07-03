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