# Kontext projektu – pracoviště Universal Robots

## 1. Robot

| Parametr | Hodnota | Zdroj |
|---|---|---|
| Výrobce | Universal Robots A/S | štítek na robotu, About dialog |
| Model | UR3e (e-Series) | štítek na robotu (`R-R-URK-UR3e`) |
| Sériové číslo | 20195300163 | štítek na robotu, About → General |
| Datum výroby | 26 Feb 2019 | štítek na robotu |
| Hmotnost | 11.2 kg / 24.7 lbs | štítek na robotu |
| Krytí (Enclosure type) | IP54 | štítek na robotu |
| Max. nosnost (payload) | 3 kg / 6.6 lbs | štítek na robotu |
| Max. dosah | 500 mm / 19.7 in | štítek na robotu |
| Typ robota | Industrial | štítek na robotu |
| Certifikace | KC `R-R-URK-UR3e` | štítek na robotu |
| Montáž | stolní, hliníková příruba na pracovním stole | foto pracoviště |

### Pokyny ze štítku

- **Transportation:** Lift in the two tubes.
- **Emergency movement:** Force joint rotation by pulling the robot arm.

## 2. Software a síť (PolyScope – About)

### General

| Položka | Hodnota |
|---|---|
| Universal Robots Software | 5.21.2 |
| Build Date | February 27 2025 |
| Hostname | ur-20195300163 |
| IP address | 192.168.0.94 |
| Teach Pendant S/N | N/A |
| S/N | 20195300163 |

### Version

| Položka | Hodnota |
|---|---|
| Baseline | 12.12.686 |
| Polyscope | 74.20.298 |
| Controller | 71.7.43 |
| Firmware | 43.10.100 |
| Platform | 0.1.1 |

### Stav PolyScope v době pořízení snímků

- Program: `<unnamed>`
- Instalace: `default`
- Aktivní TCP: `TCP_1`
- Prostředí: PolyScope 5 (CB/e-Series GUI), záložky Program / Installation / Move / I/O / Log.

## 3. Koncový efektor – Robotiq Hand-E

| Parametr | Hodnota |
|---|---|
| Typ | Robotiq Hand-E (adaptivní 2-prstý paralelní chapadlo) |
| Rok výroby | 2019 |
| S/N | D-00993 |
| Původ | Made in Canada |
| Značení | CE, `pat. ip.robotiq.com` |
| Prsty | kovové čelisti, obalené žlutou páskou |
| Integrace | Robotiq URCap (`Robotiq_Grippers - Robotiq Inc.`) |

### Robotiq URCap – lišta (toolbar) v PolyScope

- Záložky: `Vacuum Gripper`, `Adaptive Gripper`
- Identifikátor v liště: `D880` / `8E9F`
- Ovládání: `Open` (0 %), `Close` (100 %)
- Aktuální hodnoty na snímku:
  - Position: 100 % (aktuální poloha 96.86 %)
  - Speed: 10 %
  - Force: 2 %
  - Stav: `No object detected`

## 4. Dostupné script funkce

### Vestavěné URScript funkce (viditelné v nabídce Script)

- `get_target_joint_positions()`
- `get_target_joint_speeds()`
- `force()`
- `get_tcp_force()`
- `integer_to_binary_list(<integer>)`
- `binary_list_to_integer(<list>)`
- `d2r(<deg>)`
- `r2d(<rad>)`
- `read_port_bit(<address>)`
- `read_port_register(<address>)`

### Robotiq_Grippers – Robotiq Inc.

**Vakuové chapadlo**

- `rq_vacuum_grip()`
- `rq_vacuum_release()`
- `rq_is_vacuum_obj_detected()`

**Adaptivní chapadlo (Hand-E)**

- `rq_activate_and_wait()`
- `rq_reset()`
- `rq_open_and_wait()`
- `rq_close_and_wait()`
- `rq_move_and_wait_norm(<position>)`
- `rq_move_and_wait_mm(<position>)`
- `rq_move_and_wait_inches(<position>)`
- `rq_current_pos_norm()`
- `rq_current_pos_mm()`
- `rq_current_pos_inches()`
- `rq_is_object_detected()`
- `rq_set_force_norm(<force>)`
- `rq_set_speed_norm(<speed>)`
- `rq_set_gripper_max_cur(<current_mA>)`
- `rq_get_gripper_max_cur()`

**Komunikace**

- `rq_disconnect_grippers()`
- `rq_reconnect_grippers(<blocking>)`
- `rq_gripper_allow_tool_comm(<allow_tool_comm>)`

## 5. Pracoviště (prostředí)

| Prvek | Popis |
|---|---|
| Dopravník | Dorner 2200 Series (Hartland, WI, USA), žlutý pás |
| Senzor na dopravníku | optický senzor s červeným paprskem na konci dopravníku |
| Díly na dopravníku | bílé pěnové/plastové kvádry (tvar „L“ / bloky) |
| Podložka 1 | černá deska s nápisem „UNIVERSAL ROBOTS ACADEMY“ |
| Podložka 2 | černá deska s drážkou ve tvaru loga UR |
| Kovová šablona | nerezová deska s výřezem loga UR (před dopravníkem) |
| Modré kostky | modré plastové kvádry, stohované (3 ks) |
| Další | list bílého papíru na stole, klávesnice, monitor |

## 6. Školicí materiál

- Název: **Základní školení (e-Series) – Pracovní sešit**
- Jazyk: Česká verze
- Určení: školení v učebně nebo online na simulátoru

## 7. Shrnutí pro projekt

- Platforma: UR3e, PolyScope 5.21.2, IP `192.168.0.94`.
- Chapadlo: Robotiq Hand-E ovládané přes Robotiq URCap a funkce `rq_*`.
- Dostupné periferie: dopravník Dorner 2200 s optickým senzorem, polohovací desky UR Academy, bílé a modré díly.
