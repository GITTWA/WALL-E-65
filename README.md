# WALL-E 65 — Firmware ZMK (`ITA ISO` per Windows)

Firmware ZMK personalizzato per la tastiera meccanica wireless unibody **WALL-E 65** (68 tasti, layout **Italiano ISO**), basato su microcontrollore **`nice_nano_v2` / SuperMini nRF52840** e **ZMK `v0.3`**.

> ⚠️ **Compatibilità:** Questa configurazione è progettata e ottimizzata **esclusivamente per Windows** con layout di sistema impostato su **Italiano (`ITA ISO`)**.

---

## Modulo richiesto: Caps Lock Accentate

Questa tastiera utilizza il modulo personalizzato **[`zmk-behavior-CAPS-LOKS-ACCENTATE`](https://github.com/GITTWA/zmk-behavior-CAPS-LOKS-ACCENTATE)** per ottenere le vocali accentate maiuscole automatiche con il 
Caps Lock attivo su Windows.

👉 **Per istruzioni di installazione, dettagli tecnici, compatibilità, riassociazione Bluetooth iniziale e uso su altri layout, 
consulta la [documentazione ufficiale del modulo](https://github.com/GITTWA/zmk-behavior-CAPS-LOKS-ACCENTATE/blob/main/README.md).**

---

## Funzioni Extra e Differenze rispetto a una normale tastiera Windows

Essendo una tastiera compatta **65% (68 tasti)**, oltre ai normali tasti alfanumerici integra diverse funzioni personalizzate che **non** esistono (o funzionano diversamente) su una tastiera Windows standard:

### 1. Vocali accentate maiuscole automatiche con Caps Lock *(via modulo)*
Su una normale tastiera Windows italiana, premere `è`, `à`, `ò`, `ù`, `ì` con il Caps Lock acceso produce comunque lettere minuscole. Sulla **WALL-E 65**, leggendo lo stato reale del Caps Lock dal PC:
- **Caps Lock ON:** `è` → **`È`**, `à` → **`À`**, `ò` → **`Ò`**, `ù` → **`Ù`**, `ì` → **`Ì`**
- **Caps Lock ON + Shift:** `è` → **`É`** *(mentre sulle altre restano i simboli normali `°`, `ç`, `§`, `^`)*

### 2. Tasto `< > \ |` unificato (`iso_slash`)
Nei formati 65% manca il tasto dedicato a `\` e `|` (che su una tastiera Windows standard si trova sotto `ESC`). I quattro simboli sono stati riuniti tutti sul tasto **`< >`** accanto a `Shift SX`:
- **Da solo:** `<`
- **`Shift + <`:** `>`
- **`AltGr + <`:** **`\`** *(barra rovesciata)*
- **`AltGr + Shift + <`:** **`|`** *(pipe)*

### 3. Colonna destra multifunzione (`Stamp / Pausa` e `Muto / Ins`)
I tre tasti verticali sul bordo destro della tastiera svolgono 5 funzioni:
- **1° tasto in alto (`Stamp / Pausa`):**
  - Da solo → **`Stamp`** (`Print Screen`)
  - **`Shift` + tasto** → **`Pausa`** (`Pause / Break`)
- **2° tasto al centro:** **`Canc`** (`Delete`)
- **3° tasto in basso (`Muto / Ins`):**
  - Da solo → **`Muto`** audio (`Mute` toggle)
  - **`Shift` + tasto** → **`Ins`** (`Insert`)

### 4. Layer `FN` (Livello 1 — entrambi i tasti `FN` ai lati della barra spaziatrice)
Tenendo premuto uno qualsiasi dei due tasti **`FN`**:
- **Tasti Funzione `F1` – `F11`:** sui tasti numerici da `1` a `'` (il tasto dopo lo `0`).
- **Tasto `ì` con doppio comportamento (*Tap-Dance*, finestra 250 ms):**
  - **`FN` + 1 tocco su `ì`** → **`Ì`** (`Alt+0204`)
  - **`FN` + 2 tocchi rapidi su `ì`** → **`F12`**
- **Vocali accentate maiuscole rapide (anche con Caps Lock spento):**
  - `FN + è` → **`È`**, `FN + ò` → **`Ò`**, `FN + à` → **`À`**, `FN + ù` → **`Ù`**, `FN + ì` (1 tocco) → **`Ì`**
- **Navigazione sulle frecce:**
  - `FN + ↑` → **`Pag↑`** (`Page Up`)
  - `FN + ↓` → **`Pag↓`** (`Page Down`)
  - `FN + ←` → **`Home`** (inizio riga)
  - `FN + →` → **`Fine`** (`End`, fine riga)

### 5. "Cassaforte" di Amministrazione e Bluetooth (Livello 2 — Combo a 4 tasti)
Per evitare pressioni accidentali di reset o cambio profilo Bluetooth, le funzioni di sistema sono protette dietro una combinazione a 4 tasti (*Sticky Layer*, finestra 150 ms):

1. Premi contemporaneamente **`Shift DX` + `AltGr` + `FN DX` + `Ctrl DX`** e poi rilasciali (il livello resta attivo per il comando successivo e poi si chiude da solo).
2. Premi il comando desiderato:
   - **`1` – `5`** → Seleziona il profilo Bluetooth **`0` – `4`** (`BT_SEL 0..4`)
   - **`Tab`** → Commuta l'uscita attiva tra **Cavo USB ⇄ Bluetooth** (`OUT_TOG`)
   - **`Canc`** *(2° tasto colonna destra)* → Cancella l'associazione del profilo Bluetooth attivo (`BT_CLR`)
   - **`Stamp`** *(1° tasto colonna destra)* → Riavvia in **modalità Bootloader** (`&bootloader`) per caricare un nuovo file `.uf2` senza dover aprire la tastiera

### 6. Funzioni firmware e hardware integrate
- **Bluetooth potenziato e a bassa latenza:** potenza TX a `+8 dBm` e intervallo di connessione `7.5–15 ms`.
- **Deep Sleep automatico:** spegnimento profondo dopo **10 minuti** (`600.000 ms`) di inattività per preservare la batteria.
- **Livello Batteria + ZMK Studio:** reporting batteria attivo e **ZMK Studio** sbloccato (via USB) per eventuali ispezioni o regolazioni rapide.
- **Compatibilità SuperMini nRF52840:** regolatori DCDC disabilitati via firmware (`CONFIG_BOARD_ENABLE_DCDC=n` e `CONFIG_BOARD_ENABLE_DCDC_HV=n`) per prevenire blocchi/crash sui cloni `nice_nano_v2`.

> **Nota sulla configurazione attuale:** nella mappatura corrente non è assegnato il tasto `Windows` (`LGUI`).

---

## Struttura del repository

```text
.
├── .github/workflows/
│   └── build.yaml                          # Compilazione automatica GitHub Actions (ZMK v0.3)
├── boards/shields/wall_e_65/
│   ├── Kconfig.defconfig                   # Nome Bluetooth/USB ("WALL-E 65")
│   ├── Kconfig.shield                      # Definizione shield wall_e_65
│   ├── wall_e_65.conf                      # Opzioni hardware, BT, sleep, batteria, ZMK Studio
│   ├── wall_e_65.keymap                    # Mappa tasti, macro, combo e layer
│   └── wall_e_65.overlay                   # Matrice 10x8, pin GPIO nRF52840 e layout fisico
├── config/
│   └── west.yml                            # Dipendenze ZMK v0.3 e modulo CAPS-LOKS-ACCENTATE
├── zephyr/
│   └── module.yml                          # Espone la cartella boards/shields al build system
├── build.yaml                              # Target di compilazione (nice_nano_v2 + wall_e_65 + studio)
└── README.md
```

---

## Compilazione e Flash del Firmware

1. Ogni `push` su GitHub avvia automaticamente la compilazione nella tab **Actions**.
2. A compilazione completata (icona verde), scarica il pacchetto `.zip` negli **Artifacts** ed estrai il file `.uf2`.
3. Collega la tastiera via USB ed entra in modalità Bootloader usando la combo **`Shift DX + AltGr + FN DX + Ctrl DX`** seguita da **`Stamp`** (oppure con il doppio reset fisico sulla scheda).
4. Copia il file `.uf2` dentro l'unità USB che appare su Windows.
5. **Solo al primo avvio con il modulo Caps Lock Accentate su Bluetooth:** rimuovi la `WALL-E 65` dalle impostazioni Bluetooth di Windows, esegui `Combo Cassaforte + Canc` (`BT_CLR`) sulla tastiera e rifai l'accoppiamento (vedi le istruzioni nella [documentazione del modulo](https://github.com/GITTWA/zmk-behavior-CAPS-LOKS-ACCENTATE/blob/main/README.md)).
