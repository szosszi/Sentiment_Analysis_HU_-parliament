# Hangnem a parlamentben: a magyar parlamenti pártok felszólalásainak szentimentelemzése (2014–2023)

Ez a repó egy TDK-dolgozat (Budapesti Corvinus Egyetem, 2026–2027. őszi félév) elemzőkódját tartalmazza. Az elemzés a CLARIN ParlaMint projekt magyar korpuszán (ParlaMint-HU v5.0) azt vizsgálja, hogy eltér-e a kormánypárti és az ellenzéki, illetve a bal- és jobboldali pártok parlamenti felszólalásainak szentimentje.

## Kutatási kérdés és hipotézisek

**K1:** Kimutatható-e szignifikáns különbség a kormánypárti és az ellenzéki, illetve a bal- és jobboldali pártok parlamenti felszólalásainak szentimentje között a 2014 és 2023 közötti időszakban?

- **H1** (kormány–ellenzék): A kormánypártok (Fidesz, KDNP) felszólalásainak átlagos szentimentje szignifikánsan magasabb, mint az ellenzéki pártoké.
- **H2** (kormány–ellenzék): Az ellenzéki pártok szentimentje időben nagyobb ingadozást mutat, mint a kormánypártoké.
- **H3** (bal–jobb): A jobboldali pártok felszólalásainak átlagos szentimentje szignifikánsan magasabb, mint a baloldali pártoké.
- **H4** (választási ciklus): A választásokat megelőző hat hónapban a felszólalások szentimentje negatívabb, mint a választási ciklus többi részében.

## Fő eredmények

| Hipotézis | Az ülés elnökének megszólalásai nélkül | Az ülés elnökének megszólalásaival |
|---|---|---|
| H1: a kormánypártok szentimentje magasabb | teljesül (különbség 0,95; Hedges g = 1,15) | teljesül (különbség 0,83; g = 1,04) |
| H2: az ellenzék jobban ingadozik | nem teljesül, ellentétes irányban szignifikáns (szórásarány 0,63) | formálisan teljesül (szórásarány 3,31), de ezt az ülés elnökének felszólalásai okozzák |
| H3: a jobboldal szentimentje magasabb | teljesül (különbség 0,70; g = 0,79) | teljesül (különbség 0,73; g = 0,87) |
| H4: a választások előtt negatívabb | nem teljesül (+0,05; g = 0,06) | nem teljesül (+0,04; g = 0,04) |

A bal–jobb különbség (H3) nagyrészt a kormányzati szerepből adódik. A jobboldali ellenzéki pártok (Jobbik, Mi Hazánk stb.) átlaga 2,12, ez gyakorlatilag megegyezik a baloldali pártokéval (2,08). A kormánypártok átlaga ezzel szemben 3,04 (az ülés elnökének megszólalásai nélkül).

## Az ülés elnökének felszólalásai

A ParlaMint `chair` szerepkódja „az ülés elnöke”: az ülést vezető elnök vagy alelnök (a dolgozatban: ülésvezetői felszólalások). Ezek a felszólalások eljárási jellegűek, például a szómegadás vagy a napirendi pontok bejelentése. Az elemzett 108 004 felszólalás 51,5%-a ilyen. Mivel torzítják az átlagokat, az ingadozást és a témamegoszlást, minden elemzés két adathalmazon fut:

- `adatkeret_teljes`: minden elemzett felszólalás (108 004)
- `adatkeret`: az ülés elnökének megszólalásai nélkül (52 331)

A szűrés felszólalásonként történik, mert ugyanaz a személy más felszólalásaiban rendes képviselőként is megszólalhat.

## A repó tartalma

```
.
├── kod.ipynb           # a teljes elemzés
├── abrak_javitott/     # a dolgozat 2–9. ábrája
├── README-HU.ana.md    # a ParlaMint-HU korpusz eredeti leírása
├── .gitignore          # a korpusz és a helyi környezet kizárása
└── README.md
```

## A notebook felépítése (`kod.ipynb`)

A fejezetek sorrendje és a hipotézisek számozása megegyezik a dolgozatéval.

