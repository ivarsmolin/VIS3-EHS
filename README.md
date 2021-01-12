# Valijate nimekirja edastamise spetsifikatsioon

## 1. Annotatsioon

Spetsifikatsioon määratleb Valimiste infosüsteemi (VIS3) ja e-hääletamise
süsteemi (EHS) vahelise liidese, mille kaudu VIS3 edastab EHS-le valijate
algnimekirja ja valijate nimekirja muudatusi ehk muudatusnimekirju.

Spetsifikatsioon on avalik. Spetsifikatsioon ei käsitle VIS3 ega EHS
konfidentsiaalset siseehitust ega liidese konfidentsiaalseid elemente.

## 2. Sõnumivahetus valijanimekirjade edastamiseks

### 2.1 Valijate algnimekirja edastamine

### 2.2 Valijate muudatusnimekirja edastamine

### 2.3 Nimekirjade loendi edastamine

## 3. Nimekirja andmevorming

Nimekirja andmevorming on sama nii valijate algnimekirja kui muudatusnimekirjade
jaoks.

Nimekiri esitatakse UTF-8-NOBOM vormingus tekstifailina. Andmestruktuuride
eraldajaks kasutatakse reavahetusmärki `LF` (ASCII-kood `0x0A`).
Andmestruktuuride väljade eraldajaks kasutatakse tabeldusmärki `TAB` (ASCII-kood
`0x09`).

Nimekiri koosneb päiseridadest ja kirjetest.

Päiseridade sisu on järgmine:

1.  `version` - andmestruktuuri versiooninumber, mille pikkus on piiratud 2
    tähemärgiga. Spetsifikatsioonile vastava nimekirja korral on selle välja
    väärtus 2.
2.  `election_identifier` - valimissündmuse identifikaator, mille pikkus on
    piiratud 28 tähemärgiga ASCII kooditabelist. Nimekirja rakendamine toimub
    ainult vastava identifikaatoriga valimissündmuse kontekstis.
3.  `changeset` - nimekirja järjekorranumber. Rangelt kasvav number (0, 1, 2,
    ...), mis defineerib nimekirjade rakendamise järjekorra. Algnimekirja
    järjekorranumbriks on 0.

Kirje koosneb väljadest, mille sisu on järgmine:

1.  `person_code` - isikukood on valija unikaalne identifikaator, mille alusel
    EHS tuvastab isiku hääleõiguse ja ringkonnakuuluvuse.
2.  `voter_name` - valija nimi, informaalne väli, otsuste tegemisel ei kasutata.
3.  `action` - kirjega seotud tegevus. `lisamine` tähendab uue valija lisamist
    ja `kustutamine` olemasoleva valija eemaldamist.
4.  `kov_code` - haldusüksus, kuhu valija kuulub. Haldusüksuse
    identifitseerimiseks kasutatakse kohaliku omavalitsuse EHAK-koodi, Tallinna
    korral linnaosa EHAK-koodi, alaliselt välisriigis elava valija korral
    kasutatakse väärtust `FOREIGN`.
5.  `electoral_district_id` - valimisringkonna number identifitseerib
    valimisringkonna, kus valija hääletab. KOV valimiste korral kehtib
    identifikaator haldusüksuse sees. RK, EP ja RH valimiste korral on
    identifikaator haldusüksuste ülene.


Andmevormingu formaalne kirjeldus Backus-Naur notatsioonis:

``` {.sourceCode .bnf}
# Päiseridade definitsioonid
version = "2"
election_identifier = 1*28CHAR
changeset = DIGIT

# Kirje väljade definitsioonid
person_code = 11DIGIT
voter_name = 1*100UTF-8-CHAR
action = "lisamine" | "kustutamine"
kov_code = 4DIGIT | "FOREIGN"
electoral_district_id = 1*10DIGIT

# Kirje definitsioon
voter = person_code TAB voter_name TAB action TAB kov_code TAB electoral_district_id LF

# Nimekirja definitsioon
voter_list = version LF election_identifier LF changeset LF *voter
```

### 3.1 Nimekirja tõlgendamine

Nimekirju töötlev rakendus lähtub järgmistest reeglitest:

1.  Nimekiri rakendatakse tervikuna.
2.  Nimekirja rakendamisele eelnevad vormingu ja kooskõlalisuse kontrollid,
    vigaseid nimekirju ei rakendata.
