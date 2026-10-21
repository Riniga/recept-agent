# Testdata

Här finns återanvändbart underlag för att verifiera och utvärdera hur ReceptAgent läser
receptbilder och omvandlar innehållet till strukturerade recept.

## Innehåll

- `input/recept/` – bilder som används som indata i testfall. Behåll originalfilerna
  oförändrade så att körningar kan jämföras.
- `reference-output/recipe.md` och `reference-output/recipe.json` – referensexempel som
  visar möjliga receptutdata i Markdown respektive JSON.

Referensexemplen är ännu inte godkända facit för bilderna. Granska och koppla ett
referensresultat till ett testfall innan det används för att mäta extraktionens
korrekthet. Markera osäkra eller oläsliga uppgifter i stället för att gissa.

Den här katalogstrukturen bestämmer inte vilket format lösningen slutligen ska spara
recept i. Markdown och JSON finns här som referensformat; beslut om lagringsformat fattas
separat.
