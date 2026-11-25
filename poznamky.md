# Poznámky – analýza project_definition

## Zdroj pravdy (context.md)
- Robot UR3e, PolyScope 5.21.2, chapadlo Robotiq Hand-E (URCap, funkce `rq_*`).
- Detekce kontaktu: z context.md jsou k dispozici `force()` a `get_tcp_force()`.
- V context.md NEJSOU: rozměry kvádrů, polohy (výchozí pozice nad sloupcem, bod země, pozice nového sloupce), práh síly pro kontakt.

## Nalezené problémy
1. **Logická chyba – otočení o 180° (kroky 6a1, 6b1).**
   Hand-E je paralelní 2-prsté chapadlo, symetrické kolem osy nástroje.
   Po otočení o 180° jsou prsty ve stejné ose jako před otočením → druhé měření
   dá vždy stejný výsledek jako první. Kombinace „nenarazí → narazí“ (6a3b)
   a „narazí → nenarazí“ (6b3a) tak nikdy nenastanou a typy „leží svisle“ /
   „leží vodorovně“ nelze rozlišit. Pro rozlišení je potřeba otočení o **90°**.
2. **Krok 10 – přeorientace kostky.** Pořadí v novém sloupci je pevné
   (vodorovně, stojí, vodorovně, …), ale ze zdrojového sloupce přichází kostky
   v libovolné orientaci. Není definováno, jak z ležící kostky udělat stojící
   (a naopak) – vyžaduje naklopení nástroje o 90° kolem vodorovné osy.
3. **Chybějící data:** rozměry kvádrů (výška vrstvy v novém sloupci, šířka
   vs. rozevření Hand-E – zdvih není v context.md uveden), polohy, práh síly.

## Rozhodnutí (odpovědi uživatele)
- Otočení: 90° místo 180°.
- Krok 10: změna stojí ↔ leží naklopením nástroje o 90°.
- Rozměry nejsou potřeba: plně otevřený gripper je užší než delší strana → jen stavy zavřeno / plně otevřeno. Výška vrstev se řeší jízdou do nárazu.
