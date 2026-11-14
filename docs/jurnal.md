**13 noiembrie 2026 — Explorarea imaginilor RMN**

Am organizat notebookul de clasificare și am adăugat titluri celor 8 celule.

Am folosit datasetul Brain Tumor MRI Dataset, publicat pe Kaggle de Masoud Nickparvar. Acesta conține clasele glioma, meningioma, notumor și pituitary.

Rezultatele rulării:

- Imagini în Training: 5600.
- Imagini în Testing: 1600.
- Forma batchului: `torch.Size([32, 3, 224, 224])`.
- Erori întâlnite: `SecretNotFoundError` la citirea tokenului Kaggle. Problema a fost rezolvată prin crearea secretului `rmn` și folosirea comenzii `userdata.get("rmn")`.

Preprocesarea configurată include redimensionarea la 224 × 224 pixeli, oglindire orizontală și rotire aleatoare pentru antrenare, transformarea în tensori și normalizarea. Imaginile sunt încărcate în batchuri de câte 32.

În această etapă am pregătit încărcarea datelor. Nu am antrenat încă o rețea neuronală.

Următoarea activitate: verificarea imaginilor corupte și a duplicatelor.


**14 noiembrie 2026 Verificarea datelor și pregătirea listelor pentru experimente**

Am extins notebookul cu celulele 9–13 pentru verificarea citibilității imaginilor, identificarea duplicatelor exacte, analizarea distribuției acestora și pregătirea listelor de fișiere.

Pentru identificarea duplicatelor am calculat amprente SHA-256 din conținutul fișierelor. Rezultatele obținute sunt:

- 153 de grupuri de duplicate exacte;
- 340 de fișiere implicate în aceste grupuri;
- 187 de copii suplimentare, dacă păstrăm un singur reprezentant pentru fiecare conținut;
- 0 grupuri de duplicate exacte comune între Training și Testing;
- 0 grupuri de duplicate exacte cu etichete diferite.

Distribuția rezultatelor este:

| Set | Fișiere inițiale | Conținuturi distincte SHA-256 | Copii suplimentare |
|---|---:|---:|---:|
| Training | 5.600 | 5.429 | 171 |
| Testing | 1.600 | 1.584 | 16 |
| Total | 7.200 | 7.013 | 187 |

Am generat în directorul `rapoarte` următoarele fișiere:

- `training_distinct.csv`: lista celor 5.429 de intrări distincte din Training;
- `testing_original.csv`: lista completă a celor 1.600 de intrări din Testing;
- `testing_distinct.csv`: lista celor 1.584 de intrări distincte din Testing.

Aceste liste conțin căile relative către imagini, setul original, clasa și amprenta SHA-256. Crearea lor nu a modificat sau șters imaginile originale. Loaderele existente citesc încă directoarele inițiale și vor trebui adaptate ulterior.

Verificarea SHA-256 identifică fișiere identice, dar nu identifică neapărat variante redimensionate, recomprimate sau transformate ale aceleiași imagini. De asemenea, nu demonstrează că imaginile provin de la pacienți diferiți.

În celula 13 am căutat fișiere suplimentare care ar putea conține metadate și am inspectat exemple de nume de imagini. Căutarea nu a găsit fișiere candidate, iar până în acest moment nu am identificat o asociere verificată între imagini și pacienți.

Am observat și nume care conțin marcajul `-aug-`. Acesta sugerează posibile imagini augmentate, însă proveniența și transformările lor nu au fost încă verificate.

Nu am separat încă setul de validare și nu am început antrenarea unei rețele.

Următoarele activități sunt clarificarea fișierelor cu marcajul `-aug-`, stabilirea protocolului de împărțire a datelor și adaptarea încărcării imaginilor pentru primele experimente de clasificare.