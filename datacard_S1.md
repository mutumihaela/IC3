# Data card: NASA C-MAPSS (tema S1, IC3)

Mini-twin: mentenanța unui motor turbofan · Autor: Mutu Mihaela · CP1, stratul de observații

Cifrele din acest document sunt scrise de notebook-ul `CP1_S1.ipynb` (secțiunea 11), din rularea curentă. Nu sunt copiate manual.

## 1. Pe scurt

| Câmp | Valoare |
|---|---|
| Ce conține | Serii de timp multivariate pentru o flotă simulată de motoare turbofan, de la funcționare normală până la defectare (antrenare) sau până la un moment înainte de defectare (test). |
| Sursă | NASA Ames, Prognostics Center of Excellence (PCoE). Date generate cu simulatorul C-MAPSS. Referință: Saxena, Goebel, Simon, Eklund (2008). |
| Unde se descarcă | `CMAPSSData.zip` de pe https://data.nasa.gov/dataset/cmapss-jet-engine-simulated-data (fișier: https://data.nasa.gov/docs/legacy/CMAPSSData.zip) sau din NASA PCoE Data Set Repository. |
| Perioadă | Fără timp calendaristic. Axa de timp este ciclul de zbor. Setul a fost publicat în 2008 pentru competiția PHM08. Pe data.nasa.gov: ultima modificare 15 mai 2024. |
| Rezoluție | Temporală: un rând pe motor pe ciclu de zbor (un instantaneu pe zbor). Spațială: nu se aplică; obiectul este motorul. |
| Unități | Temperaturi în °R, presiuni în psia, turații în rpm, debite în lbm/s, phi în pps/psi, restul fără unitate (secțiunea 4). Fișierele nu conțin nume de coloane sau unități. |
| Dimensiune | 709 motoare de antrenare (160 359 rânduri) și 707 motoare de test (104 897 rânduri), în 4 subseturi. 26 de coloane: motor, ciclu, 3 setări de operare, 21 de senzori. |
| Goluri | 0 valori lipsă. Ciclurile sunt consecutive, fără salturi sau duplicate. Unii senzori sunt constanți (secțiunea 6). Seriile de test sunt trunchiate intenționat. |
| Împărțire train/test | Test: fișierele `test_FD00x` de la NASA, cu RUL-ul real în `RUL_FD00x`, folosite doar la evaluarea finală. Validare: 20% din motoarele de antrenare, alese cu seed 42, motoare întregi (secțiunea 7). |
| Licență | Nespecificată. data.nasa.gov afișează „License not specified”. Depozitul PCoE cere citarea depozitului și a autorilor datelor și precizează că datele se folosesc pe propriul risc. |

## 2. Sursă și proveniență

- Producător: NASA Ames Research Center, Prognostics Center of Excellence. Întreținător pe data.nasa.gov: Chris Teubert.
- Cum au fost generate: simulatorul C-MAPSS al unui motor turbofan comercial mare. Fiecare motor pornește cu uzură inițială și variații de fabricație diferite, necunoscute. Un defect apare la un moment dat și crește până la defectare. Peste ieșiri s-a adăugat zgomot de măsurare (Saxena et al., 2008).
- Fișiere folosite: `train_FD001…FD004.txt`, `test_FD001…FD004.txt`, `RUL_FD001…FD004.txt`, `readme.txt`.
- Proveniență: CMAPSSData.zip descărcat de pe data.nasa.gov (NASA Ames, Prognostics Center of Excellence). Amprenta SHA-256 a fiecărui fișier este în `reports/provenance.json`.

## 3. Dimensiuni pe subset

Durata de viață = numărul de cicluri până la defectare al unui motor de antrenare. Pentru test, „cicluri observate” = cât din serie este dat, iar „RUL real” = câte cicluri mai avea motorul după ultimul ciclu observat.

| Subset | Motoare antrenare | Rânduri antrenare | Motoare test | Rânduri test | Regimuri de zbor | Moduri de defect | Durata de viață min / mediană / max | Cicluri observate în test | RUL real la ultimul ciclu |
|---|---|---|---|---|---|---|---|---|---|
| FD001 | 100 | 20 631 | 100 | 13 096 | 1 | HPC | 128 / 199 / 362 | 31–303 | 7–145 |
| FD002 | 260 | 53 759 | 259 | 33 991 | 6 | HPC | 128 / 199 / 378 | 21–367 | 6–194 |
| FD003 | 100 | 24 720 | 100 | 16 596 | 1 | HPC + ventilator | 145 / 220,5 / 525 | 38–475 | 6–145 |
| FD004 | 249 | 61 249 | 248 | 41 214 | 6 | HPC + ventilator | 128 / 234 / 543 | 19–486 | 6–195 |

Observație: `readme.txt` spune că FD004 are 248 de motoare de antrenare și 249 de motoare de test. Fișierele au 249 de motoare de antrenare și 248 de motoare de test, iar `RUL_FD004.txt` are 248 de rânduri. Readme-ul inversează cele două numere. Notebook-ul verifică numerele din fișiere.

