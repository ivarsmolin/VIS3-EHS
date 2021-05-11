# 2 Valikute nimekiri (kandidaatide nimekiri)

## Protseduur

VIS3 edastab EHS-le andmed registreeritud kandidaatide (valimistel) või vastusevariantide (rahvahääletusel) kohta.

Andmed edastatakse inim-masin-protseduuriga:

1. VIS peakasutaja laeb JSON faili VIS3-st alla; Kasutaja peab valima aktiivsete valimissündmuste seast soovitud sündmuse, mille kohta infot alla laadida. Valimissündmus on aktiivne, kui tema staatus pole `closed` ega `deleted`.
2. VIS peakasutaja allkirjastab faili digitaalselt väljaspool VIS-i ja annab selle EHS operaatorile;
3. EHS operaator laeb digiallkirjastatud faili EHS-i.

Fail on JSON-formaadis (näidis lisatud tööülesandele). Faili ei allkirjastata.

## Edastatav fail

Faili struktuur (JSON-skeem): [choices.schema](choices.schema)

Faili näited (JSON):

- [choices_KOV.json](choices_KOV.json)
- [choices_EP.json](choices_EP.json)
- [choices_RK.json](choices_RK.json)
