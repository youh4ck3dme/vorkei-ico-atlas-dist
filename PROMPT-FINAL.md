# VORKEI IČO ATLAS — finálny prechod (skill premium-wp-plugin-creator)

Okamžite vykonaj tento text na pracovnej kópii `2.2.0-rc.7`. Nie je to greenfield a nie je to nový plugin.

## Čo zo skillu platí

- PHP 8.1+, prefix `vorkei_ico` / `vorkei_firmabind` / `Vorkei_`.
- Každý vstup sanitizovať, každý výstup escapovať, nonce a `manage_options` na admin akciách a plnom lookupe.
- Žiadne surové SQL od užívateľa. Hodnoty cez `$wpdb->prepare()`. Identifikátor len z whitelistu.
- `ABSPATH` guard. Žiadne tajomstvo v kóde ani v HTML `value`.
- Inštalačný ZIP bez `qa/`, `run.php` a PHPUnit vstupu.

## Čo skill káže a tu je zakázané

- Nepridávaj Freemius, licenciu, update checker ani cudzí účtovný systém.
- Neprekladaj reťazce hromadne. Neprerábaj štruktúru na `public/` a `languages/`.
- Uninstall nemaže profily, snímky ani limity, kým nie je `vorkei_ico_delete_data_on_uninstall=yes`. Veta skillu „uninstall zmaže všetko“ tu neplatí.
- Nepridávaj tvrdenia o dlhoch, konkurze, živnostiach, exekúciách, IBAN ani o hotových schémach JetEngine.
- FirmaBind ostáva predvolene vypnutý.
- Živé RPO, ORSR, RÚZ, FS a VIES nevolaj. Nevymýšľaj backend.

## Postup

1. Spusti `work/qa/run.php`. Ak padne, oprav len tú vetvu, ktorú test zhodil, a spusti ho znova.
2. Ak nič nové nepadá, verziu nezvyšuj a správanie nemeň.
3. Zabaľ čistý inštalačný ZIP. Do repa ho pushni ako súbor, nie len ako zdrojáky.
4. Verdikt predaja je BLOCKED, kým nebeží WordPress, MariaDB, Woo classic, Blocks, HPOS, JetForm, namerané pokrytie a živé registre na kópii. Nenapíš, že sú nájdené všetky chyby.

## Invarianty, ktoré nesmieš rozbiť

Výpadok nemaže hotovú kontrolu. Celé číslo z JSON sa dopĺňa na 8 číslic, krátky reťazec nie. Zákaznícke IČ DPH sa nenahrádza registerovým. `X-Forwarded-For` sa nečíta. CSV nevolá `exit`. CCT `ALTER` len pri `vorkei_ico_alter_cct=yes`. Rozpracovaná dávka nie je chyba.
