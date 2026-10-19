# DocPerfect 1.0.0 — Windows release procedure

These are sources and Windows build scripts, not prebuilt Windows binaries. Previous RC4 GUI, CLI, MCP, portable and installer ran successfully on the developer's Windows machine. The final **1.0.0** artifacts must be rebuilt and checked separately.

## Build from source on Windows 10/11 x64

From the project root in PowerShell:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
.\.venv\Scripts\python.exe -m pip install -e ".[dev,gui,agent]" "mcp[cli]>=1.15,<2"
.\.venv\Scripts\python.exe -m pytest -q
.\scripts\build_windows.ps1
.\scripts\test_build_windows.ps1
.\scripts\package_release_windows.ps1
```

The `build_windows.ps1` script installs PyInstaller automatically; if `.venv` does not exist, it creates one using `py -3.12`. Run `test_build_windows.ps1` before packaging; it closes the GUI process it starts to avoid ZIP locks. Close any other running DocPerfect instance. The packaging script auto-detects Inno Setup 6 in per-user and machine-wide installation locations; alternatively pass `-InnoCompiler 'C:\path\ISCC.exe'`. `-PortableOnly` creates only ZIP and checksums.

Output: `dist\release\v1.0.0\DocPerfect_1.0.0_Windows_x64_Portable.zip`, `dist\release\v1.0.0\DocPerfect_Setup_1.0.0_Windows_x64.exe`, `SHA256SUMS.txt`.

## Release checks

1. Confirm `pip show docperfect` and UI/About screen both display **1.0.0** (no RC suffix).
2. Execute all pytest tests on Windows and record count and any skips.
3. Test create/read/validate/repair with a real multi-paragraph DOCX and confirm the original SHA-256 does not change.
4. Unpack the **new 1.0.0** portable ZIP to an isolated directory; verify GUI, styles, splash, CLI and MCP without activating the venv.
5. Install **new 1.0.0** installer (not previous RC4), run app via Start Menu, check document operations, and uninstall. Confirm user's documents are unaffected.
6. Verify SHA-256 manifest and scan packaged binaries before uploading. EXEs are **unsigned** and may trigger SmartScreen.
7. Test with a compatible real MCP client when available; packaged MCP stdio smoke test alone is not a substitute for that integration.
8. Obtain explicit publishing permission, repository destination, and licensing decision before making a public release. Do not upload user files, .venv, logs, or secrets.

Limits: structural document preview is not pixel-exact Microsoft Word; complicated numbering, images, comments, headers and shapes may require manual review. Program's offline handling does not prevent cloud agents from sharing text with their providers.