## 4. Variabile și unități

Coloanele 1–2: `unit` (numărul motorului) și `cycle` (ciclul de zbor, începe de la 1).

Coloanele 3–5: setările de operare. Saxena et al. (2008) definesc regimul de zbor prin altitudine (0–42 000 ft), număr Mach (0–0,84) și unghiul manetei de accelerație (TRA, până la 100). Fișierele nu spun care coloană e care. Intervalele din FD002 se potrivesc: `setting_1` între 0 și 42,008 (altitudine, în mii de ft), `setting_2` între 0 și 0,842 (Mach), `setting_3` între 60 și 100 (TRA). Atribuirea este o deducție din intervale. În FD001 și FD003 setările variază doar cu zgomot în jurul unui singur regim.

Coloanele 6–26: cei 21 de senzori. Simbolurile, descrierile și unitățile sunt din Tabelul 2 din Saxena et al. (2008). Grupa e folosită mai târziu pentru ablația pe surse. Intervalul este cel observat în setul de antrenare FD001.

| Coloană | Simbol | Descriere (Saxena et al., 2008) | Unitate | Grupă | Interval FD001 |
|---|---|---|---|---|---|
| s1 | T2 | Total temperature at fan inlet | °R | temperatură | 518,67 – 518,67 |
| s2 | T24 | Total temperature at LPC outlet | °R | temperatură | 641,21 – 644,53 |
| s3 | T30 | Total temperature at HPC outlet | °R | temperatură | 1571,04 – 1616,91 |
| s4 | T50 | Total temperature at LPT outlet | °R | temperatură | 1382,25 – 1441,49 |
| s5 | P2 | Pressure at fan inlet | psia | presiune | 14,62 – 14,62 |
| s6 | P15 | Total pressure in bypass-duct | psia | presiune | 21,6 – 21,61 |
| s7 | P30 | Total pressure at HPC outlet | psia | presiune | 549,85 – 556,06 |
| s8 | Nf | Physical fan speed | rpm | turație | 2387,9 – 2388,56 |
| s9 | Nc | Physical core speed | rpm | turație | 9021,73 – 9244,59 |
| s10 | epr | Engine pressure ratio (P50/P2) | - | presiune | 1,3 – 1,3 |
| s11 | Ps30 | Static pressure at HPC outlet | psia | presiune | 46,85 – 48,53 |
| s12 | phi | Ratio of fuel flow to Ps30 | pps/psi | debit / raport | 518,69 – 523,38 |
| s13 | NRf | Corrected fan speed | rpm | turație | 2387,88 – 2388,56 |
| s14 | NRc | Corrected core speed | rpm | turație | 8099,94 – 8293,72 |
| s15 | BPR | Bypass ratio | - | debit / raport | 8,3249 – 8,5848 |
| s16 | farB | Burner fuel-air ratio | - | debit / raport | 0,03 – 0,03 |
| s17 | htBleed | Bleed enthalpy | - | debit / raport | 388 – 400 |
| s18 | Nf_dmd | Demanded fan speed | rpm | turație | 2388 – 2388 |
| s19 | PCNfR_dmd | Demanded corrected fan speed | rpm | turație | 100 – 100 |
| s20 | W31 | HPT coolant bleed | lbm/s | debit / raport | 38,14 – 39,43 |
| s21 | W32 | LPT coolant bleed | lbm/s | debit / raport | 22,8942 – 23,6184 |

## 5. Ținta: ciclurile rămase (RUL)

- Antrenare: motorul merge până la defectare, deci RUL = ultimul ciclu − ciclul curent. RUL este 0 la ciclul defectării.
- Test: seria se oprește înainte de defectare. `RUL_FD00x.txt` dă RUL-ul real la ultimul ciclu observat al fiecărui motor. Notebook-ul calculează RUL și pentru ciclurile anterioare, dar evaluarea standard folosește doar ultimul ciclu.
- Plafonarea RUL-ului (de exemplu la 125 de cicluri, presupunând că degradarea nu se vede la început) nu e aplicată în datele salvate. Este o decizie de reprezentare pentru CP2. În literatură se folosesc valori între 115 și 135 (Ellefsen et al., 2019).

## 6. Goluri, calitate și senzori fără informație

- Valori lipsă: 0 în toate cele 8 fișiere de date.
- Continuitate: la fiecare motor ciclurile încep de la 1 și cresc din 1 în 1. Toate cele 64 de verificări din notebook (secțiunile 5–7) trec.
- Lungimi inegale: motoarele de antrenare trăiesc între 128 și 543 de cicluri. Un model pe ferestre trebuie să trateze și seriile de test scurte: cea mai scurtă are 19 cicluri. Figura `reports/fig_datacard.png` arată distribuția duratelor de viață și evoluția senzorului T50.
- Zgomot: adăugat intenționat de NASA la toate ieșirile.
- Senzori constanți: nu poartă informație despre degradare și se elimină. La FD002 și FD004 fiecare senzor se mișcă odată cu regimul de zbor, deci constanța se verifică în fiecare regim.

