# PCB Workflow with Bouni’s Plugin and easyeda2kicad

This guide explains how to set up **Bouni’s KiCad JLCPCB Tools** and **easyeda2kicad** to prepare your KiCad projects for **JLCPCB assembly**.  
It’s written for beginners working with **KiCad 8** and aims to make the JLCPCB workflow straightforward and repeatable.

---

## 🧭 Overview

You’ll use two main tools:

- **[Bouni’s KiCad JLCPCB Tools](https://github.com/Bouni/kicad-jlcpcb-tools)**  
  → KiCad plugin that generates JLCPCB-ready BOM (Bill of Materials) and CPL (Pick-and-Place) files.

- **[easyeda2kicad](https://github.com/uPesy/easyeda2kicad.py)**  
  → Converts **LCSC** (JLCPCB) parts into KiCad-ready symbols, footprints, and 3D models.

---

## ⚙️ Step 1: Install Python and Pip

### macOS / Linux
```bash
python3 -m ensurepip --upgrade
pip3 --version
```

### Windows
1. Download and install from [python.org](https://www.python.org/downloads/).  
2. **Make sure to check “Add Python to PATH”** during installation.

---

## 🧩 Step 2: Install easyeda2kicad

```bash
pip install easyeda2kicad
```

If you see a warning like:

```
WARNING: The script easyeda2kicad is installed in '/Users/you/Library/Python/3.9/bin'
```

Add that directory to your PATH (for `zsh`):

```bash
echo 'export PATH=$PATH:/Users/you/Library/Python/3.9/bin' >> ~/.zshrc
source ~/.zshrc
```

---

## 🧱 Step 3: Generate KiCad Libraries

Each JLCPCB part has an **LCSC ID** (e.g., `C2040`).

Generate a full KiCad library for that part:

```bash
easyeda2kicad --full --lcsc_id=C2040
```

Optional: specify a custom output folder:

```bash
easyeda2kicad --full --lcsc_id=C2040 --output ~/KiCad/libs/my_parts
```

This generates:

```
my_parts.kicad_sym        # symbols
my_parts.pretty/          # footprints
my_parts.3dshapes/        # 3D models
```

---

## 🧭 Step 4: Add the Libraries into KiCad

### Add Symbol Library
In **KiCad**:
```
Preferences → Manage Symbol Libraries → Global
```

Add:
```
${EASYEDA2KICAD}/easyeda2kicad.kicad_sym
```

### Add Footprint Library
```
Preferences → Manage Footprint Libraries → Global
```

Add:
```
${EASYEDA2KICAD}/easyeda2kicad.pretty
```

---

## 🔌 Step 5: Install Bouni’s JLCPCB Tools Plugin

1. In **KiCad**, open:  
   `Tools → Plugin and Content Manager`
2. Click **Manage Repositories → + → Add URL**:

   ```
   https://raw.githubusercontent.com/Bouni/bouni-kicad-repository/main/repository.json
   ```
3. Switch to **Bouni’s Repository** in the dropdown.
4. Locate **KiCad JLCPCB Tools → Install**.
5. Apply changes and **restart KiCad**.

You’ll now see a **blue JLC icon** in the PCB Editor toolbar.

---

## 🧾 Step 6: Assign LCSC Numbers & Export

1. In your schematic, add a new **field** named `LCSC`.  
2. Enter each component’s LCSC ID (e.g., `C2040`).
3. After footprints are assigned, open **PCB Editor → Bouni’s Plugin**.
4. Generate:
   - **BOM (Bill of Materials)**
   - **CPL (Pick-and-Place)**

These are JLCPCB-ready export files.

---

## 🚀 Step 7: Upload to JLCPCB

When ordering assembly at [**jlcpcb.com**](https://jlcpcb.com):

Upload:
- Gerber files  
- BOM file  
- CPL file  

JLCPCB will automatically match your components via the **LCSC IDs**.

---

## 💡 Additional Tips

- Regenerate specific parts anytime:
  ```bash
  easyeda2kicad --footprint --3d --lcsc_id=C2040 --overwrite
  ```

- Always verify **footprint orientation** before submitting your board.
- Keep your generated libraries in a global folder (e.g. `~/KiCad/libs`) for easy reuse.
- Use **version control (Git)** to track changes to footprints and symbols.

---

## ⚡ Quick Reference Commands

| Action | Command |
|--------|----------|
| Install easyeda2kicad | `pip install easyeda2kicad` |
| Generate full KiCad part | `easyeda2kicad --full --lcsc_id=Cxxxx` |
| Generate into custom folder | `easyeda2kicad --full --lcsc_id=Cxxxx --output ~/KiCad/libs/my_parts` |
| Regenerate with overwrite | `easyeda2kicad --footprint --3d --lcsc_id=Cxxxx --overwrite` |
| Add to PATH (macOS zsh) | `echo 'export PATH=$PATH:/Users/you/Library/Python/3.x/bin' >> ~/.zshrc && source ~/.zshrc` |

---

## 🙌 Credits

- **Bouni’s Plugin:** [github.com/Bouni/kicad-jlcpcb-tools](https://github.com/Bouni/kicad-jlcpcb-tools)  
- **easyeda2kicad:** [github.com/uPesy/easyeda2kicad.py](https://github.com/uPesy/easyeda2kicad.py)
