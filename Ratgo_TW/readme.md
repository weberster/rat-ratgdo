PCB Workflow with Bouni’s Plugin and easyeda2kicad
This guide explains how to set up Bouni’s KiCad JLCPCB Tools and easyeda2kicad when designing PCBs for JLCPCB assembly.
It’s written for beginners new to electronic design workflows using KiCad 8.

Overview
You’ll use:

Bouni’s KiCad JLCPCB Tools → integrates with KiCad to generate JLCPCB‑ready BOM and placement files.

easyeda2kicad → converts LCSC (JLCPCB) components into KiCad‑ready symbols, footprints, and 3D models.

Step 1: Install Python and Pip
macOS / Linux
python3 -m ensurepip --upgrade
pip3 --version

Windows
Download from python.org and check Add Python to PATH during installation.

Step 2: Install easyeda2kicad
pip install easyeda2kicad

If you see warnings like:
WARNING: The script easyeda2kicad is installed in '/Users/you/Library/Python/3.9/bin'

Add that directory to your PATH (for zsh):

echo 'export PATH=$PATH:/Users/you/Library/Python/3.9/bin' >> ~/.zshrc
source ~/.zshrc

Step 3: Generate KiCad Libraries
Each JLCPCB part has an LCSC ID (e.g., C2040).
Run this command to generate full KiCad libraries for that part:

easyeda2kicad --full --lcsc_id=C2040

You can customize the output folder:

easyeda2kicad --full --lcsc_id=C2040 --output ~/KiCad/libs/my_parts

Generated files include:
my_parts.kicad_sym # symbols
my_parts.pretty/ # footprints
my_parts.3dshapes/ # 3D models

Step 4: Add the Libraries into KiCad
Add Symbol Library
Preferences → Manage Symbol Libraries → Global

Add: ${EASYEDA2KICAD}/easyeda2kicad.kicad_sym

Add Footprint Library
Preferences → Manage Footprint Libraries → Global

Add: ${EASYEDA2KICAD}/easyeda2kicad.pretty

Step 5: Install Bouni’s JLCPCB Tools Plugin
Open KiCad → Tools → Plugin and Content Manager

Click Manage Repositories → + → Add URL:
https://raw.githubusercontent.com/Bouni/bouni-kicad-repository/main/repository.json

Switch to Bouni’s Repository in the dropdown.

Locate KiCad JLCPCB Tools → click Install.

Apply changes and restart KiCad.

You should now see a blue JLC icon in the PCB Editor toolbar.

Step 6: Assign LCSC Numbers & Export
In your schematic, add a field named “LCSC”.

Enter each component’s LCSC ID (e.g., C2040).

With footprints assigned, open PCB Editor → Bouni’s Plugin.

Generate:

BOM (Bill of Materials)

CPL (Pick‑and‑Place)

These files are formatted for JLCPCB assembly upload.

Step 7: Upload to JLCPCB
When ordering at JLCPCB.com, upload:

Gerber files

BOM file

CPL file

Their assembly service matches your parts automatically using LCSC IDs.

Additional Tips
Regenerate specific parts anytime:
easyeda2kicad --footprint --3d --lcsc_id=C2040 --overwrite

Always verify footprint orientation before finalizing.

Keep libraries organized in a global KiCad path for reuse.

Credits
Bouni’s Plugin: https://github.com/Bouni/kicad-jlcpcb-tools

easyeda2kicad: https://github.com/uPesy/easyeda2kicad.py

