# 🍃 WILDPULK LANGS DE KUST — Changelog

**Website:** `http://192.168.43.51:8091` · **Backend:** `app.py` (Flask)
**Huidige versie:** `1.24.1` · **Laatst bijgewerkt:** 2026-11-02

## 🟩 1.24.1 — 2026-11-02 — Outages alleen nog op de radar

- **`landing/index.html`**: het uitklapblok **Outages** onder de uptime-regel is weg (markup, CSS en de `outagesLabel`-sleutel in alle zes talen). De storingen staan alleen nog op de radarpagina, in de sectie **Stratoxbox VPS**, met stoplicht, lijst, driestaps-afhandeling en het formulier voor een nieuwe melding. De ticker op de landing blijft wél waarschuwen als er een storing open staat.

## 🟩 1.24.0 — 2026-11-02 — Uptime onder de tewaterlating

- **`landing/index.html`**: de regel **Uptime** onder de tewaterlating blijft niet langer op `…` staan. `renderLaunch()` vult nu `#uptime-status` met de tijd sinds het eerste bezoek dat de site vastlegde (`data.tewaterlating` uit `/api/bezoek`), als `3 dagen` / `4 uur` / `12 minuten`. De tekst komt uit `Intl.NumberFormat` in de gekozen taal (geen nieuwe i18n-sleutels) en de tooltip toont de exacte tewaterlatingsdatum.

## 🟩 1.23.1 — 2026-11-02 — Themakiezer en taalknop zakken onder de ticker

- **`landing/index.html`**: de themakiezer staat op `top:44px` (mobiel `38px`) en de taalknop op `top:90px` (mobiel `82px`) in plaats van `14px`/`60px`. Daarmee liggen ze onder de tickerbalk (30px hoog, mobiel 26px) in plaats van erop.

## 🟩 1.23.0 — 2026-11-02 — Taalmenu, rode ticker bij storing, outage afgesloten

- **`landing/index.html` — taalkeuze als menu**: onder de themakiezer staat nu één klein rond knopje (🌐); klikken opent het taallijstje, een klik ernaast of Escape sluit het en na het kiezen van een taal klapt het dicht. Zelfde kleine `.56rem`-knoppen als eerst.
- **`landing/index.html` — ticker waarschuwt**: zolang er een outage open staat krijgt de balk `.ticker-alert` (rode onderrand, rode tag, rode labels) en begint het outage-item met ⚠️. Zodra alles gesloten is, verdwijnt dat weer — de klasse wordt bij elke verversing opnieuw gezet.
- **Outage “Iva-chat: Failed to fetch” afgesloten** via de driestaps-afhandeling van de radar: AI-commentaar, mens-stap en akkoord → stoplicht weer groen.

## 🟩 1.22.0 — 2026-11-02 — Iva-chat hersteld, taalkeuze klein onder het thema

- **Iva-chat “Failed to fetch”**: `/api/chat` accepteert nu ook `OPTIONS` en antwoordt met `204` + CORS-headers, zodat de preflight vanaf GitHub Pages slaagt (die gaf eerst 405 en dus een netwerkfout in de browser). De chat zelf probeert bij een netwerkfout **één keer opnieuw** na 0,9 s — precies wat nodig is als de site net herstart is — en toont anders een leesbare melding (`chatOffline`, in alle zes talen) in plaats van de ruwe browsertekst. Een mislukte vraag gaat niet mee in de gespreksgeschiedenis.
- **Taalknoppen**: de taalkeuze staat niet meer als balk bovenaan de pagina maar als **kleine kolom rechtsboven, direct onder de themakiezer** (`.langbar` → `position:fixed`, `top:60px`, `right:16px`, z-index onder het themamenu). De knoppen zijn veel kleiner (`.56rem`, compacte padding), zodat de paginanavigatie weer bovenaan staat.
- **Outage vastgelegd**: de storing rond de Iva-chat staat in `/api/outages` als melding “Iva-chat: Failed to fetch”.

## 🟩 1.21.0 — 2026-11-02 — Bezoekers in de ticker en in de radarzoek

- **`landing/index.html` — ticker**: nieuw item **bezoekers** uit `/api/bezoek?limit=50`: het aantal bezoeken van vandaag plus het totaal (`{today} vandaag · {total} totaal`). Vandaag wordt geteld uit de tijdstempels van de opgehaalde regels, dus zonder extra serverwerk. Zes talen kregen `tickerVisits` en `visitsToday`.
- **`landing/radar.html` — zoekbalk**: de bezoekers zitten nu in de zoekindex. Typ een IP, browser, pagina of herkomst en de treffer verschijnt tussen de andere resultaten als `ip · browser — bezoek`; klikken opent de bezoekersbox, scrolt naar die regel en licht hem ~1,8 s op. De index komt uit `/api/bezoek?limit=50` (fuzzy score, IP weegt dubbel) en wordt bij openklappen en bij het laden ververst.

## 🟩 1.20.1 — 2026-11-02 — Conceptpagina: zoekbalk als de radar, tags weg

- **`landing/site-concept.html` — zoekbalk**: dezelfde vorm als op de radarpagina — gecentreerd, `min(520px, 100%)` breed, `›`-prefix, monospace invoer met glow bij focus en de teller rechts. De gekleurde band met onderrand is weg, zodat de balk boven de lijst zweeft zoals op de radar; op smalle schermen vult hij de breedte.
- **Tags bovenaan weg**: de labels `site-concept` / `EepsHub` en het padlabel `concept-site/ · md-bestanden van ~/Projects` zijn uit de vensterbalk verdwenen. Alleen de vensterdots en de knop **← Landing** blijven over.

## 🟩 1.20.0 — 2026-11-02 — Ticker met getij en laatste commit

- **`landing/index.html` — ticker**: twee items erbij. **Getij** uit `/api/tides` (`{level} cm · {trend} · HW {tijd} ({hoogte})`, bron Rijkswaterstaat) en de **laatste commit** uit de publieke GitHub-API (`sha7 · bericht`, gelinkt aan de commit). Beide in alle zes talen (`tickerTide`, `tickerCommit`).
- **Mobiel**: op schermen tot 520px is de ticker 26px hoog met kleinere letters, minder ruimte tussen de items en een compactere tag; het Iva-paneel had die media-query al.
- **`website/app.py`**: `/api/tides` stuurt nu CORS-headers mee, zodat de ticker ook op GitHub Pages het getij kan tonen. `__version__` → `1.20.0`.

## 🟩 1.19.1 — 2026-11-02 — Ticker valt terug op de nieuwste vogel

- **`landing/index.html`**: de waarnemingen-feed van waarneming.nl stuurt geen `has_sound` mee (0 van de 500 vogels), dus de ticker bleef leeg op dat punt. Hij pakt nu de nieuwste vogel met geluid als die er is en anders de **nieuwste vogel**, met een eigen label (`laatste vogelgeluid` / `laatste vogel`) in alle zes talen. `/api/mediamap` was geen alternatief: die geeft nu 502 (GBIF via de sandbox-proxy).
- **`website/app.py`**: `__version__` → `1.19.1` (geen functionele backend-wijziging).

## 🟩 1.19.0 — 2026-11-02 — Ticker bovenaan en Iva als zwevend paneel