| Subset | Unde se verifică constanța | Constanți (o singură valoare) | Aproape constanți (2 valori) | Senzori păstrați |
|---|---|---|---|---|
| FD001 | pe tot setul de antrenare | s1 (T2), s5 (P2), s10 (epr), s16 (farB), s18 (Nf_dmd), s19 (PCNfR_dmd) | s6 (P15) | 14 |
| FD002 | în fiecare regim de zbor | s1 (T2), s5 (P2), s18 (Nf_dmd), s19 (PCNfR_dmd) | s10 (epr), s16 (farB) | 15 |
| FD003 | pe tot setul de antrenare | s1 (T2), s5 (P2), s16 (farB), s18 (Nf_dmd), s19 (PCNfR_dmd) | niciunul | 16 |
| FD004 | în fiecare regim de zbor | s1 (T2), s5 (P2), s18 (Nf_dmd), s19 (PCNfR_dmd) | s16 (farB) | 16 |

La FD001 lista eliminată (s1, s5, s6, s10, s16, s18, s19) coincide cu cea folosită de Ellefsen et al. (2019). Datele salvate păstrează toate coloanele; lista `sensors_kept` din `reports/datacard_stats.json` spune ce intră în model.

## 7. Împărțirea datelor și reguli contra scurgerii de informație

| Subset | Antrenare | Validare | Test (NASA) |
|---|---|---|---|
| FD001 | 80 motoare / 16 390 rânduri | 20 motoare / 4 241 rânduri | 100 motoare / 13 096 rânduri |
| FD002 | 208 motoare / 43 206 rânduri | 52 motoare / 10 553 rânduri | 259 motoare / 33 991 rânduri |
| FD003 | 80 motoare / 19 827 rânduri | 20 motoare / 4 893 rânduri | 100 motoare / 16 596 rânduri |
| FD004 | 199 motoare / 49 147 rânduri | 50 motoare / 12 102 rânduri | 248 motoare / 41 214 rânduri |

- Validarea se face pe motoare întregi: un motor este fie în antrenare, fie în validare. O împărțire aleatoare pe rânduri ar pune cicluri vecine ale aceluiași motor în ambele părți și ar umfla rezultatele.
- Echivalentul LOYO din planul cursului este aici „motoare nevăzute”: twin-ul trebuie să funcționeze pe un motor nou.
- Normalizarea senzorilor se va calcula doar pe motoarele de antrenare (la FD002 și FD004, separat pe fiecare regim de zbor).
- `RUL_FD00x.txt` nu se folosește la antrenare sau la alegerea hiperparametrilor.
- Listele de motoare sunt salvate în `splits/FD00x_split.json` (seed 42, fracție de validare 0,2).

## 8. Licență și citare

- data.nasa.gov: „License not specified”, acces public.
- NASA PCoE Data Set Repository: datele se folosesc pe propriul risc; publicațiile trebuie să menționeze depozitul și autorii datelor.
- Citare pentru date: A. Saxena, K. Goebel (2008). Turbofan Engine Degradation Simulation Data Set. NASA Prognostics Data Repository, NASA Ames Research Center, Moffett Field, CA.
- Citare pentru metoda de generare: A. Saxena, K. Goebel, D. Simon, N. Eklund (2008). Damage Propagation Modeling for Aircraft Engine Run-to-Failure Simulation. International Conference on Prognostics and Health Management (PHM08), Denver, CO. doi:10.1109/PHM.2008.4711414

## 9. Limitări

- Datele sunt simulate. Rezultatele nu se transferă direct pe motoare reale; articolul trebuie să spună asta.
- Un singur tip de motor și doar două moduri de defect (HPC și ventilator).
- Regimurile de zbor sunt 6 puncte fixe, nu profiluri reale de zbor.
- Seriile nu conțin intervenții de mentenanță: în antrenare defectul crește până la defectare (readme NASA).
- Nu există timp calendaristic, meteo sau altă sursă externă aliniată pe motor. Planul cursului cere minimum două surse eterogene. Propunerea: setările de operare (contextul zborului) și patru grupe de senzori (temperaturi, presiuni, turații, debite și rapoarte), folosite ca surse separate în ablație. Aștept confirmarea profesorului.
- Setul este foarte studiat; cifrele din literatură folosesc protocoale diferite și nu se compară direct.

## 10. Cum se reproduce

Deschide `CP1_S1.ipynb` în VS Code (sau Jupyter), setează `DATA_DIR` dacă folderul `CMAPSSData` nu stă lângă notebook și rulează toate celulele. Pachete: `pandas`, `numpy`, `matplotlib`.
