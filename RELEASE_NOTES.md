# AI Agent Secure v1.1.4

## Fixes

- Extended the PowerShell UTF-8 runtime guard to block inline `.NET` text writes such as `WriteAllText` when the command does not visibly name a UTF-8 encoding.
- Tightened PowerShell parsing so no-space redirection/pipelines are tokenized and UTF-8 encoding signals only satisfy writes in the same command/write call.
- Updated the GUI PowerShell UTF-8 details to name `.NET` text-write coverage alongside cmdlet, redirection, CP1252/ANSI, and UTF-16 BOM corruption.
- Clarified the runtime PowerShell block message so blocked writes mention explicit UTF-8 encoding and include a safe `.NET` `UTF8Encoding` example.

## Verification

- `powershell -NoProfile -ExecutionPolicy Bypass -File .\tests\run-quality.ps1 -NoColor -BuildGui`
- Isolated runtime repro: `Set-Content` without `-Encoding utf8` blocked, target file not written, and `Set-Content -Encoding utf8` allowed.
- Isolated edge repros: `Set-Content`, `Add-Content`, `Out-File -Encoding ASCII`, spaced/no-space `>` redirection, no-space pipelines, read-side `-Encoding utf8`, multi-write mismatch, `.NET` text writes without visible UTF-8, and `.NET` ASCII writes blocked; explicit `.NET` UTF-8 writes allowed; CP1252 PHP and UTF-8 BOM files rejected by source-encoding QA.
