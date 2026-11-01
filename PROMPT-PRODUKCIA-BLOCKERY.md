# VORKEI IČO ATLAS 2.2.0-rc.8 — produkčný prompt na odstránenie blockerov

Okamžite vykonaj prácu. Nie je to ďalší audit ani zoznam odporúčaní. Východisko sú bajty `2.2.0-rc.8`, SHA-256 `4e1e1674e8c9024785061d2298afe561e85d9c42648e81fb81ce7c87eb0286c8`. Vyššie číslo verzie nie je dôkaz kvality. Nič neoznač ako PASS bez príkazu, výstupu a zhody týchto bajtov.

Prostredie: lokálna kópia WordPressu a MariaDB, nie produkcia zákazníka. Nemeň živý web, neposielaj poštu, nekupuj a nevolaj platbu. Poverenia nevpisuj do kódu, logov, HTML ani do chatu.

## Nemenné pravidlá

- FirmaBind ostáva predvolene `no`.
- Úvodné nuly IČO sa nestratia. Krátky reťazec sa na 8 číslic nedopĺňa. Celé číslo z JSON áno.
- Zákaznícke IČ DPH sa nenahrádza registerovým. VIES a väzba sú osobitné stavy.
- Výpadok nemaže hotovú kontrolu. Neúplná odpoveď neoživí historický aktívny stav.
- `X-Forwarded-For` sa nečíta. CSV nevolá `exit`. Súbor nad 8 MiB nie je úspech.
- Expirovaný vlastník zámku nemaže novší token a nezapíše profil.
- Snímka bez väzby na objednávku nezapíše ID, akoby dôkaz bol uložený.
- Uninstall nemaže dáta, kým nie je `vorkei_ico_delete_data_on_uninstall=yes`.
- CCT `ALTER` len pri `vorkei_ico_alter_cct=yes`.
- Žiadny licenčný server, Freemius, Doklad Core ani vymyslený proxy kontrakt.

## Blokery, ktoré máš zrušiť

Každý riadok uzavri stavom PASS, FAIL alebo presným NOT RUN. Preskočené nie je PASS.

1. **WordPress a databáza.** Najnižšia deklarovaná aj aktuálna podporovaná kombinácia. Čistá inštalácia, aktualizácia z predchádzajúceho ZIP bez reaktivácie, zachovanie ručných hodnôt a núl. MariaDB, nie len sqlite stub. Dôkaz: verzie z `wp core version` a `SELECT VERSION()`, SQL riadky pred a po.
2. **WooCommerce.** Classic checkout, Blocks checkout a HPOS. Objednávka s IČO, snímka, jeden `order_id`, opakovanie platby nespôsobí druhú snímku. Dôkaz: ID objednávky, ID snímky, hash payloadu a meta `_vorkei_firmabind_snapshot_id`.
3. **JetFormBuilder.** Reálny Save Form Record s nakonfigurovaným mapovaním. Skryté ID z prehliadača nie je autorita. Dôkaz: záznam formulára a riadok snímky.
4. **Pokrytie.** PHP aj JavaScript osobitne: najmenej 99 % vykonateľných riadkov, 99 % zavolaných funkcií a 95 % vetiev. Nástroj uveď príkazom. Umelé volania a prázdne assertions sa neráta. Kalibrácia nejde do hlavného čísla.
5. **WPCS a PHPStan.** Aktuálny beh nad týmito súbormi, nie starý počet 879/143. Mechanické formátovanie oddeľ od zmeny správania. Žiadne plošné ignorovanie pravidiel. Výnimka len ako úzky, zdôvodnený false positive.
6. **Živé zdroje na kópii.** Jedno známe IČO proti RPO, ORSR a RÚZ. FS a VIES len s vlastným prístupom zákazníka; bez neho ostanú vypnuté a v texte nesmú vyzerať ako zaplatená funkcia. Kľúč sa nesmie objaviť v profile, HTML, CSV ani logu.
7. **Licencia dát.** Z primárneho dokumentu každého zdroja oddeľ predaj softvéru, použitie zákazníkom a redistribúciu dát. Verejná stránka nie je licencia. Nezistené označ NOT RUN a zdroj vypni, ak je voliteľný.
8. **Obnova.** Záloha súborov a databázy, obnova, rovnaký profil a snímka. Výpadok medzi snímkou a objednávkou nesmie potvrdiť neuložený dôkaz.
9. **Balík.** Po poslednej zmene kódu znova zlož deterministický ZIP. Druhé zostavenie má rovnaký SHA-256. ZIP neobsahuje `qa/`, `run.php`, logy, databázu ani kľúče. Potom už bajty nemeň.

`work/qa/run.php` ostáva regressia, nie náhrada bodov 1 až 8. Stub 92/92 nestačí.

## Verdikt

Stabilnú verziu bez prípony `rc` daj len keď sú body 1 až 9 PASS a nie je otvorený P0 ani P1. Inak nechaj `rc`, verdikt BLOCKED a pomenuj presne chýbajúci dôkaz. Do `docs/VYDANIE-<verzia>.md` zapíš príkazy, verzie, hashe a zostávajúce limity. Snímku nenazvi kvalifikovaným podpisom.