- **`landing/index.html` — ticker**: helemaal bovenaan een smalle balk (`position:sticky`) met een loop van drie dingen: de **laatste update** (eerste kop uit `CHANGELOG.md`), het **laatste vogelgeluid** (`/api/waarnemingen?group=1`, eerste waarneming met geluid, met tijd en plek) en de **laatste outage** (`/api/outages`, open of gesloten). Stopt met schuiven bij hover/focus en bij `prefers-reduced-motion`; de inhoud wordt één keer gedupliceerd voor een naadloze loop. Vijf nieuwe i18n-sleutels in alle zes talen.
- **`landing/index.html` — Iva-chatbox**: de chat staat niet meer als blok in de pagina maar als **zwevende knop rechtsonder + paneel**, net als de natuurgids-chat op de wildpluk-site: kop met naam en “AI op deze server · kan fouten maken”, sluitknop, scrollbaar log, textarea met **Stuur** en Escape om te sluiten. De ids (`bg-form`, `bg-input`, `bg-send`, `bg-log`) zijn gelijk gebleven, dus `frontChat` werkt ongewijzigd.
- **`website/app.py`**: `/api/waarnemingen` stuurt nu CORS-headers mee, zodat de ticker ook op GitHub Pages het vogelgeluid kan ophalen.

## 🟩 1.18.0 — 2026-11-02 — Bezoekers op de radar, filter en leesbaar logboek

- **`landing/index.html` — filter in de bezoekersbox**: boven de lijst staat nu een rij met **alleen eerste bezoek** (checkbox) en een **IP-filter** (zoekveld). Filteren gebeurt op de opgehaalde lijst; daarom vraagt de pagina nu `?limit=50` in plaats van 8, en de teller in de kop toont `{getoond} / {opgehaald}`. Zes talen kregen `visitsFirst`, `visitsIp` en `visitsNoMatch`; `applyLang` vult nu ook `data-i18n-placeholder`.
- **`landing/radar.html` — bezoekersbox**: linksonder een inklapbaar blok **👣 Bezoekers** in de groene radarstijl, met dezelfde API (`/api/bezoek?limit=8`), een teller `{getoond} / {totaal}`, scrollbox met tijd, IP, browser/OS, pagina, herkomst en of het een eerste bezoek is. Verversing bij openklappen.
- **`landing/changelog.html` — leesbaar logboek**: nieuwe pagina die `CHANGELOG.md` ophaalt en ter plaatse als markdown rendert (koppen, lijsten, code, nadruk, links). De “logboek”-link in de footer van de landing wijst nu hierheen in plaats van naar de ruwe `.md`-download. `build.py` neemt de pagina én `CHANGELOG.md` mee naar de Pages-versie, zodat het daar ook zonder CORS werkt.

## 🟩 1.17.3 — 2026-11-02 — Bezoekersbox ingeklapt met teller

- **`landing/index.html`**: het bezoekersblok staat nu **ingeklapt** bij het laden (nog steeds een `details`-blok, dus één klik open) en de kop toont rechts een teller `{getoond} / {totaal}`, gevuld door `renderVisits()` uit `GET /api/bezoek?limit=8`. Zo neemt de lijst geen ruimte in op de landing tot je hem opent.
- **CSS**: `.visits-label` (flex, duwt de teller naar rechts) en `.visits-count` (monospace-cijfers, gedempte kleur) toegevoegd; de rest van het blok is ongewijzigd.

## 🟩 1.17.2 — 2026-11-02 — Bezoekers-scrollbox terug op de landing

- **`landing/index.html`**: de bezoekerslijst staat weer op de landingspagina, onder de Radar-link: kop **Bezoekers**, een uitklapbaar `visits-wrap`-blok (standaard **open**) met een scrollbox van de laatste 8 bezoeken en de regel `{count} bezoeken van {unique} unieke IP’s · bijgewerkt {time}`.
- **Waarom weg en nu terug**: bij commit `e05d1f3` (“move radar section to separate radar.html page”) verdween de hele radarsectie van de landing, inclusief deze lijst; de CSS en `renderVisits()` stonden er nog, dus alleen de markup is teruggezet. De lijst vult zich via `GET /api/bezoek?limit=8` en meldt het eigen bezoek met een `POST /api/bezoek`-beacon.

## 🟩 1.17.1 — 2026-11-02 — Zoekbalk bovenaan de conceptpagina

- **`site-concept.html`**: de zoekbalk staat nu bovenaan, direct onder de vensterbalk, in de vorm van de radar-zoekbalk (`.scan-bar` met `›`-prefix, glow bij focus en een teller `gevonden/totaal` rechts). Het zoekveld is uit de werkbalk verdwenen; de knoppenrij blijft.
- **Escape** leegt de zoekbalk, net als op de radar; filteren gebeurt nog steeds live op het pad van de bestanden.

## 🟩 1.17.0 — 2026-11-02 — Site-concept: md-bestanden beheren

- **Nieuw: `/static/piracy/site-concept.html`** — toont alle concept-md-bestanden, met zoekveld, editor, **Opslaan**, **+ Nieuw bestand** en **Verwijderen**; `Ctrl/Cmd+S` werkt en er is een waarschuwing bij niet-opgeslagen wijzigingen. Link toegevoegd aan de paginanavigatie op de landing (op GitHub Pages wijst die naar de VPS, net als de tekst-editor).
- **`concept-sync.py`**: kopieert alle `*.md` uit `/home/eep/Projects` naar `website/concept-site/` met **identieke mappenstructuur** en hernoemt elk bestand naar `concept-site_<originele naam>.md` (166 bestanden, 79 mappen). `.git`, `.venv`, `node_modules` en caches worden overgeslagen.
- **API** (beheertoken verplicht, `X-AGENTS-Token`): `GET /api/concept` (lijst), `GET /api/concept/bestand?pad=…` (lezen), `POST /api/concept/bestand` (opslaan, `nieuw: true` → 409 bij bestaand), `POST /api/concept/verwijder`. Alleen `.md` binnen `concept-site/`; paden met `..` of een absolute start worden geweigerd.
- **Niet-publicatie**: `concept-site/` staat naast `app.py`, dus **buiten `static/`**; de bestanden zijn niet rechtstreeks te downloaden en de pagina geeft zonder token niets vrij. Opslaan maakt eerst een `.bak-<tijd>`, verwijderen hernoemt naar `.verwijderd-<tijd>` in plaats van te wissen.

## 🟩 1.16.5 — 2026-11-02 — Iva linksboven, CTA onder de intro

- **`landing/index.html`**: de **Iva-chatbox** staat nu als eerste blok in `main`, links van de hero en met `align-self:flex-start` bovenaan uitgelijnd; `main` mag wrappen, dus op smalle schermen stapelt hij boven de hero in plaats van ernaast.
- **CTA verplaatst**: de knop **Enter de Wildpluk-site** (met de fineprint eronder) staat nu direct onder de regel “…of het water mee- of tegenzit.” in de hero, in plaats van helemaal onderaan na `main`. De links **Naar de Crew** en **📚 Bookmarks** blijven waar ze stonden.

## 🟩 1.16.4 — 2026-11-02 — Haiku-vierkant naar beneden

- **`landing/index.html`**: het vierkant met de haiku-tabs (Natuurgids · Kapitein · Iva · Marta) staat nu onderaan het paginablok — onder de CTA, de kaarten en de links naar Radar en Natuur — in plaats van bovenaan naast de intro. Boven de paginalinks staat nu alleen nog de navigatie zelf.
- **Dubbele blok opgeruimd**: de haiku-box stond twee keer in de pagina (één keer boven `main`, één keer in de hero), met dubbele `id`'s tot gevolg. Er is één blok over; de losse `.intro-row`-grid om de hero-intro is vervangen door een gewone, gecentreerde `lede`.
- **Styling**: nieuwe wrapper `.musings-row` (max. 560 px, gecentreerd) en de oude `.intro-row`-regels weg.

