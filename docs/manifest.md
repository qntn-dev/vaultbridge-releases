# Het formaat van latest.json

Eén bestand in de wortel van deze repo, dat de updater van elke geïnstalleerde
Vaultbridge leest. De sleutels zijn Nederlands, net als in het brugprotocol.

```json
{
  "versie": "0.1.0",
  "naam": "Vaultbridge 0.1.0",
  "gepubliceerd": "2026-08-28",
  "samenvatting": "Eén regel, voor de melding in de app.",
  "notities": "Markdown; wat er nieuw is en wat er stuk was.",
  "macos":    { "url": "...", "omvang": 12345678, "sha256": "…64 hex…",
                "minimaal": "26.0" },
  "windows":  { "url": "...", "omvang": 12345678, "sha256": "…64 hex…" },
  "extensie": { "url": "...", "omvang": 123456,   "sha256": "…64 hex…" }
}
```

Een platform dat in deze release niets heeft staat op `null`, niet weggelaten:
dan is het verschil tussen "niet uitgebracht" en "een oude versie van het
manifest" te zien in plaats van te raden.

`versie` is één nummer voor alle drie de stukken. Dat is geen netheid maar
noodzaak: de extensie en de app praten via een brugprotocol dat per versie kan
veranderen, dus ze horen samen te reizen.

## Wat de updater moet doen

Deze vier dingen zijn allemaal met schade geleerd in qube-browser (`QUpdater.cs`,
zelfde patroon, zelfde eigenaar). Ze staan hier zodat de macOS-kant en de
Windows-kant niet elk hun eigen versie verzinnen.

1. **Haal het manifest via de contents-API, niet via raw.**
   `https://api.github.com/repos/qntn-dev/vaultbridge-releases/contents/latest.json`
   met `Accept: application/vnd.github.raw`. `raw.githubusercontent.com` zit
   achter een CDN die een bestand minutenlang vasthoudt en zijn cache niet op de
   query string sleutelt — `?t=<nu>` helpt dus niet, en `Cache-Control: no-cache`
   ook niet. Daar is bij qube op gemeten: alleen de API gaf de net gepushte
   versie. Een release van vijf minuten oud moet vindbaar zijn, anders is het
   eerste wat iemand met de updater doet concluderen dat hij niet werkt.

2. **Val terug op raw als de API niet antwoordt.** Niet-ingelogde API-verzoeken
   zijn 60 per uur per adres — één mens haalt dat nooit, een kantoor achter één
   adres wel. Een verouderd antwoord is beter dan geen.

3. **Pin de herkomst van de download.** Alleen `https://github.com/qntn-dev/vaultbridge-releases/releases/download/…`
   mag. Een manifest dat ergens anders naar wijst wordt geweigerd. Dat maakt een
   overgenomen repo niet veilig — het haalt alleen de makkelijkste helft weg.

4. **Controleer de SHA-256 vóór je iets uitpakt of uitvoert**, en weiger bij
   verschil zonder te vragen.

En wat de updater **niet** moet doen: zichzelf vertrouwen. Zolang er niet
ondertekend wordt is dit een slot op de deur en niet op de kluis, en hoort de
gebruiker dat te zien voordat hij op Bijwerken drukt.
