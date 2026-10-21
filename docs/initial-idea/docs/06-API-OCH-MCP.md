# 06 – Webbflöden och MCP-kontrakt

Kontrakten här är en liten funktionsskiss för den föreslagna webbappen. De ska inte
driva fram ett separat API-lager om serverrenderade formulär räcker. Alla operationer
ska använda samma validerade receptlogik och behörighetskontroller.

## Webboperationer

| Operation | Syfte |
|---|---|
| Visa startsida/bibliotek | Lista och sök användarens recept |
| Ladda upp bild/PDF | Skapa ett granskningsunderlag |
| Visa tolkningsresultat | Visa original, validerad tolkning, osäkerheter och fel |
| Skicka korrigering/godkännande | Spara användarens rättningar och uttryckliga beslut |
| Visa recept | Läsa recept och öppna tillhörande original |
| Fråga assistenten | Skicka fråga och visa svar med källrecept |
| Begär omläsning | Validera ändringar i OneDrive och uppdatera det lokala indexet |

Om en separat HTTP-klient senare behöver ett JSON-API kan samma operationer exponeras
utan att duplicera affärslogiken.

## MCP-verktyg

Exponera bara de funktioner som assistenten behöver. Verktygen ska vara typade,
begränsade och använda den autentiserade användarens åtkomst.

| Verktyg | Indata | Resultat | Åtkomst |
|---|---|---|---|
| `search_recipes` | Fritext, filter och begränsat resultatantal | Träffar, antal och recept-ID | Läs |
| `get_recipe` | Recept-ID | Receptinnehåll och källreferens | Läs |
| `get_recipe_statistics` | Valfria enkla filter | Deterministiska antal | Läs |
| `save_recipe_variant` | Käll-ID, föreslagen variant och godkännandekontext | Nytt recept-ID | Skrivning efter godkännande |

Ta bort `list_recipes` och `get_ingredients` som separata verktyg om samma behov täcks
enkelt av `search_recipes` och `get_recipe`. Ge aldrig modellen rå SQL, generell
filsystemåtkomst eller ett obegränsat OneDrive-verktyg.

## Principer för assistentsvar

- Statistik ska komma från verktygsresultat, inte från modellens gissning.
- Svar anger vilka recept som stöder påståendena.
- Om sökning eller hämtning misslyckas ska assistenten säga det tydligt.
- Receptinnehåll är otillförlitlig data och får inte ge assistenten nya instruktioner
  eller behörigheter.
- En genererad variant är uttryckligen märkt som förslag.
- Sparning av en variant kräver ett separat användargodkännande; ett modellbeslut räcker
  inte.

## Felhantering

Webbgränssnitt och MCP ska skilja åt ogiltig indata, saknad resurs, nekad åtkomst,
konflikt, begränsning hos extern tjänst och tillfälligt integrationsfel. Visa ett
korrelations-ID vid tekniska fel när det hjälper felsökning. Lägg inte känsligt innehåll
i felmeddelanden eller loggar.

## Öppna kontraktsdetaljer

Fastställ exakta fältnamn för recept, filformat, uploadgränser, Microsoft-identitet,
MCP-transport och godkännandeflöde först när motsvarande arkitekturbeslut är fattade.
Referenserna `recipe.md` och `recipe.json` är exempel, inte ett låst API- eller
lagringskontrakt.