## 🟩 1.16.3 — 2026-11-02 — Paginanavigatie bovenaan de landing

- **`landing/index.html`**: boven de taalkeuze staat nu een rij pill-links naar de andere pagina's: 🌿 Natuur, 📡 Radar, ⚓ Crew, 📚 Bookmarks en ✏️ Tekst-editor.
- **Relatieve paden** (`natuur.html`, `radar.html`, `crew.html`, `bookmarks.html`) werken zowel op de VPS (`/static/piracy/`) als op GitHub Pages; de tekst-editor gebruikt `/static/piracy/html-editor.html`, dat `build.py` voor de Pages-versie naar de VPS verwijst (daar staat geen editor).
- **Styling**: dezelfde pill-vorm en hover/focus-kleuren als de haiku-tabs, met rechts ruimte voor de thema-kiezer; labels zijn Nederlands, net als de navigatie op `radar.html` en `natuur.html`.

## 🟩 1.16.2 — 2026-11-02 — Sponsorknop zegt: nog geen wallet

- **`landing/index.html`**: “Zoek nog sponsors voor de boot.” is nu een echte knop (pill, hover/focus-stijl, `aria-expanded` + `aria-controls`) in plaats van een regel tekst.
- **Klik = melding**: onder de knop verschijnt “Nog geen wallet — sponsoren kan nog niet.” in een `role="status"`-regel; nog een klik klapt hem weer dicht. Er wordt niets verstuurd en er is geen betaallink.
- **Zes talen**: nieuwe sleutel `sponsorNote` in `nl`, `en`, `de`, `pl`, `bg` en `zh`, zodat de melding meewisselt met de taalknop. De knoptekst zelf gebruikt de bestaande `sponsors`-sleutel.

## 🟩 1.16.1 — 2026-11-02 — Landingspagina: agenda- en chat-knop weg

- **`landing/index.html`**: de knoppen **`📅 Agenda · Kapitein`** en de daaronder liggende **`💬 Chat met de bemanning`** zijn verwijderd, samen met het agenda-paneel en de bijbehorende CSS/JS (`toggleAgenda`, `toggleChatSelector`, het vullen van de chat-kiezer uit `/api/agents`).
- **Bemanningslijst blijft**: de `Bemanning`-dropdown met de kaarten per agent werkt gewoon door; de agenda zelf wordt nog steeds op `crew.html` beheerd (`📅 Agenda` per chatbox).
- **GitHub Pages**: `landing-gh/site/index.html` opnieuw gebouwd met `build.py` (zelfde verwijdering).

## 🟩 1.16.0 — 2026-11-02 — Radar: stoplicht voor Wildpluk met outages

- **Radar-pagina** (`landing/radar.html`): nieuwe sectie **Diensten** met de site Wildpluk en een stoplicht — groen zonder open melding, rood zodra er één openstaat, amber als de API niet bereikbaar is. Daaronder een **Outages**-droplist; klik op een regel voor het detailpaneel.
- **Melding maken**: titel, omschrijving in **maximaal drie zinnen** (480 tekens) en optioneel een duur in minuten. Die korte omschrijving is bewust: de zoekbalk matcht fuzzy op titel én omschrijving.
- **Weer op groen** gaat in drie stappen: (1) `AI-commentaar ophalen`, (2) mens-stap met een eigen opmerking, (3) `Akkoord: ja` of `Nee`. Pas bij **ja** sluit de melding en springt het licht groen; bij **nee** blijft hij open en moet de mens-stap opnieuw.
- **`GET /api/outages`**, **`POST /api/outages`** en **`POST /api/outages/<id>/stap`**: meldingen in `wildpluk.db` (tabel `outages`), zonder token — net als de ideeënbus. CORS voor `evtriest.github.io`, want de radar-pagina staat daar ook.
- **Eerlijk over de AI-stap**: het taalmodel is niet aan de site gekoppeld, dus stap 1 vat alleen samen wat er in de melding staat (titel, starttijd, aantal zinnen, duur) en zegt erbij dat het geen diagnose is. Geen verzonnen oorzaak.

## 🟩 1.15.1 — 2026-11-02 — Vertaal-knop (nog niet mogelijk)

- **`🌐 Vertalen`** in de werkbalk van de tekst-editor: vraagt eerst **“Alle talen opnieuw vertalen? Ja / Nee”**. Bij **ja én nee** volgt dezelfde melding: dit kan de lokale AI nog niet, er wordt niets aangepast.
- **Waarom niet**: vertalen met Qwen3-0.6B levert onbetrouwbare tekst (het model parafraseert en verzint; eerder schreef het een kernelversie die niet in de tool-uitvoer stond). Zes talen opnieuw laten schrijven door dit model is geen verbetering maar een risico, dus de knop is een eerlijke plekhouder zonder verborgen werking.
- **Geen backend-wijziging** buiten `__version__`; de knop raakt geen tekst en stuurt geen verzoek.

## 🟩 1.15.0 — 2026-11-02 — Tekst-editor in plaats van HTML-editor

- **`html-editor.html` omgebouwd**: van een algemene HTML-IDE naar een **tekst-editor voor de wildpluk-site**. Je ziet alleen de zichtbare tekst, per stuk in een eigen veld; opmaak, tags en `<script>`/`<style>` blijven buiten beeld. Geen snippet-bibliotheek, geen thema's, geen codevenster meer.
- **`GET /api/tekst?pagina=…`**: levert de tekststukken van `index` (de site), `landing`, `crew` of `bookmarks` — met positie, omliggende tag, de dichtstbijzijnde kop als label en een `sjabloon`-vlag voor Jinja (`{{ … }}`). Lezen is openbaar; de tekst staat toch al in de pagina.
- **`POST /api/tekst`**: schrijft alleen de gewijzigde stukken terug, op hun eigen plek. Beheertoken verplicht (`X-TEKST-Token`, dezelfde `agents_token.txt`/`AGENTS_TOKEN` als AGENTS.md), anders 401; zonder token 503; max 200 wijzigingen van 4000 tekens per keer. Elke wijziging moet nog letterlijk in het bestand staan, anders **409** — een verouderd tabblad overschrijft zo nooit nieuwere tekst. Vóór het schrijven gaat de oude versie naar `<bestand>.bak-<tijd>-tekst`.
- **Menu**: de link in het hoofdmenu van de site heet nu `✏️ Tekst-editor` (was `HTML editor`).
- **CORS**: `X-TEKST-Token` toegevoegd aan de toegestane headers.

## 🟩 1.14.0 — 2026-11-01 — Server-tools: `get_info` als eerste

- **`llama-qwen3.service`**: `ExecStart` krijgt `--tools get_info`, de lichtste van de zeven ingebouwde server-tools (geen parameters, read-only, 5 s timeout, 4 KB uitvoer). Hiermee zet `llama-server` de catalogus op `GET /tools` en de uitvoerder op `POST /tools`.
- **Belangrijk**: `--tools` stuurt de definities **niet** mee met een chatverzoek (in `server-tools.cpp` worden alleen de twee routes gekoppeld). Daarom haalt `app.py` de catalogus op, geeft de definities mee in `tools` en voert de gevraagde aanroep uit via `POST /tools {"tool": naam, "params": {}}`.
- **`app.py`**: nieuwe helpers `_gereedschap_definities()` (60 s cache), `_voer_gereedschap_uit()` en `_chat_met_gereedschap()`. `/api/chat` gebruikt die laatste: maximaal twee aanroepen per vraag, elke aanroep in het log (`tools: <naam> -> <uitkomst>`).
- **Waarom niet meer**: Qwen3-0.6B zet zonder `enable_thinking: false` zijn hele tokenbudget in ` thinking` (getest: `finish_reason: length`, geen antwoord); met thinking uit en deze ene tool blijft de chat snel.

