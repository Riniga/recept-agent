# 05 – Krav, verifiering och demo

Det här dokumentet samlar lösningens funktionella krav och hur demot kan verifieras.
Det anger inte MVP:er, leveranssteg eller en genomförandeordning. Sådana avgränsningar
bestäms separat när arbetet planeras.

## Funktionella krav

- [ ] Användaren kan ladda upp en receptbild eller PDF.
- [ ] Systemet visar originalet tillsammans med AI:ns tolkning.
- [ ] AI föreslår titel, ingredienser, mängder, instruktioner och tillgängliga metadata.
- [ ] Saknade och osäkra värden markeras utan att systemet hittar på ersättningar.
- [ ] Användaren kan korrigera uppgifter och uttryckligen godkänna resultatet.
- [ ] Endast godkända recept sparas i användarens OneDrive, tillsammans med en koppling
      till originalet. Receptfilerna placeras i `/Recept/` och kan läsas utanför appen.
- [ ] Användaren kan bläddra, söka, visa, redigera och ta bort sparade recept.
- [ ] AI-assistenten kan söka och läsa egna recept via begränsade MCP-verktyg.
- [ ] AI-svar visar receptkällor och skiljer källuppgifter från genererade förslag.
- [ ] En variant kan föreslås, men originalet ändras inte utan uttryckligt godkännande.
- [ ] Ändrade receptfiler kan läsas in igen; konflikter och fel visas i stället för att
      skrivas över tyst.
- [ ] Autentisering och behörighet skyddar OneDrive-data och skrivoperationer.

## Kvalitetskrav

- AI-resultat och externa svar valideras innan de används.
- Användaren får begriplig status vid uppladdning, granskning, sparning, synk och fel.
- Fel i AI, Microsoft Graph, autentisering eller indexering får inte döljas eller
  rapporteras som lyckade.
- Index och metadata ska kunna återskapas från beständiga receptfiler där det är rimligt.
- Loggar innehåller inte råa bilder, recepttext, token eller hemligheter.
- Automatiserade tester körs med mockade externa tjänster och kräver inte privata konton.
- Testfall använder verifierad indata och manuellt granskade förväntade resultat.

## Testdata

`testdata/input/recept/` innehåller bilder avsedda som indata. `testdata/reference-output/`
visar exempel på receptutdata i Markdown och JSON. De är inte automatiskt facit för
bilderna. Innan en mätning används ska testfallet ange vilken bild som utvärderas och
vilka uppgifter som har granskats och godkänts som förväntat resultat.

Filer i `testdata/` är testunderlag och ska inte ändras eller skrivas över av
applikationens körning.

## Förslag till demoscenario

1. Visa en av bilderna i testunderlaget och förklara vad som är läsbart respektive osäkert.
2. Ladda upp bilden och visa systemets extraktion.
3. Jämför tolkningen med originalet och rätta minst ett värde.
4. Godkänn receptet och visa var det sparades i OneDrive.
5. Sök efter receptet och visa vilken källa som används.
6. Be assistenten besvara en fråga om receptsamlingen via MCP och kontrollera
   källhänvisningarna.
7. Be om en variant och visa att den förblir ett förslag tills den godkänns.
8. Visa relevanta tester och hur Claude Code användes för implementation och granskning.

Demonstrationen ska vara reproducerbar och får inte vara beroende av manuella
databasändringar eller dolda specialingrepp.

## Avsiktliga avgränsningar

Ingen separat React-klient, distribuerad kö, separat söktjänst, flerleverantörsramverk,
familjedelning, mobilapp eller avancerad semantisk sökning krävs av den här målbilden.
Lägg endast till sådan teknik när ett konkret krav och en verifierbar nytta motiverar
den.
