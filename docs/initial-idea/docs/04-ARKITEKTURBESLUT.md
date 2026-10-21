# 04 – Arkitekturprinciper och öppna beslut

Det här dokumentet skiljer mellan användarbehov som ska bevaras och tekniska
rekommendationer som ännu inte är fastställda beslut.

## Krav som målbilden ska bevara

| Område | Krav |
|---|---|
| Inläsning | Bild och PDF ska kunna tolkas till receptuppgifter |
| Kvalitet | Användaren ska kunna granska och rätta innan sparning |
| Osäkerhet | Okända eller oläsliga värden ska markeras, inte gissas |
| Receptbibliotek | Recept ska kunna sparas, läsas och sökas |
| Ägarskap | OneDrive är avsedd plats för användarens recept och original |
| Assistent | Svar ska grundas på användarens recept och ange källor |
| MCP | Assistentens åtkomst ska gå via begränsade, typade verktyg |
| Ändringar | OneDrive-ändringar och konflikter ska hanteras synligt |
| Varianter | Förslag får inte ändra eller sparas över original utan godkännande |
| Skydd | Identitet, hemligheter, fel och uppladdat innehåll ska hanteras säkert |

## Rekommenderad teknisk riktning

| Område | Rekommendation | Varför |
|---|---|---|
| Applikation | En Python-webbapp, exempelvis FastAPI | En kodbas och enkel lokal körning |
| Gränssnitt | Serverrenderad HTML, JavaScript bara vid behov | Undviker separat frontendramverk och byggkedja |
| Arkitektur | Modulär monolit med få tydliga gränser | Testbar utan distribuerade tjänster |
| Lokal data | SQLite som status-/sökindex, återskapbart från OneDrive | Begränsar dubbel lagring och drift |
| AI | En vald multimodal modell bakom en tunn integrationsmodul | Konkreta tester utan flerleverantörskomplexitet |
| MCP | Verktyg i samma kodbas som receptlogiken | Ingen separat MCP-tjänst eller duplicerad logik |
| Körning | Lokal process för demon | Docker/molndrift blir inte ett förhandskrav |
| Receptformat | Inte fastställt | Markdown och JSON är ännu bara referensexempel |

Rekommendationerna är en utgångspunkt för mänskligt godkännande, inte redan fattade
arkitekturbeslut.

## Öppna beslut

1. **Microsoft-identitet:** privat OneDrive eller OneDrive for Business? Detta påverkar
   appregistrering, OAuth-flöde och behörigheter.
2. **AI-tjänst:** vilken modell får behandla receptbilderna, och vilka krav gäller för
   datalagring, region, kostnad och åtkomst?
3. **Receptens filformat:** ska den beständiga formen vara Markdown, JSON eller ett annat
   enkelt öppet format? Ska ett format genereras från ett annat?
4. **Testfacit:** vilka bilder i `testdata/input/` ska ha manuellt granskade
   referensresultat, och vem godkänner dem?
5. **MCP-användning:** ska MCP främst koppla ReceptAgents egen assistent till receptlogik,
   eller ska även externa AI-klienter ansluta?
6. **Demoåtkomst:** räcker lokal körning med ett användarkonto, eller ska demon nås av
   andra? Det avgör autentiserings- och driftsättningskrav.
7. **Original och retention:** hur länge ska originalbilder bevaras och hur ska de kunna
   tas bort?
8. **Synkronisering:** räcker en användarinitierad omläsning av OneDrive-biblioteket,
   eller krävs automatisk synk?

## Risker att hantera

| Risk | Hantering |
|---|---|
| AI misstolkar handstil eller mängder | Visa original och markera osäkerhet; kräva granskning |
| Recepttext innehåller promptinjektion | Behandla innehåll som data och begränsa verktyg |
| Privat och arbets-OneDrive har olika auth-flöden | Bekräfta kontotyp innan integration låses |
| OneDrive-skrivning lyckas men indexering misslyckas | Redovisa delstatus och gör indexet återskapbart |
| Referensdata matchar inte bilderna | Granska och dokumentera varje utvärderat testfall |
| Lösningen växer till onödig infrastruktur | Kräv verifierat behov innan kö, separat tjänst eller molnkomponent tillkommer |
