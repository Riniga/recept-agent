# 03 – Detaljerad lösningsskiss

Det här dokumentet beskriver en rekommenderad, liten teknisk grund. Det är inte ett
beslut om exakt bibliotek, leverantör, lagringsformat eller implementation.

## Rekommenderad teknik

| Behov | Minimal rekommendation | Avsiktlig avgränsning |
|---|---|---|
| Webbapp och backend | Python med FastAPI | En applikation och en kodbas |
| Webbsidor | Serverrenderade HTML-mallar, exempelvis Jinja2 | Ingen separat React-/Vite-byggkedja |
| Formulär och små UI-detaljer | HTML först, lite vanlig JavaScript vid behov | Inget separat frontendramverk |
| Receptvalidering | Pydantic-modeller | Ett internt validerat receptobjekt |
| Lokal sökning och status | SQLite, FTS5 när fritextsökning behövs | Ingen separat databas- eller söktjänst |
| Receptfiler | OneDrive via Microsoft Graph | Ingen egen filserver som konkurrerar med OneDrive |
| Bildtolkning och chatt | En vald multimodal modell via en tunn integrationsmodul | Ingen generell flerleverantörsplattform |
| AI-åtkomst till recept | Typade MCP-verktyg i samma kodbas | Ingen separat MCP-mikrotjänst |
| Körning | En lokal Python-process för demon | Docker och molndrift är inte förutsättningar |

Tekniken kan ändras om ett verifierat krav motiverar det. Håll gränserna enkla så att
modell, OneDrive och receptlogik går att testa utan riktiga externa anrop.

## Föreslagen kodstruktur

```text
recept-agent/
  recept_agent/
    main.py
    routes/
    templates/
    static/
    models/
    services/
      recipes.py
      extraction.py
      storage.py
      search.py
      chat.py
    integrations/
      ai_provider.py
      onedrive.py
      mcp_tools.py
  tests/
  testdata/
  docs/
  pyproject.toml
  .env.example
  pyproject.toml
```

Strukturen är vägledande. Undvik tomma abstraktionslager och dela bara upp kod när
ansvaret faktiskt kräver det. `services/` ska innehålla användningsfall och
`integrations/` endast gränser mot externa system.

## Receptdata och lagring

Applikationen använder ett validerat internt receptobjekt med åtminstone:

- stabilt recept-ID och titel
- kategori och taggar när de finns
- portioner och tidsuppgifter när de går att utläsa
- ingredienser med namn, mängd, enhet och eventuell råtext
- instruktioner, anteckningar och osäkerhetsmarkeringar
- referens till originalbild/PDF och lagrad fil
- skapad/uppdaterad-tid samt versions- eller ETag-information där tillgängligt

Internt JSON-format är ett utbyte mellan komponenter, inte ett beslut om permanent
lagring. Markdown och JSON i `testdata/reference-output/` är exempel. Välj slutligt
filformat efter granskning av testfall och hur användaren ska läsa/redigera recepten.
Undvik att behöva hålla två auktoritativa versioner synkroniserade.

OneDrive behåller de godkända recepten och originalen. SQLite innehåller bara den status
och det index som behövs för jobbhantering och sökning; innehållet ska kunna byggas om
från OneDrive. Lagra inte hemligheter eller fler kopior av recept än nödvändigt.

## Receptflöde

1. Begränsa filstorlek och tillåtna format; kontrollera innehållstyp server-side.
2. Bevara originalet och skapa ett spårbart arbetsobjekt.
3. Skicka bild/PDF till vald modell och begär utdata enligt receptmodellen.
4. Validera modellsvaret. Markera okända fält och osäkerhet; fabricera inte uppgifter.
5. Visa original och tolkning så användaren kan rätta innan godkännande.
6. Spara bara efter uttryckligt godkännande.
7. Hantera OneDrive-skrivning och indexuppdatering med explicita statusar. Om ett steg
   misslyckas visas vad som sparades och vad som återstår; visa inte falskt "klart".

Första flödet kan vara ett dokument i taget. Bakgrundskö, parallellbearbetning och
automatisk batchimport införs bara om observerad svarstid eller volym kräver det.

## OneDrive och sökning

- Använd delegerad Microsoft-åtkomst med minsta nödvändiga behörigheter.
- Kontrollera identitet och åtkomst på servern; skicka aldrig åtkomsttoken till klientkod.
- Identifiera filer med stabilt Graph item-ID och använd ETag vid skrivning när det stöds.
- Erbjud en uttrycklig omläsning/synkronisering av biblioteket; hantera ändringar,
  borttagningar och ogiltiga filer synligt.
- Börja med enkla filter och SQLite/FTS5 om fritextsökning behövs.
- Bygg om index från OneDrive-filer i stället för att behandla indexet som original.
- Lägg inte till delta-token, återförsökssystem eller separat söktjänst innan behovet
  har verifierats.

## AI-assistent och MCP

- AI får åtkomst till recept genom typade funktioner, aldrig rå SQL eller godtyckligt
  filsystem.
- Håll MCP-verktygen få och begränsade: sök/lista recept, hämta ett recept och vid behov
  hämta enkel statistik.
- Anropa samma recepttjänst som webbgränssnittet; duplicera inte affärslogik i MCP.
- Behandla recepttext och uppladdade dokument som otillförlitlig data, inte som
  instruktioner till modellen.
- Svara med källor och skilj verifierat receptinnehåll från genererade variationer.
- En genererad variant är ett förslag; skrivning kräver separat uttryckligt
  användargodkännande.

## Säkerhet och fel

- Håll modellnycklar och Graph-token server-side, utanför repo och loggar.
- Logga status, korrelations-ID och tekniska fel utan rå recepttext, bilder eller token.
- Validera uppladdningar och begränsa storlek/format; behandla parsersvar som obetrodda.
- Visa autentiseringsfel, modellfel, throttling, lagringsfel och konflikter tydligt.
- MCP och webbgränssnitt ska använda samma autentiserade användarkontext och
  behörighetskontroller.

## Testning

- Enhetstesta receptvalidering, normalisering, osäkerhetsmarkering och formatering.
- Använd mockad AI och mockad OneDrive i automatiserade tester; tester ska inte kräva
  API-nycklar eller privata konton.
- Använd bilderna i `testdata/input/` som indata och koppla granskade referensresultat
  till de bilder som faktiskt ska utvärderas.
- `testdata/reference-output/` är inte facit förrän människor har granskat och godkänt
  kopplingen mellan bild och förväntat resultat.
- Testa felvägar: avvisad/ogiltig fil, osäkert modellresultat, misslyckad lagring,
  åtkomstfel och konflikt.
- Verifiera hela användarflödet i webbläsaren, inklusive rättning och godkännande.

## Körning

Lösningen ska kunna startas lokalt med en dokumenterad Python-miljö och konfiguration.
En separat frontendserver, Redis, containerorkestrering eller molnmiljö är inte
nödvändiga för den tekniska målbilden. Konkreta miljövariabler och autentiseringsflöde
bestäms när valda tjänster och kontotyp är kända.