## 🟩 1.13.3+patch2 — 2026-10-31 — Crew reorder, Nederlands default, bookmarks, Iva, kapitein-tekst

- **Landing**: `<html lang="nl">` en `applyLang('nl')` als default in plaats van `zh`; Nederlands-taalknop nu eerst in de taalkeuzebalk.
- **Crew-volgorde**: Kapitein → Iva (Bulgaars) → Natuurgids → Vogelgeluiden (HTML + `agentPaneel()` call order).
- **Foto + AGENT.md/agenda links**: van rechts naar links van de chat verplaatst (`grid-template-columns: 132px 1fr` + DOM-volgorde `aside → main`).
- **Hiërarchie badges**: Natuurgids krijgt badge “onder kapitein” + `.hierarchy-1` (blauwe rand); Vogelgeluiden krijgt badge “onder natuurgids” + `.hierarchy-2` (groene rand).
- **Bulgaarse gids → Iva**: beide `crew.html`-bestanden (HTML-titel + JS `titel`) omgedoopt naar “Iva”.
- **Kapitein-tekst**: “AI is nog niet slim genoeg” → “Geen chat, de lokale llm is voor hem te klein” (HTML + chat-note + 1e canned reply).
- **Natuurgids intro**: “Hoi, ik ben de gids. Wat wil je weten?” → “Hoi, ik ben de natuurgids, lastige materie maar altijd in voor een babbeltje, speciliteit trekvogels, compagnon vogelgeluiden gent”.
- **Bookmark-pagina**: `/static/piracy/bookmarks.html` (en GitHub Pages-variant) met 12 vooraf gevulde links (agent-links bovenaan), favicon via Google S2, sleep-sorteren, toevoegen, bewerken (naam/URL/omschrijving), verwijderen, max 30, opslag in `localStorage`.
- **Crew → Bookmarks link**: onder “Terug naar de landing” staat nu “📚 Bookmarks” in zowel `landing/crew.html` als `landing-gh/site/crew.html`.
- **RADAR**: plugin “mouse-in-the-corner” verplaatst van binnen `radar-inner` naar direct onder de `<summary>` (dus boven de bezoekerslijst).
- **Haiku-label**: “Haiku van de Natuur Gids” → “De natuurgids met lokale llm”.
- **Site-naam**: “Eeps Piracy Site” → “Eeps Site” in `<title>`, meta description, i18n `pageTitle`, crew-footer en README/CHANGELOG.

---

## 🎯 Waar dit naartoe gaat

Wildpluk begon als een persoonlijke plukagenda voor de **Den Helder kust** en groeit naar een **persoonlijke, kaartgerichte natuurgids**:

- **Eén blik, alles wat nú speelt** — wat is er te plukken/zien, waar, met foto en geluid erbij.
- **Van "hier" naar "jouw plek"** — straks vul je je eigen **plaatsnaam/coördinaten** in en werkt de app op jouw locatie.
- **Live uit de bron** — waarnemingen rechtstreeks uit **GBIF** en **waarneming.nl**, zo vers mogelijk.
- **Uiteindelijk in je zak** — als **app op de telefoon** (Android eerst, later iOS).

## 💚 Bonus: lid van waarneming.nl

De app werkt volledig op **open data (GBIF)** — een account is niet nodig. Maar ben je **lid van waarneming.nl**, dan krijg je extra's:

- **Je eigen waarnemingen** verschijnen apart op de kaart (in **wit**).
- Die van jou staan op hun **exacte locatie** — GBIF levert ze gegeneraliseerd op een 0,05°-grid (`floor(coord*20)/20`); de precieze plek halen we uit **waarneming.nl**.
- Je eigen waarnemingen ophalen via je **waarneming.nl-account** (API-key in `~/.waarneming_key`).

Kortweg: **meedoen op waarneming.nl = bonus**; de app blijft ook zonder lidmaatschap werken.

---

## 📝 TODO
- **📱 App als PWA** — ✅ **gebouwd** (v1.5.0): persoonlijke kaart-app met locatie, categorieën en eigen waarnemingen; te installeren via "Toevoegen aan startscherm". **Nog te doen:** korte URL `/app` activeren (na **herstart** van de service), **HTTPS/tunnel** → echte Chrome-"Installeren" + gebruik **buiten huis**, en een **echte APK** (WebView-wrapper) voor de **Play Store** (vereist JDK + Android SDK).
- **Eigen waarnemingen server-side** bewaren (nu per toestel in `localStorage`), zodat ze op elk apparaat terugkomen.
- **Promotie op waarneming.nl**: de Android-app onder de aandacht brengen bij/promoten op **waarneming.nl** (leden = bonus).
- **Seizoensherinnering**: eetbaar iets vorig jaar (zelfde maand/seizoen) gezien? → meld dat het nú de tijd is om daar weer te kijken.
- **Noten**: noten (hazelnoot, beukennoot, tamme kastanje, eikel).
- **Bulgarije**: (reminder).
- **Laatste geluid(sopname) vooraan**: de website start met de **meest recente geluidswaarneming** (audio-speler bovenaan; via waarneming.nl/GBIF waar een `Sound`-opname aanwezig is).
- **Nieuwste vogel- of ander geluid uitlichten**: toon de meest recente **geluidsopname** (vogel of ander dier) prominent als item — een eigen plek met afspelen, ook als er geen foto is.
- **Plukidee toevoegen** (invoer/beheer): nieuwe wildplukitem kunnen aanmaken — **locatie** (kaart/coördinaten), **foto**, en een **beschrijving** incl. eetbaar/medicinaal e.d.
- Accent op **eetbaar** en **medicinaal**: per plukidee/soort aangeven of het eetbaar en/of medicinaal is (label + filter).
- Menu-link voor de dag-sectie ("Wildplukidee van de dag") ontbreekt nog (wees naar `#wildeplukideen`).

---

## 🟩 1.13.3+patch — 2026-10-31 — Emoji-personalities per agent + kapitein-avatar live