| Fejezet | Tartalom | Hipotézis | A dolgozatban |
|---|---|---|---|
| 1. | Adatelőkészítés: pártok, képviselők és pártváltások beolvasása, felszólalásonként egy sort tartalmazó adatkeret (dátum, beszélő, párt, politikai pólus, szentiment, szószám, lexikális diverzitás, szerep, téma) | – | IV.1, IV.2.1 |
| 2. | Feltáró elemzés: a havi átlagos szentiment alakulása oldalak és pártok szerint | – | IV.2.2–IV.2.3 |
| 3. | Kormánypártok és ellenzék: egyoldalú Welch- és Mann–Whitney-próba évenkénti bontással (H1); a havi átlagok ingadozása, Brown–Forsythe-próba és kiegyenlített újramintavétel (H2) | H1, H2 | IV.2.6 |
| 4. | Bal- és jobboldal: Welch-féle t-próba és Hedges g, egyutas ANOVA a pártok között, varianciák | H3 | IV.2.2, IV.2.7 |
| 5. | A 2018-as és a 2022-es választás előtti hat hónap összevetése a ciklus többi részével, ciklusonként és csoportonként is | H4 | IV.2.8 |
| 6. | Válasz a K1-re: a négy hipotézis döntése egy táblázatban | H1–H4 | IV.2.9 |
| 7. | Kiegészítő elemzések: kulcsszereplők és szélsőséges hangok, témaelemzés | – | IV.2.1, IV.2.4–IV.2.5 |

A cellák egymásra épülnek, ezért a notebookot elejétől sorban kell lefuttatni (Restart & Run All).

## Módszertani megjegyzések

- Egy felszólalás szentimentje a mondataihoz tartozó szentimentértékek átlaga, egy 0–5 közötti skálán. A mondatszintű értékeket a ParlaMint a `classla/xlm-r-parlasent` osztályozóval állította elő.
- Az elemzésbe azok a felszólalások kerülnek, amelyekhez azonosítható pártaffiliáció, legalább egy szentimentérték és legalább egy token tartozik.
- **Kormány–ellenzék besorolás:** a Fidesz és a KDNP kormánypárt, minden más párt ellenzéki. A nemzetiségi szószóló (MNOÖ) egyik csoportba sem kerül.
- **Bal–jobb besorolás:** a ParlaMint Wikipédia-alapú, tizenhárom fokú orientációs kódjain alapul, három kategóriában (baloldal, jobboldal, közép/egyéb). A közép/egyéb pártok egyik pólusba sem kerülnek.
- **Próbák:** H1 és H4 esetén egyoldalú Welch- és Mann–Whitney-próba, a H4 bontásainál egyoldalú Welch-próba. H3 esetén kétoldalú Welch-próba, az irányt az átlagkülönbség előjele adja. H2 esetén Brown–Forsythe-próba és 500 ismétléses kiegyenlített újramintavétel.
- A szignifikanciaszint 0,05. A próbák egyszerűsítő feltevésként a felszólalásokat egymástól függetlennek tekintik, a részpróbákra nem alkalmazunk többszörös összehasonlítási korrekciót. A nagy elemszám miatt az értelmezés elsősorban a hatásméretre (Hedges g) támaszkodik.

## Futtatás

### Követelmények

- Python 3.9 vagy újabb, Jupyter Notebook vagy JupyterLab
- Csomagok: `pandas`, `numpy`, `scipy`, `matplotlib`, `seaborn`

### Adatok

A korpusz mérete miatt az adatok nem részei a repónak. A ParlaMint-HU 5.0 nyelvileg annotált változata a CLARIN.SI repozitóriumban, a ParlaMint.ana 5.0 kiadás részeként érhető el: <http://hdl.handle.net/11356/2005>. Innen a magyar alkorpusz (`ParlaMint-HU.TEI.ana`) letöltése elég. A kicsomagolt `ParlaMint-HU.TEI.ana` mappának a notebook mellett kell lennie:

```
.
├── kod.ipynb
└── ParlaMint-HU.TEI.ana/
    ├── ParlaMint-HU-listOrg.xml
    ├── ParlaMint-HU-listPerson.xml
    ├── ParlaMint-taxonomy-topic.xml
    ├── 2014/
    │   └── ParlaMint-HU_2014-...ana.xml
    ├── ...
    └── 2023/
```

A korpusz jellemzői: 2014.05.06 – 2023.07.31, 116 346 felszólalás, 1 730 955 mondat, 36 293 453 token.

A notebook az ülésnapok `.ana.xml` fájljait az alkönyvtárakban is megkeresi. A teljes korpusz (kb. 8 GB XML) beolvasása gépenként 10–15 percig is eltarthat, a többi cella együtt kb. egy perc alatt lefut.

## Hivatkozás

Az adatállomány:

> Erjavec, T., Kopp, M., Kuzman Pungeršek, T., Ljubešić, N., Ogrodniczuk, M. és mtsai (2025). *Linguistically annotated multilingual comparable corpora of parliamentary debates ParlaMint.ana 5.0* [Adatállomány]. CLARIN ERIC. http://hdl.handle.net/11356/2005

A ParlaMint projekt leírása:

> Erjavec, T., Ogrodniczuk, M., Osenova, P. és mtsai (2023). The ParlaMint corpora of parliamentary proceedings. *Language Resources and Evaluation*, 57(1), 415–448.

## Licenc

A ParlaMint.ana 5.0 korpusz a [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) licenc alatt érhető el.
