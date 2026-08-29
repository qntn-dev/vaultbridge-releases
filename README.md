# Vaultbridge — releases

Downloads voor Vaultbridge, en het manifest dat de ingebouwde updater leest.

De broncode staat hier niet. Deze repo bestaat omdat een geïnstalleerde app moet
kunnen vragen "is er een nieuwere versie?" zonder inloggegevens, en de bronrepo
privé is — dus liggen de binaries apart van de code.

Vaultbridge is één ding in drie stukken, en ze horen bij dezelfde versie:

| | wat het is |
|---|---|
| **Kluis.app** (macOS) | de app die met de lokale Keeper-service praat |
| **Kluis** (Windows) | dezelfde app, als WebView2-schil om hetzelfde venster |
| **de Chrome-extensie** | praat nooit met de kluis, alleen met de app |

De extensie en de app praten via een brugprotocol dat per versie kan veranderen.
Een nieuwe extensie bij een oude app levert "onbekende actie" op, en omgekeerd een
half werkend venster. Neem ze samen.

## Installeren op macOS

`Kluis-<versie>.dmg` openen, **Kluis naar Programma's slepen en hem daar starten**.
Niet vanaf het schijfbeeld: het pad van een gemonteerd schijfbeeld verdwijnt bij het
uitwerpen, en dan start de browser een hostbinary dat er niet meer is. Kluis zegt het
zelf als je dat vergeet.

De eerste keer: rechtermuisknop op Kluis → Openen → Openen. Dat is één keer nodig,
en waarom staat hieronder.

Verder hoef je niets aan te melden. Kluis schrijft bij elke start zijn hostmanifest
naar elke Chromium-browser die op deze Mac staat — Chrome, Brave, Edge, Vivaldi,
Arc, Chromium. Zet je er later een browser bij, dan is één keer Kluis starten genoeg.

De extensie staat niet in de Chrome Web Store: pak `vaultbridge-extensie-<versie>.zip`
uit, en laad hem via `chrome://extensions` → Ontwikkelaarsmodus → "Uitgepakte extensie
laden". De extensie-ID ligt vast, dus dit is eenmalig.

## Dit is nog niet ondertekend, en dat is hier erger dan elders

Geen van deze builds is code-signed voor distributie: op macOS is er een
*Apple Development*-certificaat en geen Developer ID, dus geen notarisatie; op
Windows is er niets. Gatekeeper en SmartScreen waarschuwen dus, en — dit is het
punt — **je kunt een echte build niet van een aangepaste onderscheiden.**

Bij een browser is dat vervelend. Bij dit programma niet: het leest je hele
kluis. Een geruilde download is niet "een rare browser", het is iemand anders die
je wachtwoorden leest. Geef deze builds daarom aan niemand buiten de twee mensen
die eraan bouwen, tot er wél een Developer ID en een Authenticode-certificaat is.

## latest.json

De updater haalt
[`latest.json`](https://raw.githubusercontent.com/qntn-dev/vaultbridge-releases/main/latest.json)
op en vergelijkt `versie` met zijn eigen. Per platform staan de download-url, de
omvang en een SHA-256, zodat een afgekapte of geruilde download opvalt vóór er
iets wordt uitgevoerd.

Die hash bewijst integriteit, geen auteurschap: hij reist over hetzelfde pad als
het bestand dat hij beschrijft. Alleen ondertekenen lost dat op, en dat is er nog
niet — zie hierboven.

Het formaat staat in [docs/manifest.md](docs/manifest.md).
