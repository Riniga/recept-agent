# 08 – Riktlinjer för plattform och leverans

Det här dokumentet anger gemensamma ramar för verktyg, källkodshantering, CI/CD och
molnplattform. Det väljer inte lösning för enskilda komponenter. Sådana val motiveras i
projektets HLD/DLD när behoven är kända.

## Grundprinciper

- Välj det enklaste verktyg som uppfyller ett verifierat behov.
- Prioritera verktyg som redan ingår i GitHub, Azure eller projektets utvecklingsmiljö.
- Föredra textbaserad konfiguration och skript som går att läsa, granska och köra om.
- Undvik ett extra ramverk eller en lokal verktygsinstallation om samma kontroll kan
  göras med projektets befintliga kommandon eller direkt i GitHub.
- Dokumentera nya verktyg, deras syfte, installation och verifieringskommando innan de
  blir ett krav för utvecklare eller CI.
- Alla beslut ska gå att granska av en människa och kunna verifieras utan att lita på
  ett ogenomskinligt automatiserat steg.

## Källkod och versionshantering

- GitHub är projektets hem för källkod, granskning och automatisering.
- Ändringar går via pull request och granskas innan de förs in i huvudgrenen.
- Håll branchskydd och obligatoriska kontroller så små som möjligt men tillräckliga för
  att förhindra att ändringar utan godkänd CI förs vidare.
- Konfiguration, bygginstruktioner och leveransskript versionshanteras tillsammans med
  koden. Hemligheter, token och miljöspecifika värden gör det inte.

## CI

- GitHub Actions är standardplattform för kontinuerlig integration.
- CI ska köras automatiskt vid pull request och relevanta ändringar i GitHub.
- Återanvänd projektets vanliga kommandon för installation, lint, tester och bygg.
  Samma kommando ska gå att köra lokalt och i CI när det är praktiskt möjligt.
- Håll workflow-filer korta, läsbara och begränsade till kontroller som ger konkret
  kvalitets- eller säkerhetsvärde.
- Använd standard actions och befintliga runner-funktioner före egen pipelinekod.
  Pinna actions enligt organisationens säkerhetsriktlinjer.
- CI ska visa tydligt vilken kontroll som misslyckades och länka till relevant logg.
  Dölj inte fel eller rapportera lyckat resultat efter en misslyckad kontroll.
- Lägg inte till containerbaserade byggsteg eller särskilda verktyg på runnern om
  projektets faktiska behov inte kräver dem.

## CD och Azure

- Azure är målplattform för molndrift när lösningen behöver publiceras.
- Välj den enklaste Azure-tjänst som passar den verifierade lösningstypen och dess krav.
  För en helt statisk webbplats kan statisk webbhosting, exempelvis Azure Storage Static
  Website, vara tillräcklig. Detta är en riktlinje, inte ett beslut om ReceptAgents
  arkitektur eller driftstjänst.
- Föredra hanterad applikationsdrift framför att införa containerplattform eller
  containerorkestrering. Containrar används bara om ett dokumenterat krav motiverar
  komplexiteten.
- Håll deploymentflödet nära GitHub Actions och projektets egna skript. Skilj tydligt
  mellan bygg/test och publicering.
- Publicering ska kunna upprepas och ge en tydlig rapport om vad som ändrades. Skarpa
  eller destruktiva åtgärder kräver avsiktlig kontroll och får inte ske som en dold
  bieffekt av en vanlig CI-körning.
- Håll miljöer och godkännanden på minsta nödvändiga nivå. Lägg inte till separata
  miljöer, manuella grindar eller avancerad releaseorkestrering utan behov.

## Infrastruktur och skript

- PowerShell eller Bash är förstahandsval för installations- och deploymentautomation
  när ett skript behövs. Välj skal efter körmiljö; kräv inte dubbla implementationer
  utan ett uttalat portabilitetsbehov.
- Skript ska vara läsbara, versionshanterade, dokumenterade och säkra att köra om.
- Skript ska validera nödvändig konfiguration och avsluta med tydligt fel när ett
  förutsättningskrav saknas.
- Visa eller sammanfatta planerade ändringar innan potentiellt destruktiva åtgärder.
- Bicep, Terraform, ARM-mallar och andra infrastrukturverktyg är inte standardkrav.
  Inför dem endast om skript inte längre ger tillräcklig säkerhet, spårbarhet,
  reproducerbarhet eller hanterbarhet; dokumentera då skälet.
- Undvik att lagra parallella, motstridiga beskrivningar av samma infrastruktur.

## Identitet och hemligheter

- Ge workflow och applikation minsta möjliga behörighet.
- Föredra kortlivad federerad autentisering mellan GitHub Actions och Azure framför
  långlivade Azure-hemligheter där miljön stöder det.
- Förvara nödvändiga hemligheter i GitHub Environments/Secrets eller Azure Key Vault,
  aldrig i repo, skript, workflow-loggar eller klientkod.
- Begränsa vilka workflows och grenar som får publicera till Azure.
- Redigera bort hemliga värden från loggar och felutskrifter.

## Verifierbarhet

- Dokumentera för varje kontroll: förutsättningar, kommando, förväntat resultat och hur
  den körs i GitHub.
- En utvecklare ska kunna förstå CI/CD genom att läsa workflow- och skriptfilerna i
  repot; ingen separat orchestrator ska krävas för att följa flödet.
- Kontrollera leveransskript och workflow-ändringar i pull request innan de används
  mot delade eller produktionsmiljöer.
- Håll deploykonfiguration och instruktioner uppdaterade när leveranssättet ändras.

## Utanför dessa riktlinjer

Det här dokumentet väljer inte Azure-tjänst, nätverk, databas, hostingmodell, miljöantal,
branchmodell i detalj, exakt CI-jobb eller deploymenttrigger för ReceptAgent. HLD/DLD
och senare arkitekturbeslut identifierar och motiverar sådana val utifrån lösningens
faktiska behov.