3.  Rakendus kontrollib nimekirja versiooni.
4.  Rakendus kontrollib valimissündmuse identifikaatorit.
5.  Rakendus kontrollib nimekirja järjekorranumbrit.
6.  Rakendus kontrollib nimekirja kõigi kirjete kooskõlalisust oma andmebaasiga.
    1.  Kooskõlalisust kontrollitakse kirjete esinemisjärjekorras.
    2.  Kui tegevus on `lisamine`, siis ei tohi vastava isikukoodiga kirjet
        rakenduse andmebaasis olla.
    3.  Kui tegevus on `eemaldamine`, siis peab vastava isikukoodiga kirje
        rakenduse andmebaasis olema ning tema `kov_code` ja
        `electoral_district_id` väljade väärtused peavad juba registreeritutega
        ühtima.

Kui valijaga soetud andmeid on vaja muuta, näiteks valija liigub ühest
haldusüksusest või valimisringkonnast teise, siis kantakse valijate nimekirja
muudatuste hulka üks kustutamise kirje, millega valija oma eelmisest üksusest
kustutatakse ja üks lisamise kirje, millega valija uues üksuses valijate
nimekirja lisatakse. Ka kõik teised muudatused valija andmetes - näiteks
nimemuutus - toimuvad läbi vana kirje eemaldamise ja uue lisamise.

Valijate algnimekirjas on ainult lisamiskirjed, iga valija kohta maksimaalselt
üks kirje. Muudatusnimekirjas peavad ühe valija kohta käivad kirjed olema
nimekirjas loogilises järjestuses ning liiasuseta. Ehk kustutamine enne lisamist
ning maksimaalselt üks kustutamise-lisamise paar.

On võimalik, et valimise ajal muutub valija isikukood. Sellisel juhul lisatakse
nimekirja vana isikukoodiga kustutamise kirje ning uue isikukoodiga lisamise
kirje. Täiendav korduvhääletamise kontroll ei ole selle liidese skoobis ja
teostakse vastavate infosüsteemide operaatorite poolt eraldi.


### 3.2 Näited nimekirjadest

Valijate algnimekiri, 0.

``` {.sourceCode .bnf}
2<LF>
RK2051<LF>
0<LF>
10000000001<TAB>NIMI NIMESTE1<TAB>lisamine<TAB>0482<TAB>3<LF>
20000000002<TAB>NIMI NIMESTE2<TAB>lisamine<TAB>0514<TAB>7<LF>
30000000003<TAB>NIMI NIMESTE3<TAB>lisamine<TAB>0735<TAB>7<LF>
40000000004<TAB>NIMI NIMESTE4<TAB>lisamine<TAB>0482<TAB>3<LF>
50000000005<TAB>NIMI NIMESTE5<TAB>lisamine<TAB>0339<TAB>1<LF>
60000000006<TAB>NIMI NIMESTE6<TAB>lisamine<TAB>0296<TAB>4<LF>
70000000007<TAB>NIMI NIMESTE7<TAB>lisamine<TAB>FOREIGN<TAB>11<LF>
80000000008<TAB>NIMI NIMESTE8<TAB>lisamine<TAB>0793<TAB>10<LF>
90000000009<TAB>NIMI NIMESTE9<TAB>lisamine<TAB>FOREIGN<TAB>3<LF>
```

Muudatusnimekiri, 1:

1.  Muutub valija 20000000002 haldusüksus.
2.  Valija 30000000003 kaotab hääleõiguse.
3.  Valijale 11000000011 antakse hääleõigus.


``` {.sourceCode .bnf}
2<LF>
RK2051<LF>
1<LF>
20000000002<TAB>NIMI NIMESTE2<TAB>kustutamine<TAB>0514<TAB>7<LF>
20000000002<TAB>NIMI NIMESTE2<TAB>lisamine<TAB>0735<TAB>7<LF>
30000000003<TAB>NIMI NIMESTE3<TAB>kustutamine<TAB>0735<TAB>7<LF>
11000000011<TAB>NIMI NIMESTE11<TAB>lisamine<TAB>0653<TAB>4<LF>
```

Muudatusnimekiri, 2:

1.  Muutub valija 20000000002 nimi.
2.  Valija 60000000006 kaotab hääleõiguse.

``` {.sourceCode .bnf}
2<LF>
RK2051<LF>
2<LF>
20000000002<TAB>NIMI NIMESTE2<TAB>kustutamine<TAB>0735<TAB>7<LF>
20000000002<TAB>UUSNIMI NIMESTE2<TAB>lisamine<TAB>0735<TAB>7<LF>
60000000006<TAB>NIMI NIMESTE6<TAB>kustutamine<TAB>0296<TAB>4<LF>
```

## 4. Nimekirja signeerimine

## 5. Transpordiprotokoll

## 6. Näited

## 7. Viited

["DigiSign" - lihtne protokoll edastatavate failide allkirjastamiseks, koos teostusnäitega](https://github.com/e-gov/DigiSign)
