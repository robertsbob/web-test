# Pentest speedrun instructions

## Purpose

You are speedrunning vulnerability testing. Use the 20/80 method — 20% of techniques to find 80% of vulns. The pentester (user) will run manual exploitation in the background. You are on Kali Linux. There will be around 8 vulnerabilities to find — order does not matter.

Do not stop running at any point until explicitly stopped.

The scope, target and any starting data will be provided by the user in their first chat message.

## Documenting vulnerabilities

For each vulnerability, provide:

1. Vulnerability name (expand abbreviations, e.g. XXE → XML External Entity)
2. Simple description
3. PoC — prefer payloads that can be entered manually for the vulnerability presentation
4. Potential impact
5. Mitigation

All vulns must have a concrete, working way to demonstrate exploitation. All vulns must also be displayed in chat (user may check chat more often than files).

The findings will be presented and demonstrated later, so steps to reproduce must be exact and unambiguous — exact URLs, exact payloads, exact commands, copy-pasteable.

### VULN file template

```
# VULN: [Name] ([Abbreviation expanded])

## Reproduce
1. Step...
2. Step...

## PoC
[exact payload or command]

## Evidence
[response snippet or observation proving it worked]

## Description
[short explanation on what this vulnerability is]

## Impact
...

## Mitigation
...
```

## Formatting rules

- Avoid tables in all output files — reports will likely be viewed in a simple text editor.
- Keep files brief and human-readable.

## Communication with the user

If you need inputs or tools you don't have (e.g. a cookie from Burp Suite), ask the user. However, **do not stop running** — add the request to the `USER_INPUT_...` file and continue working on other things. Everything asked in the file must also be displayed in chat.

The user may post elapsed time in chat. If a lot of time has passed on a single thread without progress, move on.

## General rules

- If you need Python-specific libraries, install them — just create a venv first (put it in the artifacts folder).
- **NEVER** use `alert()`/`confirm()`/`prompt()` in injected JS (XSS or other) — they freeze the browser. Use another method.
- Try to run tools, especially browser tools, together with other tools in parallel.
- For `curl` where a cookie is needed, use `-H "Cookie: <value>"`.

## Reconnaissance

Start by mapping and recon. Use automated tools like:

- `nmap`, nmap scripts
- `whatweb`, `nikto`
- `gobuster`, `ffuf`
- `sqlmap`
- Browser tool (important — some vulns can only be found through manual exploitation)
- Any other tools that are relevant

## Restrictions

- **No password brute-force** (`wfuzz`, `hydra`, `medusa`) — risk of account lockout. Exception: local brute-force or when specifically allowed by the user.
- **No DoS or flood tools** (`slowloris`, `hping3`, `ab`).
- If the target returns **HTTP 429** or starts blocking requests — stop immediately, report it, and move on.
- Do not disrupt the application or the VM.
- Do not need to be stealthy, just not destructive.
- Do not spend too much time on one task — move often.

## File structure

### Current folder

Your home. Do not create new files here. You may edit `claude.md` only if specifically instructed.

### User

For file presentations to the user. All files here must be brief and human-readable.

- `VULN_...` — One file per confirmed, exploited vulnerability. Reproducing steps first, then explanations. Created as soon as you find and exploit it.
- `VULNNON_...` — One file per confirmed vulnerability or security misconfig where you are confident it is a vulnerability but exploiting it is impractical or would take too long. These are not loose threads.
- `MAYBE_` — Single file for loose threads — things you suspect but have not confirmed. Add threads immediately; close them by creating a `VULN_` or `VULNNON_` file when resolved. Must be brief.
- `USER_INPUT_...` — Single file for requesting data from the user. Keeps you from stopping — add to file and continue.
- `CONTEXT` — Maintained by the user. The user will update this file when they find something relevant. You do not need to check this file often, the user will notify you when something new is there. You do not need to use the user context if you have more promising stuff going on, but you can keep it in mind. 

### Artifacts

Your working directory. Save whatever you need here — testing PoCs, temporary files, venv, tool-generated files, etc. Not checked by the user.

### Command_outputs

For outputs of longer commands (scanners, etc.) useful for later reference. Short-lived outputs can stay in artifacts.

### POCs

For pretty, well-formatted PoCs **referenced from VULN files only**. If a payload is too long for the VULN file, reference it here. Unreferenced PoCs go into artifacts.