- **Emoji per agent, afgeleid uit zijn SOUL.md/profiel** (10 per agent): natuurgids 🍄🐚🐟🌊🧺🌿🦀🌲🍂🥾 · bulgaars 🇧🇬🍸🥂🧿💅💰💃✈️🏔️🙄 · kapitein ⚓🌊⛈️🐟🥃🧭📡🚢🐦🎣. Emoji-sectie in de `AGENTS.md` van natuurgids en bulgaars (server leest die live in via de mtime-cache) én de stijlopdracht bleek te zwak voor het 0.6B-model (~2 van de 3 antwoorden), dus `app.py` garandeert het nu deterministisch: `_AGENT_EMOJI` + `_met_emoji()` zetten **minimaal 2 agent-emoji's in elk AI-antwoord** (bevat het antwoord er al 2+, dan blijft de tekst zoals hij is). Getest live: natuurgids-antwoord eindigt op `🦀 🐚`, bulgaars op `🧿 🥂`.
- **Vogelgids bewust uitgesloten**: zijn antwoordcontract (uitsluitend mp3-url of het vaste onbekend-antwoord) zou door emoji's kapot gaan — de chatbox herkent de url niet meer.
- **Kapitein-box (geen AI, vaste antwoorden)**: de 8 vaste antwoorden hebben nu zijn emoji's ingebakken (2 à 3 per antwoord, roulerend uit zijn tien) — Bron én Crew.html zijn allebei aangepast (`agents/kapitein/AGENTS.md` + `landing/crew.html` + `landing-gh/site/crew.html`).
- **Kapitein-foto live**: `agents/kapitein/foto.jpg` (1120×1400, zelfde formaat als de foto van de bulgaars) uit `landing/kapitein-avatar.svg`; `/api/agents/kapitein/foto` → 200. Ook de **foto van de bulgaars stond niet op de VPS** → gekopieerd, `/api/agents/bulgaars/foto` → 200.
- **GitHub-crewpagina**: fotopaneel + kapitein-emoji's gepusht (`c972251`), Pages live binnen een minuut.
- Op de VPS is de patch aangebracht naast een backup (`app.py.pre-emoji`); `__version__` is niet verhoogd — 1.13.3 (lokaal) / 1.13.2 (VPS) blijf zoals ze waren.

## 🟩 1.13.3 — 2026-10-31 — Speler-box, Bulgaarse kennisbank + foto, lokaal startbaar als user

- **Speler-box voor de vogelgids-chat** (`landing/crew.html`, GitHub-versie via `build.py` opnieuw gegenereerd): het kale .mp3-adres verdwijnt uit de antwoordtekst en komt in een aparte **speler-box** in de bubbel — vogelnaam op het label (gelezen uit de bestandsnaam, bijv. `koolmees-roep-1474.mp3` → "🐦 Koolmees") plus `<audio controls>` dat meteen probeert af te spelen; lukt de automatische start niet (autoplay-blokkade) dan staat de play-knop er direct klaar voor.
- **Bulgaarse kennisbank**: `agents/bulgaars/kennis.txt` (tekst over Sofia, uit `TeachBulgarian/app/data/sofia_1500_woorden.txt`) wordt nu door de server gelezen — `_bulgaars_kennis()` met mtime-cache (zelfde patroon als de vogelgids-kennisbank) en toegevoegd aan `_bulgaars_system_prompt()`. Getest: 21 kB prompt = 5248 tokens, past, antwoord blijft Bulgaars.
- **Foto van de Bulgaarse gids**: `/home/eep/iva.jpg` staat als `agents/bulgaars/foto.jpg`; `/api/agents/bulgaars/foto` → 200.
- **Lokaal als user starten is mogelijk** (was geblokkeerd): `website/.venv` aangemaakt (flask + requests — `pip install --user` is geblokkeerd door PEP 668). Starten: `cd website && WILDPLUK_LLAMA_URL=http://127.0.0.1:8080 .venv/bin/python3 -c "import app; app.app.run(host='0.0.0.0', port=8090)"`. Let op: **poort 8080 is lokaal bezet door `llama-server.service`** (user-unit, llama op 8080), dus de site moet lokaal op een andere poort; op de VPS is de combinatie wildpluk=8080 + llama=8081 gewoon goed.
- **Lokaal beperkingen** ( staan alleen op de VPS): onze kopie mist `templates/` en `static/`, dus `/` en `/landing` geven lokaal 500/404; de API-routes (`/api/chat` met beide agents, `/api/agents/<naam>/foto`) werken lokaal wél. De lokale `wildpluk.db` kreeg een lege `ideas`-tabel zodat `get_pulkideeën()` niet meer crasht.
- **`__version__`** naar `1.13.3`.

## 🟩 1.13.2 — 2026-10-30 — Vogelgids: strikt antwoordcontract server-side

- **Vogelgids-reactie** volgt een vast antwoordcontract (gedocumenteerd in `agents/vogelgids/AGENTS.md`): **uitsluitend de mp3-url** van de gevraagde vogel, óf het vaste onbekend-antwoord: `De vogel <naam> is onbekend. / Je vroeg: <vraag>. / Zoek een andere vogel; ik ken alleen de vogels uit mijn lijst.`
- **De server beslist deterministisch**: `/api/chat` met `agent="vogelgids"` matcht zelf op de kennisbank (`_vogel_treffers`) en roept de llama voor dit antwoord niet meer aan — het kleine 0.6B-model verwisselde de formulieren en verzon urls (getest: "Pinguïn" gaf een verzonnen mp3-link).
- Vogelnaam-extractie via `_vogel_naam_vraag`: eigen naamwoorden in de vraag gaan voor, anders de vraag kort doorgegeven.
- **`__version__`** naar `1.13.2`.

## 🟩 1.13.1 — 2026-10-30 — Agent-profielboxen met foto + agenda

- **Crew-pagina (`landing/crew.html` + GitHub-variant)**: het AGENTS.md-beheer is per afzonderlijke chatbox geplaatst. Elke chatbox heeft rechts een **profielkolom** met **foto** (`GET /api/agents/<naam>/foto`, fallback op placeholder met initialen), een ingeklapte **Agent.md-editor** (lees/sla op met beheertoken, versielijst met backups) en een **📅 Agenda-knop**.
- **Agenda per agent**: knop opent de **beheerbare agenda** van de betreffende agent onder de chatbox (`GET/PUT /api/agents/<naam>/agenda`,zelfde token en 10 versies backups als AGENTS.md). Nog geen AGENDA.md → startersagenda ("Vandaag / Deze week / Vaste afspraken") die bij opslaan vast wordt gelegd.
- **Fix**: `GET /api/agents/<naam>` gaf per ongeluk de backupversielijst terug (dubbele route-decorator op `_agent_versies`); nu weer de agenttekst met de versielijst erin.
- **Foto per agent**: zet `foto.jpg/.jpeg/.png/.webp/.gif/.avif` in `website/agents/<naam>/`.
- **CORS**: `PUT`-methode en de `X-AGENTS-Token`-header zijn nu toegestaan voor de `/api/agents*`-endpoints (bewerkingsbeheer werkt daarmee óók van GitHub Pages uit).
- **`__version__`** naar `1.13.1`.

## 🟩 1.13.0 — 2026-10-30 — AGENTS.md per chat + vogelgids

### 🗂️ AGENTS.md-principe voor de chats
- Elke chat heeft nu zijn eigen personabestand: **`website/agents/<naam>/AGENTS.md`**
  met rol en regels; `_chat_system_prompt()` leest zijn basis uit
  `agents/natuurgids/AGENTS.md` (fallback op de ingebouwde tekst als het bestand
  ontbreekt — de chat kan dus nooit stuk). Maandinfo en plukideeën blijven
  dynamisch uit de database.
- `/api/chat` accepteert een optionele **`agent`**-keuze in het verzoek
  (`"natuurgids"` is de standaard); per chat zo zijn eigen AGENTS.md.
- **`agents/kapitein/AGENTS.md`**: bron-tekst van de vaste kapitein-antwoorden
  (de Kapitein-box blijft bewust canned, geen llama).

