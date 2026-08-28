# Releases

Twee repo's, zoals bij qube-browser:

| repo | wat er staat |
|---|---|
| `qntn-dev/Vaultbridge` (privé) | de code |
| `qntn-dev/vaultbridge-releases` | de downloads en `latest.json` |

De tweede bestaat omdat een geïnstalleerde app moet kunnen vragen of er een
nieuwere versie is, en dat moet zonder inloggegevens kunnen. Het formaat van het
manifest en wat de updater moet doen staan in `docs/manifest.md` van die repo —
niet hier, want die repo is wat een updater leest.

## Één versienummer

`VERSIE` in de wortel is de waarheid. `gereedschap/maak-release.sh` zet dat nummer
in `chrome-extensie/manifest.json` en `keeperapp/Info.plist`, zodat de app, de
extensie en het manifest het niet met elkaar oneens kunnen zijn over welke build
dit is. Ze reizen ook echt samen: het brugprotocol kan per versie veranderen, en
een nieuwe extensie bij een oude app geeft "onbekende actie".

## Een release maken

```bash
echo 0.2.0 > VERSIE
gereedschap/maak-release.sh                       # bouwt, hasht, schrijft het manifest
```

Dat raakt niets buiten `dist/` en het manifest in je eigen werkmap. Kijk het na,
schrijf de `notities` in `latest.json` met de hand — een changelog uit commits
leest als een git-log — en dan:

```bash
gereedschap/maak-release.sh --publiceren
```

Die stap pusht het manifest, maakt de GitHub-release met de bestanden eraan, en
zet een tag in deze repo. In die volgorde: het manifest eerst zou een updater een
versie laten zien waarvan de bestanden er nog niet zijn.

De Windows-schil bouwt deze machine niet. Geef het daar gebouwde bestand mee:

```bash
gereedschap/maak-release.sh --windows ~/Downloads/Kluis-0.2.0-win-x64.zip
```

Zonder dat komt `windows` in het manifest op `null`, en dat is dan de waarheid.

## Wat er nog niet is

**Ondertekenen.** `build-app.sh` signeert met het *Apple Development*-certificaat
dat er is. Dat is genoeg om de Keychain-toestemming te laten plakken tussen twee
builds, en niet genoeg om te distribueren: daarvoor is een Developer ID plus
notarisatie nodig. Op Windows is er nog niets. Tot dat er is kun je een echte
build niet van een aangepaste onderscheiden — en dit programma leest de hele
kluis, dus dat is hier geen ongemak. De README van de releases-repo zegt dat ook
met zoveel woorden.

**De updater zelf.** Het manifest en zijn formaat staan er; wat er nog moet komen
is de kant die hem leest, in de macOS-app en in de schil. De vier dingen die zo'n
updater moet doen (contents-API in plaats van raw, terugvallen op raw, de herkomst
van de download pinnen, de SHA-256 controleren vóór het uitpakken) staan in
`docs/manifest.md` van de releases-repo, met de meting erbij waarom.
