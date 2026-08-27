# Corne-ish Zen Custom Configuration

![Corne-ish Zen Logo](zenlogo.png)

This repo is the official configuration of the Corne-ish Zen low profile wireless mechanical keyboard. Use it to develop your own keymap and easily build your own ZMK firmware to run on your Corne-ish Zen. These steps will get you using your keymap on your keyboard in the fastest time possible. It uses the GitHub Actions feature to build your firmware online, rather than setting up a complex tool chain on your local computer.
If you are looking to dig deeper into ZMK and develop new functionality, it is recommended to follow the steps of installing ZMK as found on the official ZMK documentation site (linked below).

**Note:** This process is temporary, and will be used until such time that the Corne-ish Zen board definition is merged into ZMK Main.

## Resources

- The [official ZMK Firmware GitHub](https://github.com/zmkfirmware/zmk) repository. View the keymaps for other boards and shields as a starting point for your keymap.
- The [official ZMK Documentation](https://zmk.dev/docs) web site. Find the answers to many of your questions about ZMK Firmware.
- The [official ZMK Discord Server](https://discord.gg/8cfMkQksSB). Instant conversations with other ZMK developers and users. Great technical resource!

## Instructions

1. Log into, or sign up for, your personal GitHub account.
2. Fork this repository to your local computer, and then push it to your GitHub personal account. ([instructions](https://docs.github.com/en/get-started/quickstart/fork-a-repo))
3. Edit the keymap file(s) to suit your needs
4. Commit and push your changes to your personal repo. Upon pushing it, GitHub Actions will start building a new version of your firmware with the updated keymap.

## Firmware Files

To locate your firmware files...

1. log into GitHub and navigate to your personal config repository you just uploaded your keymap changes to.
2. Click "Actions" in the main navigation, and in the left navigation click the "Build" link.
3. Select the desired workflow run in the centre area of the page (based on date and time of the build you wish to use). You can also start a new build from this page by clicking the "Run workflow" button.
4. After clicking the desired workflow run, you should be presented with a section at the bottom of the page called "Artifacts". This section contains the results of your build, in a file called "firmware.zip"
5. Download the firmware zip archive and extract the two .uf2 files. They are named according to which side they need to be flashed to.
6. Flash the firmware to your keyboard by double-clicking the reset button to put the it in bootloader mode. A window should pop up showing the contents of the storage on the keyboard. Drag and drop the correct .uf2 file into the window. When the upload is complete the window will close and the keyboard will exit bootloader mode.

Your keyboard is now ready to use.

## Keymap

The shared keymap uses a native ZMK translation of [Mark Stosberg's Corne 3x5+1 v2.2 layout](https://mark.stosberg.com/markstos-corne-3x5-1-keyboard-layout/). The goal is to stay close to a normal QWERTY keyboard while providing simple switching between five paired computers.

`config/corne-ish_zen.keymap` remains the only shared keymap. Both `config/corne-ish_zen_left.keymap` and `config/corne-ish_zen_right.keymap` continue to include it.

The four layers are declared and numbered in this order:

```text
BASE = 0
LOWER = 1
RAISE = 2
FUNCTION = 3
```

The old `COL`, `NAV`, `NUM`, and invalid `CONFIG = 4` definitions have been removed.

### Base

```text
 TAB     Q     W     E     R     T       Y     U     I     O     P     DEL
 ALT     A     S     D     F     G       H     J     K     L     '     RALT
 SHIFT   Z     X     C     V     B       N     M     ,     .     /     FUNC

                 CTRL  GUI/ENTER  LOWER/TAB   RAISE/BSPC  SPACE  SHIFT
```

Alt, Ctrl, and Shift are sticky modifiers. `GUI/ENTER`, `LOWER/TAB`, and `RAISE/BSPC` are thumb tap-hold keys. There are no home-row modifiers.

The only combo is `J+K` for Escape. It is active only on Base.

### Lower

Hold `LOWER/TAB` to enter Lower. Numbers stay in the same left-to-right columns as a normal keyboard.

```text
 TRANS   !     @     #     $     %       ^     &     *     (     )     TRANS
 TRANS   1     2     3     4     5       6     7     8     9     0     TRANS
 TRANS  NONE   ~     `     [     {       }     ]     ,     .     /     TRANS

                TRANS   TRANS   LOWER     TRANS   TRANS   COLON
```

### Raise

Hold `RAISE/BSPC` to enter Raise. The arrow keys use the physical H/J/K/L positions.

```text
 TRANS  DEL    INS    _      +      PGUP    NONE   NONE   NONE   BSLH   PIPE   TRANS
 TRANS  HOME   END    -      =      PGDN    LEFT   DOWN   UP     RIGHT  MENU   TRANS
 TRANS  <      >      COPY   PASTE  ;       PLAY   PREV   NEXT   VOLDN  VOLUP  TRANS

                CTRL/ESC   TRANS   NONE      RAISE   TRANS   TRANS
```

Copy and Paste use macOS Command+C and Command+V so they work consistently with the existing macOS setup.

### Function and Bluetooth

Tap `FUNC` for the next Function key, or hold it while pressing another key. F1-F12 use the complete physical top row.

```text
 F1      F2    F3    F4    F5    F6      F7    F8    F9    F10   F11    F12
 BT_CLR  BT1   BT2   BT3   BT4   BT5     NONE  NONE  NONE  NONE  NONE   NONE
 NONE    CAPS  NONE  NONE  NONE  NONE     NONE  NONE  NONE  NONE  RESET  NONE

                 NONE   NONE   NONE       NONE   NONE   NONE
```

The labels `BT1` through `BT5` map to ZMK profiles `BT_SEL 0` through `BT_SEL 4`:

| Computer | Hold Function and press | ZMK profile |
| --- | --- | --- |
| 1 | A | `BT_SEL 0` |
| 2 | S | `BT_SEL 1` |
| 3 | D | `BT_SEL 2` |
| 4 | F | `BT_SEL 3` |
| 5 | G | `BT_SEL 4` |

To switch computers, hold Function and press A, S, D, F, or G.

To replace a pairing:

1. Select the intended profile with Function and A, S, D, F, or G.
2. Enter Function again and press the far-left home-row key marked `BT_CLR`.
3. Start pairing from the new computer.

`BT_CLR` changes pairing state. It is intentionally separated from the profile keys to reduce accidental activation. Reset is also isolated on the Function layer and must not be used for normal profile switching.

### ZMK Behavior

The implementation uses behavior support already present in the pinned LOWPROKB ZMK fork:

- Sticky modifiers expire after 2000 ms.
- Thumb layer-taps are hold-preferred with a 125 ms tapping term.
- Modifier-taps are hold-preferred with a 200 ms tapping term.
- The Base-only `J+K` Escape combo has a 40 ms timeout.
- ZMK sticky modifiers do not reproduce QMK's triple-tap lock. Caps Lock remains available on Function.

## Implementation Notes

- `config/corne-ish_zen.keymap` defines the four sequential layers and native ZMK behaviors described above.
- The old home-row-mod behavior, Miryoku-era layers, workspace shortcuts, and symbol combos have been removed.
- macOS copy and paste, Function-layer reset access, and Mark Stosberg attribution are preserved.
- `config/original.keymap`, both side-specific include files, `config/west.yml`, and the build workflow remain unchanged.
- Both firmware halves must pass the matrix audit and build before handoff.

### Verification

Before handoff:

- Confirm there are exactly four layers and 42 bindings per layer.
- Confirm all layer references resolve to 0 through 3.
- Confirm there are no home-row mods and only one combo.
- Confirm `BT_SEL 0` through `BT_SEL 4` each appear once.
- Confirm `BT_CLR` and reset each appear once in protected positions.
- Build `corne-ish_zen_left` with the existing west configuration.
- Build `corne-ish_zen_right` from a pristine build directory.
- Run the local reviewer against `main` and address all critical findings.

The following checks require user-controlled flashing and hardware:

- Verify quick and held behavior for Enter, Tab, Backspace, Lower, and Raise.
- Verify every Lower, Raise, and Function key.
- Pair at least two computers and switch between them repeatedly.
- Clear and re-pair one profile, then confirm the untouched profile still reconnects.
- Confirm reset cannot be triggered during ordinary typing or profile switching.

### Compatibility Boundaries

This change does not migrate to upstream ZMK, update the pinned LOWPROKB fork, modernize GitHub Actions, change hardware, or modify external repositories. Those are separate projects.

Existing Bluetooth profile numbers are preserved, but bond data is not guaranteed to survive every firmware update. If stored bonds disagree after flashing, select the intended profile, clear only that profile, and pair it again.

Both halves must use firmware built from the same commit. `config/original.keymap` remains unchanged as the historical reference.
