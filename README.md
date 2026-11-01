# Hangnem a parlamentben: a magyar parlamenti pártok felszólalásainak szentimentelemzése (2014–2023)

Ez a repó egy TDK-dolgozat (Budapesti Corvinus Egyetem, 2026–2027. őszi félév) elemzőkódját tartalmazza. Az elemzés a CLARIN ParlaMint projekt magyar korpuszán (ParlaMint-HU v5.0) azt vizsgálja, hogy eltér-e a kormánypárti és az ellenzéki, illetve a bal- és jobboldali pártok parlamenti felszólalásainak szentimentje.

## Kutatási kérdés és hipotézisek
K1: kimutatható-e szignifikáns különbség a kormánypárti és az ellenzéki, illetve a bal- és jobboldali pártok parlamenti felszólalásainak szentimentje között a 2014 és 2023 közötti időszakban? 

H1: A kormánypártok (Fidesz, KDNP) felszólalásainak átlagos szentimentje szignifikánsan magasabb, mint az ellenzéki pártoké.

H2: Az ellenzéki pártok szentimentje időben nagyobb ingadozást mutat, mint a kormánypártoké. 

H3: A jobboldali pártok felszólalásainak átlagos szentimentje szignifikánsan magasabb, mint a baloldali pártoké.

H4: A választásokat megelőző hat hónapban a felszólalások szentimentje negatívabb, mint a választási ciklus többi részében.

## Fő eredmények

| Hipotézis | Ülésvezetői felszólalások nélkül | Ülésvezetői felszólalásokkal |
|---|---|---|
| H1: a kormánypártok szentimentje magasabb | teljesül (különbség 0,95; Hedges g = 1,15) | teljesül (különbség 0,83; g = 1,04) |
| H2: az ellenzék jobban ingadozik | nem teljesül, ellentétes irányban szignifikáns (szórásarány 0,63) | formálisan teljesül (szórásarány 3,31), de ezt az ülésvezetői felszólalások okozzák |
| H3: a választások előtt negatívabb | nem teljesül (+0,05; g = 0,06) | nem teljesül (+0,04; g = 0,04) |
| K1, bal–jobb különbség | szignifikáns (0,70; g = 0,79) | szignifikáns (0,73; g = 0,87) |

A bal–jobb különbség nagyrészt a kormányzati szerepből adódik. A jobboldali ellenzéki pártok (Jobbik, Mi Hazánk stb.) átlaga 2,12, ez gyakorlatilag megegyezik a baloldali pártokéval (2,08). A kormánypártok átlaga ezzel szemben 3,04.

## Az elnöki felszólalások kezelése

Az ülést vezető elnök vagy alelnök (`#chair`) felszólalásai eljárási jellegűek. Ilyen a szómegadás vagy a napirendi pontok bejelentése. A korpusz 108 004 felszólalásának 51,5%-a ilyen. Mivel ezek torzítják az átlagokat, az ingadozást és a témamegoszlást, minden elemzés két adathalmazon fut:

- `adatkeret_teljes`: minden felszólalás (108 004)
- `adatkeret`: az ülésvezetői felszólalások nélkül (52 331)

A szűrés felszólalásonként történik, mert ugyanaz a személy más felszólalásaiban rendes képviselőként is megszólalhat.

## A notebook felépítése (`kod.ipynb`)

1. **Adatelőkészítés:** beolvassa a pártok, a képviselők és a pártváltások adatait. Felszólalásonként egy sort tartalmazó adatkeretet épít (dátum, beszélő, párt, politikai pólus, szentiment, szószám, lexikális diverzitás, szerep, téma).
2. **Feltáró elemzés és a bal–jobb összevetés:** a havi átlagos szentiment időbeli alakulása, Welch-féle t-próba és hatásméret a bal- és jobboldal között, egyutas ANOVA a pártok között, valamint varianciák.
3. **Hipotézisvizsgálat (H1, H2):**
   - H1: kormánypártok és ellenzék, egyoldalú Welch- és Mann–Whitney-próba, évenkénti bontással.
   - H2: a havi átlagok ingadozása. Ehhez Brown–Forsythe-próba és a mintanagyságot kiegyenlítő újramintavétel tartozik.
4. **H3:** a 2018-as és a 2022-es választás előtti hat hónap összevetése a ciklus többi részével, ciklusonként és csoportonként is.
5. **Válasz a K1-re:** összesítő táblázat.
6. **Kiegészítő elemzések:** kulcsszereplők és szélsőséges hangok, témaelemzés.

A cellák egymásra épülnek, ezért a notebookot elejétől sorban kell lefuttatni.

## Módszertani megjegyzések

- Egy felszólalás szentimentje a mondataihoz tartozó szentimentértékek átlaga, egy 0–5 közötti skálán.
- **Kormány–ellenzék besorolás:** a Fidesz és a KDNP kormánypárt, minden más párt ellenzéki. A nemzetiségi szószóló (MNOÖ) egyik csoportba sem kerül.
- **Bal–jobb besorolás:** a ParlaMint pártokhoz rendelt orientációs kódjain alapul.
- A szignifikanciaszint 0,05. A próbák egyszerűsítő feltevésként a felszólalásokat egymástól függetlennek tekintik. A nagy elemszám miatt az értelmezés elsősorban a hatásméretre (Hedges g) támaszkodik.

## Futtatás

### Követelmények

- Python 3.9 vagy újabb, Jupyter Notebook vagy JupyterLab
- Csomagok: `pandas`, `numpy`, `scipy`, `matplotlib`, `seaborn`

### Adatok

A korpusz mérete miatt az adatok nem részei a repónak. A ParlaMint-HU nyelvileg annotált (`.ana`) változata a [ParlaMint projekt oldaláról](https://www.clarin.eu/parlamint) érhető el. A kicsomagolt `ParlaMint-HU.TEI.ana` mappának a notebook mellett kell lennie:

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

A notebook az ülésnapok `.ana.xml` fájljait az alkönyvtárakban is megkeresi. A teljes korpusz beolvasása néhány percig tarthat.

## Hivatkozás

Az adatok forrása: Erjavec, T., Ogrodniczuk, M., Osenova, P. és mtsai (2023). The ParlaMint corpora of parliamentary proceedings. *Language Resources and Evaluation*, 57(1), 415–448.
