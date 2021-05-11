# E-hääletamise tulemus

E-hääletamise lõppedes genereerib EHS e-hääletamise tulemuste faili (hääletamistulemuse fail). Fail laetakse VIS3-e (valimistulemuse moodulisse TUL).

## Protseduur

Fail edastatakse inim-masin-protseduuriga:

1) EHS operaator laeb JSON-faili EHS-st alla;

2) EHS operaator allkirjastab faili digitaalselt väljaspool EHS-i ja annab selle VIS peakasutajale;

3) VIS peakasutaja laeb digikonteinerist välja võetud faili VIS3-i.

Fail on JSON-formaadis.

## Edastatav fail

Faili struktuur (JSON-skeem): [results.schema](results.schema)

Faili näited (JSON):

- [results_EP.json](results_EP.json)
- [results_KOV.json](results_KOV.json)
- [results_RH.json](results_RH.json)
- [results_RK.json](results_RK.json)