### 🇧🇬 Nieuw: Bulgaarse gids op de Crew-pagina
- Vierde chat, naast natuurgids, vogelgids en kapitein: dezelfde gidsrol, maar
  de bezoeker praat in het Bulgaars (`agent: "bulgaars""); plukideeën en
  maandinfo blijven dynamisch uit de database en het Nederlands, het bestand
  `agents/bulgaars/AGENTS.md` schrijft voor om die namen te vertalen.
- /api/chat kiest op agent-veld: `vogelgids` / `bulgaars` / standaard natuurgids.
- month-context van de prompt verhuist naar `_maand_context()`, zodat
  natuurgids en Bulgaarse gids dezelfde dynamische kennis delen.

### 🐦 Nieuw: Vogelgeluiden-chat op de Crew-pagina
- Derde chat op de crew-pagina, naast natuurgids en kapitein: de **vogelgids**,
  die bij de vraag de best passende soorten zoekt in de kennisbank
  **`agents/vogelgids/kennis.txt`** (één vogel per regel, "Naam - mp3-url",
  kopie van `rag/vogelgeluiden.txt`) en de luister-link meelevert.
- Retrieval in pure Python (woord-overeenkomst met voorvoegsel-treffers, top-5
  records aan de systeemprompt; personabestand `agents/vogelgids/AGENTS.md`).

### 🔧 Techniek
- `frontChat()`-wiring in `crew.html` voor de drie AI-chats, met eigen ids,
  spreekkleur (`.chat-birds`/`.msg-bird`/`.chat-bulg`/`.msg-bulg`) en optioneel
  agent-veld.
- **mtime-cache**: `_agents_md()` en `_vogel_records()` lezen het bestand opnieuw
  zodra de mtime verandert — AGENTS.md- en kennisbank-aanpassingen gaan live
  zonder herstart of andere server-actie.
- **`__version__`** naar `1.13.0`.

### ✏️ Nieuw: AGENTS.md bewerken via de Crew-pagina
- **`GET /api/agents`** (lijst met bewerkbare agents), **`GET/PUT /api/agents/<naam>`**
  (ruwe tekst lezen / overschrijven). Schrijven vereist het beheertoken
  (`AGENTS_TOKEN` env of `agents_token.txt` naast app.py), zoals bij de
  hotspot-meldingen; zonder token is bewerken uit (503), verkeerd token 401.
- Alleen agents die al bestaan zijn schrijfbaar (regex-check op de mapnaam) —
  geen schrijfroute naar willekeurige bestanden; max 20 000 tekens.
- **Versiegeschiedenis, max 10 versies per agent**: bij elke opslag schuiven de
  backups op (`AGENTS.md.bak.1` = vorige versie … `.bak.10` = oudste, die valt
  weg); een losse `.bak` uit de eerste versie wordt in de rotatie meegenomen.
  `GET /api/agents/<naam>` levert de versielijst mee (nr + bytes) en
  `?versie=<nr>` leest een oudere versie; in de beheerbox staan die in de
  keuzelijst en is "Opslaan" de herstel-knop: de oudere versie wordt weer
  actief en de dan draaiende tekst gaat zelf veilig naar de backups.
- Crew-pagina heeft onderaan een opvouwbare "AGENTS.md-beheer"-box (details):
  agent kiezen, tekst bewerken, opslaan met token (bewaard in de local storage
  van de browser). Opslaan is direct van kracht dankzij de mtime-cache.
- CORS uitgebreid naar `/api/agents*`, zodat de GitHub Pages-versie met het
  juiste token ook kan bewerken.

### 🧭 Beheer — bestanden en herstart
- **AGENTS.md-bestanden** (persona per chat, vrij aan te passen):
  `agents/natuurgids/`, `agents/bulgaars/`, `agents/vogelgids/` en
  `agents/kapitein/` (bron-tekst van de canned antwoorden; die chat zit in
  `crew.html` en heeft geen backend).
- **Kennisbank**: `agents/vogelgids/kennis.txt` (284 vogels, "Naam - mp3-url",
  kopie van `rag/vogelgeluiden.txt`).
- **Live zonder herstart**: de AGENTS.md-bestanden en de kennisbank (mtime-cache)
  plus alle front-end-bestanden (`crew.html`, `landing-gh/`).
- **Herstart van `wildpluk.service` nodig**: één keer, om de nieuwe `app.py`-code
  (agent-keuze, vogel-RAG, Bulgaarse prompt) te activeren; daarna alleen nog bij
  toekomstige **code**-wijzigingen, niet bij persona- of kennisbank-wijzigingen.

---

## 🟩 1.12.0 — 2026-10-29 — Taalkeuze, haiku en Crew

### 🌍 Landingspagina (`/landing`, en `evtriest.github.io`)
- **Taalkeuze bovenaan**: Chinees, Bulgaars, Pools, Engels, Nederlands en Duits,
  met vlag-buttons; de pagina **start in het Chinees**. Alle teksten staan in het
  `I18N`-object; het wisselen vertaalt kop, knoppen, radar, bemanning, uitje,
  praatjes, footer en het logboek mee.
- **Tewaterlating** onder de kicker: de datum van het eerste bezoek aan de
  GitHub Pages-variant, uit het nieuwe veld **`tewaterlating`** van `GET /api/bezoek`
  (valt terug op het allereerste bezoek).
- **Haiku van de Natuur Gids** bovenaan: de lokale llama maakt hem, en bij elke
  **refresh een nieuwe** — `GET /api/haiku?t=<timestamp>` met `cache: 'no-store'`;
  server-side **geen cache** meer, willekeurige seed, `temperature 0.95` en tot
  **drie pogingen** voor er drie regels zijn. De gids dicht in het Nederlands
  (het kleine model herhaalt zichzelf in het Chinees).
- **Uitje**: sectie over de tocht met verrekijker en kokkels langs de dijk, met
  het getij-check en het gesprek met de gids — in alle zes talen.
- **Praatjes**: een e-mailadres en een oude laptop is genoeg om de pagina te
  laten maken; eerste consult gratis, daarna automatiseren.
- **Dropdowns**: **Bezoek** (radar) en **Bemanning / Who is on board** zijn
  inklapbaar (`<details>`), standaard dicht.
- **Logboek** in de footer: `$ git log --oneline -5` van
  `evtriest/evtriest.github.io`, **pas zichtbaar en opgehaald na een klik**
  (`details` + `toggle`-event), met de commits in de gekozen taal.
- Nieuwe favicon `japan-flag.svg`.

### ⚓ Nieuw: Crew-pagina
- `/static/piracy/crew.html`: dezelfde look als de landing, met de titel
  **Crew** en een **chatbox met de natuurgids** (`POST /api/chat`, context blijft
  bewaard) plus CTA naar de wildpluk-site en een terug-link.
- Link **“Naar de Crew”** op de landingspagina, onder de knop naar de site.
- **Kapitein-box** op de Crew-pagina: een tweede chat **zonder** verbinding met
  llama of de backend — vaste antwoorden in de pagina, met de mededeling dat AI
  nog niet slim genoeg is voor de kapitein en de knipoog “FreeBuff betwijfelt dat.”

### 🔧 Techniek
- **CORS** voor `https://evtriest.github.io` op `/api/chat` (via `after_request`),
  zodat de GitHub Pages-versie cross-origin kan chatten; `/api/haiku` had dat al.
- `landing-gh/build.py` bouwt nu **beide** pagina's (`index.html` + `crew.html`)
  met relatieve assets en een absolute CTA naar de VPS.
- **`__version__`** naar `1.12.0`.

## 🟩 1.11.0 — 2026-10-29 — Radar met bezoekers

### 📡 Nieuw: radar-sectie “Bezoek”
- De landingspagina heeft onderaan een **radar**-sectie met daarin **Bezoek**: de laatste bezoekers van de site, met **tijd, IP, browser, OS, apparaat, pagina, herkomst en taal**, plus hoe vaak dat IP al langs kwam.
- **Eén regel per IP**: de lijst toont per adres alleen het nieuwste bezoek (met het aantal keren en het eerste bezoek erbij); de teller blijft het totale aantal bezoeken.
- Backend: **`POST /api/bezoek`** legt een bezoek vast, **`GET /api/bezoek`** geeft de laatste bezoeken terug; beide met **CORS** voor `https://evtriest.github.io`.
- De site logt **`/` en `/landing`** zelf via `before_request`; de GitHub Pages-variant meldt zich met een klein `fetch`-beacon. Een refresh binnen **2 minuten** telt niet als nieuw bezoek, en de tabel blijft op **5000** bezoeken (sqlite, `wildpluk.db`).
- **`__version__`** naar `1.11.0`.

## 🟩 1.10.0 — 2026-10-29 — Piraten-landingspagina

### 🏴‍☠️ Nieuw: `/landing`
- **Eeps Site**: een losse landingspagina met de **getijdenachtergrond van de Omarchy-desktop** (thema “ship at sea” plus de getijcurve van Den Helder), een grote knop naar de **wildpluk-site** (`/`) en een **piratenvlag als favicon**.
- **Volledig statisch** (één HTML-bestand met eigen CSS, geen externe libs of fonts), dus ook zonder JS en zonder extra verzoeken naar de backend.
- Bestanden: `static/piracy/index.html`, `pirate-flag.svg`, `tide-bg.jpg`; de route `/landing` (en `/landing/`) serveert de pagina.
- De achtergrond is een **momentopname** van het getij, gerenderd met dezelfde code als de desktop-achtergrond (`tide-wallpaper.py`, 1920×1080); opnieuw maken met `landing/make-bg.py` op de laptop.
- **`__version__`** naar `1.10.0`.

## 🟩 1.9.1 — 2026-10-29 — Schijfruimte in de kopbalk

### 💾 Toegevoegd: disk naast CPU en RAM
- Derde chip in de kopbalk: **`DISK <gebruikt> / <totaal> GB`** van de schijf waar de site staat (`/data`), met in de tooltip het **percentage**, de **vrije ruimte** en het **mountpoint**.
- Komt uit dezelfde **`/api/vps`** en ververst dus ook **elke minuut** mee.
- **`__version__`** naar `1.9.1`.

## 🟩 1.9.0 — 2026-10-29 — CPU en geheugen in de kopbalk

### 📈 Nieuw: VPS-belasting in plaats van getij
- **De kopbalk toont geen HW/LW meer** (dat staat nu in de getijcurve in de hero) maar de **staat van de VPS**: `CPU <percentage>% · <load>` en `RAM <gebruikt> / <totaal> GB`.
- **Automatisch verversen: elke 60 seconden** via **`/api/vps`**; de eerste waarden staan al server-side in de HTML, en bij een mislukte verversing blijft de vorige waarde staan.
- **CPU-percentage uit `/proc/stat`**: het verschil sinds de vorige meting (venster van minimaal 10 s, dus bij de minuutverversing een echt 1-minuutgemiddelde). Bij een korte herhaling of de eerste meting wordt de laatste waarde respectievelijk een schatting via load average gebruikt, zodat de balk niet springt.
- **Geheugen uit `/proc/meminfo`** (`MemTotal − MemAvailable`), dus inclusief cache die de kernel zo nodig vrijmaakt. Geen extra dependency's (geen `psutil`).
- Hover over de waarden toont load (1/5 min), aantal cores, geheugenpercentage en het tijdstip van de meting.
- **`__version__`** naar `1.9.0`.

## 🟩 1.8.0 — 2026-10-29 — Getijcurve bovenaan de pagina

### 🌊 Nieuw: getij in de hero
- **De hero toont nu het getij** in plaats van de losse slogan: een curve van **3 uur terug tot 21 uur vooruit**, met de **huidige waterstand** (cm t.o.v. NAP), het verloop (stijgend/dalend), de **NU-lijn** en de komende **HW/LW** met tijd en hoogte.
- **Bron:** Rijkswaterstaat Waterinfo, astronomisch getij **Den Helder, Marsdiep** — dezelfde bron als de getij-achtergrond op de desktop, dus site en desktop lopen gelijk.
- **Server-side getekend** (inline SVG uit Jinja), dus geen extra JavaScript of externe libs; de curve werkt ook zonder JS.
- **`/api/tides`** geeft nu de eerstvolgende **HW/LW met tijd en hoogte** uit dezelfde data (5 min cache); de kopbalk bovenin toont daardoor actuele tijden in plaats van de oude vaste tabel. Bij een storing aan de RWS-kant blijft de vorige curve staan; is er nog niets, dan valt de hero terug op de oude slogan.
- **`__version__`** naar `1.8.0`.

## 🟩 1.7.0 — 2026-10-28 — Hotspot-status in de footer

### 📶 Nieuw: hotspot-status
- **Knop in de footer** (`Hotspot aan/uit/onbekend`) met een **groene** of **rode** stip, plus de tijd van de laatste melding in de tooltip. Ververst elke 30 s.
- **Herkomst:** `GET /api/hotspot` geeft de laatst gemelde status. De Pi (Hookipa) post elke 30 s of de telefoon-gateway `192.168.43.1` bereikbaar is via `POST /api/hotspot` (`hotspot_report.py` + `wildpluk-hotspot.timer`).
- **Uit = geen melding:** valt de hotspot uit, dan is de Pi offline en stopt het melden; de site kleurt rood zodra de laatste melding ouder is dan **90 s**.
- **Token verplicht** voor melden (`HOTSPOT_TOKEN` of `hotspot_token.txt` naast `app.py`); zonder token antwoordt de POST met 503, met een verkeerd token 401. Lezen (`GET`) is openbaar.

## 🟩 1.6.1 — 2026-10-19 — Naam: Wildpluk langs de kust

- **Naamswijziging** naar **Wildpluk langs de kust** in de kop, paginatitel, footer, app-header, app-titel en het webmanifest.
- Topbar: **WILDPULK** met subtitel **LANGS DE KUST**; app-header: **Wildpluk** met tag **langs de kust**.
- `short_name` van de PWA is nu **Wildpluk kust**; service-worker-cache gebumpt (`v3`) zodat de nieuwe manifest direct doorkomt.
- APK-bestandsnaam, URL's, database, servicenaam en package-id zijn **ongewijzigd**.

## 🟩 1.6.0 — 2026-10-19 — Ideeënbus

### 💡 Nieuw: Ideeënbus
- **Kleine knop in de footer** (`💡 Ideeënbus`) opent een klein venster met alle binnengekomen **ideeën en opmerkingen** (alleen tekst) — nieuwste bovenaan
- **Iedereen kan een idee plaatsen** (vrij tekstveld, max 500 tekens) — geen login of account nodig
- **Iedereen kan een idee weer weghalen** (✕ per item, met bevestiging)
- Ideeën gaan over **alles** — een wildpluk-idee, een verbetering aan de site, of gewoon een losse opmerking

### 📱 App (web-app in de APK)
- **Tik op een rondje op de kaart** opent nu betrouwbaar de popup met foto, naam, datum en GBIF-link (op touch/WebView gebeurde er soms niets)
- Nieuwe **APK v1.6.0** (`/static/wildpluk.apk`); WebView-foutopsporing aan

### 🔧 Techniek
- Nieuwe tabel **`ideeenbus`** in `wildpluk.db` (`id`, `text`, `created`), automatisch aangemaakt bij het eerste gebruik
- Nieuwe API-routes: `GET /api/ideeenbus` (lijst), `POST /api/ideeenbus` (plaatsen), `DELETE /api/ideeenbus/<id>` (weghalen)
- `app.py` versie → **1.6.0**

---

## 🟦 1.5.0 — 2026-10-17 — Mobiele app (PWA)

### 📱 App
- Nieuwe **persoonlijke mobiele app**: `/static/app.html` (korte URL `/app`), los van de site
- **Jouw locatie** instellen (GPS via 📡 of handmatig: plaatsnaam → coördinaten via Nominatim)
- **Kaart rond jouw plek** met waarnemingen mét media (GBIF, laatste 60 dagen)
- **Categorie-chips** om te filteren (vogels, zoogdieren, walvis, vissen, planten, paddenstoelen, insecten, …)
- **Eigen waarneming toevoegen** (＋): naam, categorie, datum, notitie, **foto** en locatie → **witte stip** op de kaart
- **Profiel** (⚙️): locatie/nummer wijzigen, **zoekstraal** 10/25/50 km, eigen waarnemingen bekijken en verwijderen
- **Startscherm-icoon**: PWA-manifest + service worker + iconen (groen blad) → "Toevoegen aan startscherm"
- **Echte Android-app (APK)**: dunne WebView-wrapper met **native GPS** (brug om de `http`-geolocatiebeperking te omzeilen), **fotokiezer** en terug-knop — te downloaden op `/static/wildpluk.apk`; broncode in `android/` (aapt2→javac→d8→zipalign→apksigner), getest in een **Android 14-emulator**
- Menu-link **📱 App** op de site

### 💚 Lid van waarneming.nl
- Vul je **waarneming.nl-gebruikersnummer** in → jouw waarnemingen komen erbij (via GBIF `recordedBy="User <nr>"`), in **wit** met eigen label
- **Bonus, geen must**: de app werkt volledig zonder account; een account kun je later aanmaken

### 🔧 Techniek
- Eigen waarnemingen bewaard **op het toestel** (browser `localStorage`), geen login nodig
- Data rechtstreeks uit **GBIF** (open, CORS); `app.py` versie → **1.5.0**

## 🟦 1.4.0 — 2026-10-17 — Kaarten, categorieën & UI

### 🗺️ Kaarten & waarnemingen
- Nieuwe sectie **"Recente waarnemingen rond Den Helder"** (direct ná "Wildpluk van de dag"): inline Leaflet-kaart met waarnemingen **mét media** binnen 50 km (GBIF), **60 dagen**, max **1.000** punten, nieuwste eerst
- Nieuwe **kleur per soortgroep** (i.p.v. één kleur): vogels blauw · zoogdieren oranje · **walvis/dolfijn grijs** · insecten geel · spinachtigen paars · **kwallen transparant** · weekdieren & schelpen zandbruin · kreeftachtigen rood · zeesterren magenta · vissen cyaan · planten groen · **paddestoelen bruin** · mossen & korstmossen grijsgroen · zeewier olijfgroen · overig grijs
- Nieuwe **dynamische legenda met aantallen** per groep; geluidsopnames = **gestippelde rand**
- **Klikbare legenda**: op een groep klikken opent een kaart met **alleen die groep** + een **namenlijst** ernaast (klik op een naam → zoom naar de stip)
- **Namen op de kaart**: bij kleine groepen (≤12 stippen, o.a. "Eep (eigen)") tonen de stippen hun **naam als label** op de kaart; ook knop **"📍 Op de kaart"** in de sectie **Waarnemingen Eep**
- Eigen waarnemingen **Eep** apart op de kaart in **wit** (groep "Eep (eigen)"; altijd zichtbaar, ook zonder media en ouder dan 60 dagen)
- Eep-waarnemingen op de kaart op hun **exacte locatie** (opgehaald uit de publieke waarneming.nl API), i.p.v. GBIF's gegeneraliseerde 0,05°-grid (`floor(coord*20)/20`)
- **Walvis/dolfijn en vissen extra nadruk**: walvis = grotere stip met zwarte rand + permanent label "🐋 …"; vissen = grotere stip met donkere rand + label bij hover; staan altijd bovenop (ook in de per-groep-kaart)
- Popup met **foto-thumbnail**, wetenschappelijke naam, datum/tijd en GBIF-link
- Soorten in de buurt: straal 5 → **10 km**; soorten nu klikbaar → **per-soort kaart** (Leaflet) met stippen binnen 10 km
- Recente waarnemingen: **media-badges** (📷 foto / 🔊 geluid)
- Eigen waarnemingen (GBIF): **foto-thumbnails** uit GBIF media, link naar waarneming.nl

### 🧭 Categorieën & navigatie
- Categorieën uitgebreid naar **14 groepen** (OR over meerdere taxonKeys); `facetLimit` 5000 → **20000** (fix: "Alles" toonde max 5000 i.p.v. 8795)
- Categorie-selectie: dropdown → **horizontale chip-balk** (emoji + naam) bovenaan de sectie; wrapt naar meerdere rijen
- Secties hernoemd: "Wildpluk Ideeën" (was "Alle Wildpluk Ideeën"), "Waarnemingen Eep" (was "Eigen waarnemingen (GBIF)"), "Soorten in de buurt op de kaart" (was "Soorten in de buurt (GBIF)")
- Sectie "Soorten in de buurt op de kaart" **verplaatst** naar direct onder "Wildplukidee van de dag"; menu bijgewerkt
- Nieuwe **menu-link** "Recente waarnemingen rond Den Helder"

### 🎨 Layout & UI
- Wildplukidee van de dag: kop hernoemd (was "van vandaag"); foto als **los, rondom afgerond blok** (marge, uiteindelijk 400px hoog)
- "Wildpluk van de dag": plaatje vult nu **altijd het hele kader** (`object-fit:cover`, rekt mee met de kaart), i.p.v. verticaal gecentreerd met witruimte; op mobiel vast kader van 220px
- Footer-versie gesynchroniseerd met `app.py` (`{{ version }}` → v1.4.0)

### 🔧 Techniek & API
- `/api/soortmap`: puntensampling via jaar-facet (K=5) i.p.v. diepe paginering → eerste lading **~1,5-2 s** (was ~97 s), met cache
- Nieuw **endpoint `/api/mediamap`** (paging 300/verzoek, cache 10 min)

## 🟦 1.3.0 — 2026-10-16 — Wildpluk fixes
### 🌊 Getijden
- Header HW/LW/klok laden via JS (loadTideHeader), HW/LW + tijd
### 🌿 Wildpluk
- Wildpluk-idee van vandaag: 1 willekeurig item (database)
### 🐦 Waarnemingen
- Recente waarnemingen Den Helder: laatste 5 waarnemingen binnen 5 km
- Eigen waarnemingen (GBIF): /api/gbifeigen → 5 records
- Soorten in de buurt (GBIF): /api/soortenbuurt → 5 km ronde Den Helder
### 🔧 Techniek
- Wildpluk template versie 1.3.0 (backend + template)
- Template: $ helper + esc ingeboekt; CHANGELOG in de footer

## 🟦 1.0.0 — 2026-10-16 — Wildpluk initialisatie
### 🌿 Basis
- Wildpluk-website: plukagenda + plukideeën + getijden + recente waarnemingen
- Wildpluk template layout en styling
