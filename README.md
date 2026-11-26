# MQ2ExtendedSocials

**v0.4.0 Stable — AAs, disciplines, spells and item clickies — Windows x86 (Win32)**

Extend EverQuest social buttons beyond the traditional five-line limit by assigning owned alternate advancement abilities (AAs), disciplines, memorized spells and bard songs, and clickable inventory or equipped items to a named sequence in MacroQuest.

## Distribution status

This is the public distribution and issue-reporting repository. The v0.4.0 Stable release is published. Download its verified binary, corresponding source package and checksums from [GitHub Releases](https://github.com/ouijacat/MQ2ExtendedSocials-Releases/releases/latest).

Copyright (C) 2026 ouijacat. MQ2ExtendedSocials is licensed under GPL-2.0-only and provided for educational purposes, without additional restrictions on GPL rights. MacroQuest is licensed under GPLv2; see its [pinned upstream license](https://github.com/macroquest/macroquest/blob/bc1930741e03370d806e623ec60070735df26d76/LICENSE.md).

## Compatibility and download

Designed for the tested custom RoF2 EQEmu client and compatible MacroQuest build. Other clients, servers and MacroQuest versions are not verified. Use only where permitted by server rules.

Download the ZIP from the [latest GitHub Release](https://github.com/ouijacat/MQ2ExtendedSocials-Releases/releases/latest). The planned release includes the original tested DLL, documentation, licensing notices, a source archive and SHA-256 checksums. Clean source buildability and complete corresponding-source coverage have not been independently established. No compilation is needed.

## Installation and upgrades

1. Download MQ2ExtendedSocials-v0.4.0-Win32.zip and extract it.
2. Completely exit EverQuest and MacroQuest.
3. Back up any existing MQ2ExtendedSocials.dll and character configuration files.
4. Copy the extracted MQ2ExtendedSocials.dll into the directory containing your other MacroQuest plugin DLLs.
5. Start MacroQuest and EverQuest and log into a character.
6. Enter `/plugin MQ2ExtendedSocials` in game.
7. Enter `/esocial` to open the Extended Socials window.

If the plugin fails to load, do not force it into an incompatible client. Check architecture and MacroQuest version.

## Quick start: create your first hotbutton

1. In the Extended Socials window, enter TestBurn in **New name** and click **Create**.
2. Select TestBurn in the **Social** dropdown.
3. Click **Refresh abilities** to populate the available owned AAs, displayed disciplines, memorized spells and clickable items.
4. Use **Search** to filter by name or ability type prefix. Use All/AA/Disc/Spell/Item filters and click **Add** or double-click an ability. Duplicate additions are prevented by default; enable Allow duplicate abilities per social to permit them.
5. In the assigned panel, drag rows to reorder or use ^, v and X to move or remove entries.
6. Select **Execution mode** (Mode A or Mode B). Expand **Advanced execution settings** and set **Delay (ms)**. Default is 250 ms; range is 100–5000 ms. Try 500 ms initially if abilities need extra time.
7. Click **Save**. Look for the Saved chat message.
8. Click **Run** to test in a suitable in-game situation.
9. Click **Copy hotbutton command** and paste the command into one line of a normal EverQuest social:

       /esocial run TestBurn

10. Drag the native social to your hotbar. Press it to execute the assigned abilities.

Names are case-sensitive and must be 1–63 characters using letters, numbers, underscores or hyphens. Do not include spaces. Create additional named socials for different purposes. Select an existing social to edit it; click Save after every change, including deletion.

## How Mode A works

- Abilities are evaluated in assigned order. The sequence starts from the beginning on every accepted activation.
- Unavailable or unowned abilities are skipped, including abilities on cooldown. They are not retried automatically; a later explicitly assigned duplicate is evaluated independently.
- The configured delay follows each dispatched command; skipped entries have no configured delay.
- The plugin waits while the client reports casting. A 60-second per-step timeout cancels a stalled sequence.
- Only one sequence runs at a time; additional run requests while active are rejected.
- Zoning, detected death, leaving the game, character changes and unloading cancel active execution. A new button press after recovery starts from the beginning and skips unavailable abilities.
- Dispatch does not guarantee server-side activation: target, range, resources, shared timers and server restrictions may still prevent an ability from firing.
- Editing a social does not alter an already queued sequence.

## How Mode B works

Each accepted press starts at the top of the assigned priority list. Unavailable AAs are skipped. Discipline commands are checked for cast-start confirmation using chat, casting spell ID or the active discipline label. A confirmed discipline ends the sequence; an unconfirmed attempt times out and the scan continues.

Mode B settings (per social; click Save):

| Setting | Default | Range |
|---|---|---|
| Protect active disciplines | On | On/off |
| Disc confirmation (ms) | 1000 | 250–3000 |
| Disc retry (sec) | 5 | 0–30 |

Protection skips disciplines while another is active; it never cancels an effect. Effective confirmation time is the greater of social delay and confirmation setting. Retry suppression is shared across socials for the current character, in memory; zero bypasses suppression. The discipline protection and retry settings apply to Mode B only.

RoF2 exposes only 20 native combat timer slots; extended IDs such as 161 and 302 return zero. Mode B works around this with activation confirmation, not an authoritative cooldown lookup. Rejected attempts can still display cooldown messages. Late or missing cast confirmation can allow additional activations; cast start is not proof of completion. Shorter timeouts increase this risk.

## Spells, songs and item clickies (v0.4)

Refresh abilities after changing memorized spells or inventory. Spell assignments store spell IDs and resolve the current gem at execution; moving a spell to another gem does not require reassignment. Unmemorized spells are skipped. Individual gem and global recovery checks are used. Mana, target and other server restrictions can still reject a cast. Bard songs are supported in the tested environment.

Item assignments store item IDs and resolve current inventory locations, including equipped items and items inside bags. Cursor-held items are excluded. The catalog shows one entry per item ID; available copies are preferred. Native slot-based /useitem activation is used. The plugin checks native reuse, equipment requirements and limited charges; it does not equip or move items. Shared reuse restrictions depend on client reporting. Refresh after inventory changes.

Spell confirmation uses cast-start chat or casting state. Item confirmation additionally checks reuse or charge changes at the selected inventory location. Instant items without observable changes or consumed items that disappear may be unconfirmed. In Mode B an unconfirmed item attempt stops the sequence conservatively rather than risking another activation. An unconfirmed spell attempt may continue the scan. The effective confirmation window uses the existing discipline confirmation setting and social delay. Confirmation is not proof of a successful effect; delayed or missing signals remain a limitation.

## Optional duplicate assignments

Enable **Allow duplicate abilities** under Advanced execution settings and Save. This is a per-social setting, off by default. All four ability types may be assigned repeatedly. Each occurrence can be moved or removed independently and receives its own execution result. Disabling the option prevents new duplicates but preserves existing entries.

Mode A evaluates each occurrence in order, allowing a repeated spell, AA, discipline or clicky to activate again if ready when reached. It skips unavailable occurrences and does not wait for their cooldown to expire. Mode B still starts at the top on each press and stops after an accepted activation (or conservative unconfirmed item attempt); duplicates do not create a rotating priority cursor. There is no automatic looping, targeting, memorization or equipment swapping.

## Configuration window reference

| Control | Purpose |
|---|---|
| New name / Create | Create or select a named social |
| Social | Choose a social to edit |
| Execution mode | Ordered or priority execution |
| Protect active disciplines / Disc confirmation / Disc retry | Mode B settings described above |
| Allow duplicate abilities | Permit repeated entries in this social |
| Delay (ms) | Set spacing between dispatched commands |
| Refresh abilities | Rebuild the ability catalog |
| Search | Filter available abilities, case-insensitively |
| Add | Append an ability |
| Up / Down / Remove | Reorder or remove assigned entries |
| Copy hotbutton command | Copy the exact native social command |
| Run | Start the selected sequence |
| Delete social | Confirm removal; Save to persist |
| Save | Persist all current character socials |
| Stop | Cancel active sequence |
| Debug logging | Toggle per-entry dispatch/skip messages |
| Running | Show actual evaluated count and progress during execution |

## GUI and social management (v0.3)

Resize the window or drag the visible divider between available and assigned panels. Filter the catalog with All, AA, Disc, Spell or Item; search is case-insensitive. Hover ability names for IDs and base recast information (actual reuse may differ). The assigned count is shown in the panel heading. Clear assigned list requires confirmation. Advanced execution settings collapse to conserve space.

The social dropdown is name-sorted and case-sensitive. Rename preserves settings and abilities but requires updating native hotbuttons referencing the old name. Clone makes an independent copy. Both reject invalid or existing names and support cancellation. Delete requires confirmation. All edits remain in memory until Save; the unsaved changes indicator compares current configuration to the last saved state.

## Readiness and execution feedback

Assigned rows show Ready, Not ready, Active, Protected, Retry suppressed, Cooldown, Unknown or Unavailable, plus spell and item states such as Not memorized, Casting, Must be equipped, No charges and Item not available. Ready means the client check reports availability, not guaranteed server success. Unknown includes unsupported native discipline timer IDs. Protection and retry indicators reflect the selected social's Mode B settings.

Expand Last execution results to inspect the most recent sequence, mode, termination status, evaluated count and each ability's outcome. Results distinguish skipped entries (with reason), command dispatched, awaiting confirmation, confirmed cast start, active effect or item change, no confirmation, interrupted confirmation and not evaluated. Mode B can finish before the entire list is evaluated. Live progress counts actual evaluated entries. Results are replaced on the next accepted run and cleared on character change/logout; no disk history is kept.

Expand Chat notifications to toggle sequence start and completion (default on), individual results and debug logging (default off). Errors and interruption warnings remain visible. These preferences are session-only and reset on plugin reload. Debug output may include detailed diagnostics.

## Command reference

| Command | Action |
|---|---|
| `/esocial` | Open configuration |
| `/esocial run NAME` | Start named social |
| `/esocial stop` | Cancel current sequence |
| `/esocial list` | List names and ability counts |
| `/esocial reload` | Reload saved data only if no unsaved edits exist |
| `/esocial reload confirm` | Explicitly discard unsaved edits and reload |
| `/esocial diag NAME` | Report discipline timer and callback diagnostics |
| `/esocial debug on` | Enable per-entry debug messages |
| `/esocial debug off` | Disable debug messages |
| `/esocial help` | Show command syntax |

The alias `/extendedsocial` accepts the same arguments.

## Saving, backups and character switching

Configurations are separated by server short name and character name. The filename is formed from MacroQuest's plugin INI filename plus `.<server>_<character>.socials`. The exact directory depends on the MacroQuest installation. Search for *.socials to locate the files. Back them up with both applications closed. Save explicitly before logging out or switching characters; changes are not automatically saved. Reload refuses unsaved changes unless explicitly confirmed. Logout, character changes and unload warn but do not block loss of unsaved edits. Do not edit configuration files while the plugin is active.

### Automatic backups and restoration

Before replacing an existing saved configuration, Save rotates five backups: .socials.bak1 (newest) through .socials.bak5 (oldest). First save of a new character configuration has no prior file to back up. Backups are per server/character. If backup creation fails, the primary save is cancelled. Rotation itself is not transactional. To restore, exit EQ and MacroQuest, copy the current file somewhere safe, then copy the chosen backup over the matching .socials file (retain the backup). Restart and verify. Do not restore another character's file accidentally.

## Troubleshooting

| Symptom | What to check |
|---|---|
| Plugin fails to load | Correct Win32 DLL, plugin directory, compatible MacroQuest/RoF2 build |
| /esocial unknown | Confirm plugin loaded; check MacroQuest chat for errors |
| Window says enter game | Log into a living character and finish zoning |
| Missing abilities | Refresh abilities; check ownership, discipline visibility, memorization and inventory |
| Unknown or empty social | Check exact name, assigned entries and current character |
| Ability skipped | Check cooldown, ownership and availability; enable debug |
| Command dispatched but no activation | Check target, range, resources, shared timers and server restrictions |
| Sequence already running | Wait for completion or use /esocial stop |
| Sequence timeout | Check casting state; retry after recovery |
| Changes disappear | Click Save and check for Saved or write errors |
| Hotbutton stops after zoning/death | Expected behavior; press again when playable |

Debug messages are prefixed [ExtendedSocials]. A dispatched message confirms command submission, not server-side success. When reporting a problem, include client/MacroQuest version, social order, delay, reproduction steps and relevant debug output. Remove private character/account details as needed.

## Validation and limitations

User-reported v0.4 testing passed all 140 checks: Alpha 1 spell support (42), Alpha 2 items and duplicates (50), Alpha 3 final validation (40), and stable installation checks (8). Final validation used the same Alpha 2 DLL without recompiling and included 20-entry mixed rotations, shared cooldowns, duplicate chain casting, multiple item copies, final charges, death/zoning/logout, configuration restoration and at least one hour of gameplay. One Alpha 2 compilation failure caused by an upstream inventory traversal overload was corrected before testing. No runtime defects were reported.

The validated stable package preserves the tested Alpha 2 DLL byte-for-byte; its startup message intentionally identifies Alpha 2. Existing v0.3.0 and earlier configurations are supported. Back up before upgrading or downgrading: older versions cannot safely read socials containing spell or item types and may discard newer settings when saving. Notification preferences and execution results are not persisted. Activation confirmation is not proof of server-side completion. Other servers and client versions are not verified.

