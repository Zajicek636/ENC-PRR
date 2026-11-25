# Přestavění sloupce kvádrů – UR3e + Robotiq Hand-E

Soubor programu: [stack_blocks.script](stack_blocks.script)

## Platforma (dle context.md)
- UR3e, PolyScope 5.21.2
- Robotiq Hand-E přes Robotiq URCap (funkce `rq_*`)
- Detekce nárazu: `force()` (snímač síly v TCP na e-Series), před každým hledáním `zero_ftsensor()`

## Předpoklady
- Plně otevřený gripper je užší než delší strana kvádru a širší než kratší strana (potvrzeno uživatelem) → gripper se používá jen zavřený nebo plně otevřený, rozměry kvádrů nejsou potřeba.
- `HOME` a `BUILD` mají stejnou orientaci nástroje (svisle dolů, „0°“).
- Nový sloupec stojí na stejné úrovni jako zem pod zdrojovým sloupcem (`GROUND_Z`).

## Úpravy oproti project_definition
| Krok | Původně | Upraveno | Důvod |
|---|---|---|---|
| 6a1, 6b1 | otočení o 180° | otočení o **90°** | 2-prsté chapadlo je symetrické; po 180° by druhé měření dalo vždy stejný výsledek |
| 10 | neurčeno | změna stojí ↔ leží naklopením nástroje o 90° kolem osy prstů | pořadí v novém sloupci je pevné, orientace zdrojových kostek libovolná |

## Rozhodovací tabulka (kroky 4–6)
| Náraz v 0° | Náraz v 90° | Výsledek | Uchopení |
|---|---|---|---|
| ne | ne | stojí (6a3a) | v 90° |
| ne | ano | leží svisle (6a3b) | v 0° |
| ano | ne | leží vodorovně (6b3a) | v 90° |
| ano | ano | zem (6b3b) → konec | – |

Uchopení probíhá vždy v orientaci, ve které prsty nenarazily → prsty svírají delší stěny kostky (krok 7).

## Průběh
1. Aktivace gripperu, cílová vrstva = „leží vodorovně“.
2. `HOME` → zavřený gripper jede dolů do nárazu nebo na `GROUND_Z`.
3. O `D_UP` nahoru, otevřít, `D_PROBE` dolů (0°); zpět, otočit o 90°, `D_PROBE` dolů.
4. Vyhodnocení dle tabulky; při „zem“ → krok 9.
5. Uchopení, kontrola `rq_is_object_detected()` (jinak chyba a `halt`).
6. Svisle nahoru, `HOME`.
7. `BUILD` otočený o 90°; pokud se typ uchopené kostky (stojí/leží) liší od cílové vrstvy, naklopení o 90°.
8. Dolů do nárazu (dosednutí na zem nebo předchozí vrstvu), otevřít, nahoru, přepnout cílovou vrstvu, opakovat.
9. Po dosažení země: svisle nahoru, `HOME`, kloubový přejezd do home pozice robota (`ROBOT_HOME`), konec.

## Parametry k nastavení
### Body v instalaci (Installation → Features → Point)
Body se naučí ručním najetím robota a tlačítkem **Set point**; uloží se do instalace a skript je čte jako proměnné typu pose. Při učení musí být aktivní stejný TCP jako při běhu (`TCP_1`).

| Bod | Význam |
|---|---|
| `HOME` | pozice nad zdrojovým sloupcem, nástroj svisle dolů |
| `BUILD` | pozice nad novým sloupcem, stejná orientace jako `HOME` (dostatečně vysoko i pro naklopenou kostku) |
| `GROUND` | TCP při dotyku země; skript z něj použije jen Z (`GROUND_Z`) |
| `ROBOT_HOME` | home pozice robota; robot nejdřív najet do Installation → General → Home, pak Set point |

Dostupnost bodů instalace jako proměnných ve skriptu je nutné ověřit na PolyScope 5.21.2 (není v context.md).

### Parametry ve skriptu
| Proměnná | Význam |
|---|---|
| `F_CONTACT` | práh síly nárazu [N] – ověřit na pěnových dílech |
| `D_UP`, `D_PROBE` | odjezd nahoru / zkušební jízda dolů (`D_PROBE > D_UP`); rozdíl = hloubka uchopení |
| `V_SEARCH`, `A_SEARCH` | rychlost a zrychlení hledání kontaktu |

Výchozí hodnoty parametrů ve skriptu jsou pouze zástupné – před spuštěním je nutné je nastavit.

## Známá omezení
- `ROBOT_HOME` je poloha TCP, ne úhly kloubů. `movej` na pózu volí řešení inverzní kinematiky nejbližší aktuální konfiguraci, takže výsledné úhly kloubů se mohou lišit od home pozice v instalaci (poloha TCP bude stejná).
- Při naklopení stojící kostky do polohy „leží“ je kostka držena u konce → její střed je vůči `BUILD` posunut o (polovina délky − hloubka uchopení). Bez znalosti rozměrů to nelze kompenzovat.
- Dosažitelnost naklopených poloh (dosah UR3e 500 mm) a kolize gripperu se sloupcem je nutné ověřit v simulátoru.
- Funkce mimo seznam v context.md (`movel`, `movej`, `speedl`, `stopl`, `pose_add`, `pose_trans`, `get_actual_tcp_pose`, `zero_ftsensor`, `popup`, `halt`) jsou standardní URScript dle URScript manuálu pro e-Series.
