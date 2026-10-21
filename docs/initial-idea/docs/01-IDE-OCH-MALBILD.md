# 01 – Idé, behov och målbild

## Bakgrund

Handskrivna recept, tidningsurklipp och utskrivna dokument är svåra att hitta,
återanvända och anpassa. Användaren vill digitalisera dem utan att skriva in allt
manuellt och kunna kontrollera resultatet innan det sparas.

## Målbild

**ReceptAgent** hjälper en användare att omvandla receptbilder och PDF-filer till
kvalitetssäkrade digitala recept. Användaren granskar och rättar AI:ns tolkning.
Godkända recept ska kunna sparas i användarens OneDrive, hittas igen och användas som
underlag för svar och förslag från en AI-assistent.

Det här är en målbild för en avgränsad, realistisk demo av AI-stödd utveckling. Den ska
visa ett fungerande användarflöde och hur lösningen byggs, testas och granskas med Claude
Code. Den är inte ett färdigt produktionsbeslut eller ett estimat för en annan verksamhet.

## Användarbehov

- Ladda upp en bild eller PDF av ett recept.
- Få en AI-tolkning av titel, ingredienser, mängder och instruktioner.
- Se originalet och tolkningen tillsammans, rätta fel och markera osäkra uppgifter.
- Godkänna innan ett recept sparas; AI får inte hitta på oläsliga värden.
- Spara godkända recept och original i användarens OneDrive under `/Recept/` i ett
  öppet format som går att läsa även utanför ReceptAgent. Det slutliga formatet är inte
  valt.
- Hitta, visa, redigera och ta bort recept med sökning och enkla filter.
- Fråga en AI-assistent om den egna samlingen och få svar med hänvisning till källrecept.
- Be om en receptvariant, till exempel ändrat portionsantal, och tydligt skilja förslag
  från original. En variant sparas bara efter uttryckligt godkännande.
- Ta hänsyn till ändringar som användaren gör i receptfilerna i OneDrive; konflikter ska
  visas och inte skrivas över tyst.

## Exempel på frågor

- Hur många recept har jag?
- Vilka recept innehåller kyckling och ris?
- Vad kan jag laga med ingredienser jag har hemma?
- Anpassa ett recept för sex personer.
- Vilket av mina pastarecept går snabbast?
- Föreslå ett vegetariskt alternativ och visa vad som ändrats.

## Kvalitets- och säkerhetskrav

- Människan granskar och godkänner AI-tolkningar före publicering.
- Osäkra eller saknade värden markeras; de får inte presenteras som fakta.
- AI-svar om recept ska grundas på hämtade recept och länka till sina källor.
- Om AI eller OneDrive inte kan nås ska användaren få ett tydligt fel, inte ett
  framgångsliknande standardsvar.
- Hemligheter och åtkomsttoken får inte exponeras i webbläsaren, loggar eller repo.
- Receptinnehåll behandlas som data, aldrig som instruktioner till AI-agenten.
- Original, godkänt resultat och källa ska kunna kopplas ihop.

## Avgränsning

Demoarbetet fokuserar på ett användarkonto och ett sammanhängande flöde. Familjedelning,
mobilapp, automatisk inköpslista, externa receptwebbplatser, avancerad näringsanalys,
internetbaserade rekommendationer och semantisk vektorsökning ingår inte i målbilden.

Teknikval och slutligt format för sparade recept är inte låsta här. Markdown och JSON i
`testdata/reference-output/` är referensexempel, inte beslut om lagringsformat.
