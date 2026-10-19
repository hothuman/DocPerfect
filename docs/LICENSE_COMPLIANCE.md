# DocPerfect — proprietary distribution and third-party licensing

**Status: legal/third-party license clearance OPEN. Do not represent this
project as cleared for public binary redistribution until the checklist is
completed by the actual rights holder.** This is an engineering audit, not
legal advice.

## 1. DocPerfect rights (separate from dependencies)

- `LICENSE` declares proprietary rights; it is deliberately **not** MIT/GPL.
- `EULA.txt` proposes narrow permission for users to run official binaries.
- Public GitHub source visibility does not grant a general license to reuse
  DocPerfect code. GitHub platform terms and mandatory legal rights still apply.
- Confirm actual natural/legal person holding copyrights; replace placeholder
  in `LICENSE` and update EULA/README contact as appropriate.
- Confirm rights/provenance for generated logo, startup animation, code and
  any contributed assets, and ensure no third-party source was reused without
  permission. AI-generated assets may have limitations on exclusivity.

## 2. The critical issue: PySide6 / Qt

Official Qt materials confirm community PySide6/Qt uses LGPLv3/GPL (with
commercial alternatives); some Qt modules are GPL-only in community builds.
A proprietary application may use LGPL libraries **if all LGPL conditions are
met**. The project currently depends on community PySide6 from PyPI.

BEFORE ANY PUBLIC BINARY DISTRIBUTION:

- [ ] Record exact PySide6/Qt/Shiboken versions for Windows **and** Ubuntu.
- [ ] Review the *actual shipped Qt DLLs/.so and plugins*, including components
      imported by PyInstaller hooks. Identify any GPL-only modules; remove
      unnecessary components or obtain rights rather than assuming LGPL.
- [ ] Include copies of relevant LGPLv3/GPLv3 (and relevant underlying GPLv3)
      license texts; see the bundled `licenses/` texts and wheel notices.
- [ ] State clearly that Qt and its other open-source components are licensed
      separately and users have rights to modify/relink/replace LGPL libraries.
- [ ] Confirm relinking/replacement remains practically possible in the final
      installed application; test replacing a compatible Qt dynamic library
      in an **independent disposable test installation**. Never assume the
      PyInstaller build layout automatically establishes compliance.
- [ ] Provide under **the distributor's control** the complete relevant Qt
      Corresponding Source (including changes) or a valid actionable written
      offer. Linking only to upstream Qt downloads is insufficient according
      to Qt's own FAQ. `docs/QT_SOURCE_OFFER_TEMPLATE.md` is only a draft.
- [ ] Assess whether any Installation Information obligations apply.
- [ ] Review Qt's included third-party code, translations, plugins and
      platform-specific libraries; preserve notices/source obligations.
- [ ] Have qualified counsel review the finished distribution or secure an
      appropriate Qt commercial license where the LGPL model doesn't fit.

Official sources:
https://www.qt.io/development/open-source-lgpl-obligations
https://www.qt.io/faq/qt-open-source-licensing
https://doc.qt.io/qt-6/licensing.html
https://doc.qt.io/qtforpython-6.10/commercial/index.html

## 3. Other dependencies

- `python-docx`: MIT; `lxml`: BSD + third-party exceptions; `pydantic` and
  `pydantic-core`: MIT; MCP Python SDK: MIT. Preserve license texts and notices.
- PyInstaller: GPL + bootloader distribution exception permits closed-source
  frozen binaries; changes to PyInstaller itself are a separate issue.
- Inno Setup can be used commercially under its own license.
- Bundled Python, OpenSSL, libxml2, libxslt, Qt plugins and other dependencies
  have **their own** requirements. Check actual binaries rather than relying
  only on `pyproject.toml` or an example manifest.
- Run `python scripts/license_inventory.py --out dist/DocPerfect/legal` in the
  exact release-build environment. It copies discovered wheel license files,
  outputs a JSON summary and inventories bundled Qt files. Investigate its
  `missing_license_files` before publishing. Run `scripts/qt_module_audit.py --root dist/DocPerfect` as well; it blocks obvious GPL-only community module filenames but does **not** replace full manual review.
  In particular, these scripts do *not* download Qt source or prove relinkability.

## 4. Publication gate

- [ ] Confirm legal name/contact of actual copyright holder, assets and
      third-party code provenance.
- [ ] Complete Qt checklist and source availability offer, including an
      owner-controlled source delivery route with retained required files.
- [ ] Rebuild Windows EXE+portable+installer and Ubuntu .deb+portable with
      `EULA.txt`, `THIRD_PARTY_NOTICES.md` and license inventory.
- [ ] Inspect shipping `dist/DocPerfect/_internal` libraries; compare inventory
      with `THIRD_PARTY_NOTICES.md`. Test Qt library replaceability.
- [ ] Run Windows test_build, Ubuntu 22.04 GUI+CLI+MCP, installed .deb on 24.04,
      and verify final installer uninstall preserves user documents.
- [ ] Check SHA-256 hashes from actual release files.
- [ ] **Only after all above:** manually approve publication. Automated tag
      pushes **build but never directly publish**; manual GitHub Actions publish
      requires owner-set repository flags and complete `docs/QT_LGPL_COMPLIANCE_COMPLETE.md` evidence.

The repository's README may be public for review before these checks. Do not
attach binary releases as 'license cleared' while this gate is incomplete.
