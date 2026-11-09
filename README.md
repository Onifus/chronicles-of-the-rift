# Chronicles of the Rift — v0.9 systems and implementation notes

## v0.9.4: Vesnický hub

Připravený hub je nyní malá průchozí vesnice se šesti místy: portálem, cechem, kovárnou, tržištěm, tréninkem a úschovnou. Místa lze vybrat kliknutím, přejít k nim klávesami WASD/šipkami nebo použít Enter/E. Po první návštěvě se místo objeví v rychlém cestování. Na telefonu jsou popisky stanic a ovládání zmenšené pro úzkou obrazovku.

- **Cech:** místní denní zakázky na zabití, bosse nebo dokončenou výpravu nabízejí zlato, úlomky duší a runové úlomky. Vítězství a zlato odemykají tři hodnosti pomocníka s trvalým bonusem do poškození a zlata.
- **Tržiště:** tři osobní kusy výbavy lze koupit za zlato. Vyprodané zboží se obnoví až placeným přetočením, jeho cena postupně roste. Tlačítko rychlého rozkladu používá nastavený filtr a chrání zamčenou i nošenou výbavu.
- **Kovárna:** zachovává výzkum, runy, výrobu a truhly. Alchymie uvaří až tři elixíry vitality za esence; každý přidá 15 % maximálního zdraví pro jednu běžnou výpravu. **Trénink** vede přímo ke třídám, talentům a Knize strategií; **Úschovna** otevře správu výbavy a inventáře.
- **Automatizace:** vedle automatického výběru kouzel, další výpravy a rozkladu lze zapnout výrobu náhodných run. Výroba spotřebuje pouze runové úlomky nad zásobu devíti; nastavení je dobrovolné a vypnuté ve výchozím stavu. Offline postup používá nedávné místní výpravy jako odhad.
- Hub počítá místní čas a návštěvy míst, cechovní zakázky, pomocníka i útratu na trhu. Souhrn je dostupný v lokálním exportu analytiky; data se nikam neposílají.
- Tmavý režim platí také pro nová okna a prvky vesnice. Globální žebříčky a výměna mezi hráči vyžadují server, proto zůstávají vypnuté.

Stávající LocalStorage savy se migrují doplněním nových výchozích polí. Samotné denní zakázky i nabídka trhu fungují lokálně.

## Hub a kniha strategií

Odpočinkový hub obsahuje přímé vstupy do Knihy strategií, Arzenálu, Síně tříd, Dílny a Výprav. Kniha popisuje profily, automatický výběr, styly pohybu, synergie kouzel a dovoluje změnit profil přímo v přehledu. Přepínač Tmavý režim je v horní liště a jeho volba se ukládá do místního nastavení.

## v0.9: endgame and readable automation

This release applies the actionable redesign in the attached research report to the existing static game while preserving the v1 LocalStorage key and save format.

- **Progression:** fixed Normal → Veteran → Nightmare I–IV → Abyss ladder. Tiers unlock by visible lifetime win thresholds, and use announced enemy HP/damage, elite, loot, and modifier values. Rift tiers scale explicitly rather than secretly adapting existing enemies.
- **Combat variants:** 10 named run modifiers, weekly challenge, endless Astral Rift, and a 10-boss Gauntlet with a blessing and curse choice after each boss. Enemy waves and bosses scale by published tier and wave formulas. Boss attacks show a warning before firing.
- **Loot:** 11 equipment slots, side-by-side equipment and inventory, filtering and sorting, up to 200 carried items, rarity colors, physical drops, pickup/salvage filters, 50 affixes, Greater Affixes, Ancient/Mythic rarity, 12 legendary effects, sockets and six runes. Boss drops use a guaranteed first Rare and a Legendary pity counter. Boss Sigils buy a Legendary; Rift Dust buys an Ancient/Mythic item.
- **Automation:** 16 build profiles in total, including seven added archetypes: summoner, melee, ranger, tank, hybrid, trapper and idle. Combat Rules set target, movement, skill trigger, potion threshold, ultimate policy, danger handling and loot/salvage filters; slots unlock at account levels 10 and 30.
- **Long-term economy:** boss kills give sigils and runeshards, Rift clears grant Rift Dust, and completed or failed runs grant Soul Fragments. Astral Prestige remains available. At Rift 10+, Transcendence awards permanent tree points while resetting the Nightmare/map ladder; it keeps collection, cosmetics, gear and class mastery.
- **HUD and mobile layout:** difficulty and progression summaries, rarity/socket indicators, three-column menu cards on desktop and one-column cards on small phones, with inventory loadout and bag responsive to narrow screens.
- **Local analytics:** export includes local active-day counts (1/7/30 day windows), runs, completion and boss failure rates, build usage, death causes, difficulty clears, session estimates, Legendary pacing, inventory size and discards. It does not send data anywhere.

### Analytics and online limits

D1/D7/D30 here mean days on which this local save was active in the rolling window. They are not population retention: without accounts or a server, browsers cannot combine one another’s data. Session and discard rates are estimates based on saved run history. A/B testing is not enabled; the export supplies local observations for later balancing. The proposal’s global leaderboards and cohort measurements therefore remain outside the capabilities of this static build.

## Instalace a spuštění

1. Obsluž celou složku statickým webserverem nebo ji nahraj do kořene GitHub Pages (`main` → `/(root)`). Místně lze použít `python -m http.server 8000` a otevřít `http://localhost:8000`.
2. Po aktualizaci načti stránku přes `Ctrl+Shift+R`.
3. Před nasazením si v Nastavení a data stáhni JSON zálohu. LocalStorage je svázané s originem (doména + protokol + port); při přesunu hostingu použij import/export.

Hra nevyžaduje Node, databázi ani CDN. Všechny savy a analytika zůstávají v prohlížeči.

## Hlavní pravidla

Výprava standardně trvá 20 minut s bossem každé 2 minuty. Vítězství v oblasti zvedá její tier; dvě vítězství otevírají další oblast. Smrt neodebírá výbavu, zlato ani mistrovství. Prestige a Transcendence jsou samostatné resetovací vrstvy s odlišnými zachovanými odměnami.

## Obsah projektu

- `index.html` — webové rozhraní.
- `game.js` — souboj, loot, postup, ukládání a analytika.
- `style.css`, `storybook.css`, `combat.css` — UI, fantasy motiv a soubojový HUD.
- `assets/` — sprite atlas a mapové ilustrace.

### Save compatibility

Existing saves continue to use `chronicles-of-the-rift-save-v1`, format version 1. Newly introduced fields receive defaults when old saves load. Export a JSON backup before replacing a hosted build, especially if switching the site origin.

### Manual verification performed

JavaScript syntax was checked with `node --check game.js`. Runtime/browser smoke checks and full 20-minute balance runs still need a browser session; numerical values in the research report are starting balance targets, not playtest results.

---

