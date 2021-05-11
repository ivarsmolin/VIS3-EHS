# E-hääletamise tulemus

E-hääletamise lõppedes genereerib EHS e-hääletamise tulemuste faili (hääletamistulemuse fail). Fail laetakse VIS3-e (valimistulemuse moodulisse TUL).

## Protseduur

Fail edastatakse inim-masin-protseduuriga:

1) EHS operaator laeb JSON-faili EHS-st alla;

2) EHS operaator allkirjastab faili digitaalselt väljaspool EHS-i ja annab selle VIS peakasutajale;

3) VIS peakasutaja laeb digikonteinerist välja võetud faili VIS3-i.

Fail on JSON-formaadis.

## Edastatav fail

Faili struktuur (JSON-skeem): [revoke.schema](revoke.schema)

Faili näited (JSON):

- [revoke_EP.json](revoke_EP.json)
- [revoke_KOV.json](revoke_KOV.json)
- [revoke_RH.json](revoke_RH.json)
- [revoke_RK.json](revoke_RK.json)
