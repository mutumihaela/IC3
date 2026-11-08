# IC3

**S1 · SensorTwin · Motor turbofan (NASA C-MAPSS): durata de viață rămasă**

Autor: Mutu Mihaela

Proiect de semestru la Inteligență Computațională III. Întrebarea twin-ului: câte cicluri mai poate zbura un motor până la defectare și cât de sigură este estimarea? Clientul este echipa de mentenanță, care decide când oprește motorul pentru revizie.

## Conținut

| Fișier | Checkpoint | Ce este |
|---|---|---|
| `CP1_S1.ipynb` | CP1 · observații | citirea datelor, ținta RUL, 64 de verificări de integritate, regimul de zbor, senzorii constanți, împărțirea pe motoare, figura și scrierea data card-ului |
| `datacard_S1.md` | CP1 | data card-ul setului de date, scris de `CP1_S1.ipynb` |
| `CP2_S1.ipynb` | CP2 · reprezentare | două reprezentări ale ferestrei de 30 de cicluri, cinci baseline-uri, protocolul pe 5 împărțiri pe motoare |
| `reports/` | CP1, CP2 | rezultate scrise de notebook-uri: proveniență (SHA-256), statistici, tabele și figuri |
| `splits/` | CP1 | ce motoare sunt în antrenare și ce motoare în validare |

Datele NASA (`CMAPSSData/`) și datele procesate (`data_processed/`) nu sunt în repository.

## Cum rulezi

1. Descarcă `CMAPSSData.zip` de pe https://data.nasa.gov/dataset/cmapss-jet-engine-simulated-data și dezarhivează-l.
2. Notebook-urile caută singure folderul `CMAPSSData`: lângă notebook sau un nivel mai sus. Dacă e în altă parte, scrie calea în `DATA_DIR`, în prima celulă de cod.
3. `pip install -r requirements.txt`
4. Deschide notebook-ul în VS Code sau Jupyter și rulează toate celulele. Notebook-urile sunt independente; CP2 durează 2–3 minute.

Ce trebuie să iasă:
- `CP1_S1.ipynb`: 709 motoare de antrenare, 707 de test, 0 valori lipsă, 64 din 64 de verificări trec.
- `CP2_S1.ipynb` (FD001): Random Forest pe statisticile ferestrei are RMSE de test în jur de 13,3 cicluri; climatologia (vârsta medie a flotei) în jur de 36,8.

## Date și citare

Licența datelor nu este specificată de NASA; depozitul NASA PCoE cere citarea:

- A. Saxena, K. Goebel (2008). Turbofan Engine Degradation Simulation Data Set. NASA Prognostics Data Repository, NASA Ames Research Center, Moffett Field, CA.
- A. Saxena, K. Goebel, D. Simon, N. Eklund (2008). Damage Propagation Modeling for Aircraft Engine Run-to-Failure Simulation. PHM08, Denver, CO.

