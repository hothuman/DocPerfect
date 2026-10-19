# Third-party components — DocPerfect 1.0.0

**This file is not a license for the proprietary DocPerfect source code.**
The listed components and any other third-party files remain under their own
licenses. The **exact shipping dependency inventory** must be generated from
both final Windows and Ubuntu outputs, including transitive and native Qt
modules, before distributing binaries. `scripts/license_inventory.py` produces
an inventory of the build environment and a list of detected license files;
**that inventory by itself does not establish license compliance**.

| Component | License family (upstream) | Distribution note |
|---|---|---|
| PySide6, Shiboken6, Qt Essentials/Addons | LGPL-3.0 / GPL / commercial, **module-specific** | Check *every bundled Qt library* and comply with LGPLv3, or secure appropriate commercial licensing. |
| python-docx | MIT | Preserve applicable copyright and license text. |
| lxml | BSD-based, with third-party parts | Inspect `LICENSES.txt` and bundled libxml2/libxslt/native dependencies. |
| Pydantic / pydantic-core | MIT | Preserve notices and license text. |
| MCP Python SDK | MIT | Preserve notices and license text. |
| Python runtime (CPython) | PSF and bundled third-party licenses | Preserve Python license/third-party notices. |
| PyInstaller (build tool and embedded bootloader) | GPL-2.0-or-later with Bootloader exception | The exception permits bundling proprietary applications; respect conditions of redistributed components. |
| Inno Setup (installer generator) | Inno Setup license | Commercial installers are permitted; retain required tool notices where applicable. |
| Pillow, pytest, numpy (development/test dependencies) | Package-specific licenses | Only include if actually packaged. |
| Additional transitive packages, Qt plugins, OpenSSL etc. | Package-specific | Confirm from frozen artifact and wheels; this table is not exhaustive. |

## Binding upstream references

- https://www.qt.io/development/open-source-lgpl-obligations
- https://www.qt.io/faq/qt-open-source-licensing
- https://doc.qt.io/qtforpython-6.10/commercial/index.html
- https://github.com/python-openxml/python-docx/blob/master/LICENSE
- https://github.com/lxml/lxml/blob/master/LICENSES.txt
- https://github.com/pydantic/pydantic/blob/main/LICENSE
- https://github.com/modelcontextprotocol/python-sdk/blob/main/LICENSE
- https://github.com/pyinstaller/pyinstaller/blob/develop/COPYING.txt
- https://github.com/jrsoftware/issrc/blob/main/license.txt

**LGPL Qt distribution checklist:** provide appropriate notices and license
copies, permit replacement/modification of covered libraries, provide Qt
Corresponding Source (including relevant changes) or a genuinely actionable
written offer under the distributor's control, and review installation
information duties, GPL-only modules and third-party native libraries.
A link to qt.io **alone does not fulfill** the source availability requirement
as described by Qt. See `docs/LICENSE_COMPLIANCE.md`.
