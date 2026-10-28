# RTS Quest Voice (RTSQuestVoice) v1.0.0
-  ((A custom curated list of addons has also been added to this attached to branch: 
"WoW-Forever-&-TBC-Anniversary-Addons-(Curated-list))"
By C-scape. When you accept a quest you hear the quest-pickup sound and a Warcraft RTS peon saying **"Ready to work!"**. When you turn a quest in, the peon says **"Work complete!"**.

Works on **TBC Anniversary** realms (2.5.5 / 2.5.6) and **WoW Forever** (1.60.x).

## What it does

| Event | What plays |
|---|---|
| `QUEST_ACCEPTED` | Quest-pickup sound (`SOUNDKIT.IG_QUEST_LIST_OPEN`), then the accept line |
| `QUEST_TURNED_IN` | The complete line (it does not use `QUEST_COMPLETE`, so you don't hear it twice) |

Modes:
- **peon** (default): plays the original peon clips that ship inside the game files:
  - `558137`, `sound/creature/peon/peonready1.ogg` ("Ready to work!")
  - `558132`, `sound/creature/peon/peonbuildingcomplete1.ogg` ("Work complete!")

  If a clip can't play, the addon reads the line aloud with TTS instead.
- **tts**: uses the game's text-to-speech to say "Ready for work" and "Quest complete".
- **both**: plays the peon clip and the TTS line.

## Install

1. Unzip `RTSQuestVoice.zip`. It holds one folder, `RTSQuestVoice/`, with the `.toc` file inside it.
2. Put that folder in the AddOns folder for your client:
   - **TBC Anniversary:** `World of Warcraft\_anniversary_\Interface\AddOns\` (the usual folder name for Anniversary clients; check it in your install).
   - **WoW Forever (beta):** `World of Warcraft\_classic_beta_\Interface\AddOns\`. Several Forever guides from Oct 2026 give this folder. The live client launches on 4 Nov 2026 and may use a different folder. If it does, use the `Interface\AddOns\` folder inside the new Forever folder. Create `AddOns` if it isn't there.
3. The result should be `...\Interface\AddOns\RTSQuestVoice\RTSQuestVoice.toc`, not a folder inside another folder.
4. Restart the game. At character select, open **AddOns** and make sure RTS Quest Voice is ticked.

**If it shows as "Out of date":** tick **Load out of date AddOns** on the AddOns screen. The TOC lists `20506, 20505, 16001`. A patch that changes your client's interface number will mark the addon out of date, but it should still work.

## Settings panel

Type `/qv` to open the settings panel, and type it again or press Escape to close it. You can drag the panel around by its title bar. It has exactly four controls:

- Quest accept voice on/off, with its delay slider (1 to 10 seconds, 0.1 steps, plus -/+ buttons)
- Quest completion voice on/off, with its delay slider (same range)

Changes save right away, and the slash commands below still work.

## Slash commands

| Command | Effect |
|---|---|
| `/qv` | Open the settings panel |
| `/qv on` / `/qv off` / `/qv toggle` | Turn the whole addon on, off, or flip it |
| `/qv mode peon\|tts\|both` | Pick the voice mode |
| `/qv voice` | List the TTS voices on your system |
| `/qv voice N` | Use TTS voice number N |
| `/qv accept on` / `/qv accept off` | Turn the quest accept voice (pickup sound plus accept line) on or off. `/qv accept` alone flips it |
| `/qv complete on` / `/qv complete off` | Turn the quest turn-in voice on or off. `/qv complete` alone flips it |
| `/qv accept N` / `/qv complete N` | Set that voice's delay, 1 to 10 seconds, one decimal place (for example `/qv accept 3.5`). Values outside 1 to 10 are rejected |
| `/qv delay` | Show both delays |
| `/qv test accept` | Play the accept sounds |
| `/qv test complete` | Play the complete sound |
| `/qv help` | Show the commands |

Settings are saved per account in `RTSQuestVoiceDB`.

## Why TTS can't sound like an orc

WoW's text-to-speech (`C_VoiceChat.SpeakText`, voices from `C_VoiceChat.GetTtsVoices`) only uses the voices installed in your operating system, such as Microsoft David or Zira on Windows, or the macOS system voices. No orc voice exists there, and addons can't add new voices. The real peon voice comes from the recorded clips inside the game files, which is why **peon** mode is the default.

## Technical notes

- `C_VoiceChat.SpeakText` changed. Up to 2.5.5 it was `(voiceID, text, destination, rate, volume)`. From 2.5.6, 12.0 and in Forever it is `(voiceID, text, rate, volume[, overlap])`. The addon checks whether `Enum.VoiceTtsDestination` exists to pick the right form.
- Every game API call checks first that the function exists and runs inside `pcall`, so a missing function can't throw an error.

## Changelog

- 1.0.0: First build. Peon voice or TTS on quest accept and turn-in.
- 1.1.0: Separate on/off switches for the accept and turn-in voices (`/qv accept on|off`, `/qv complete on|off`). Separate delays for each, 1 to 10 seconds with one decimal place, both 2.5 seconds by default (`/qv accept N`, `/qv complete N`, `/qv delay`). Out-of-range delays are rejected with a usage message. All commands moved to `/qv`, and `/rqv` was removed. Settings are saved between sessions.
- 1.2.0: `/qv` now opens a settings panel with the accept and completion voice switches and a delay slider for each. The whole addon is now turned on and off with `/qv on`, `/qv off`, or `/qv toggle`. All other slash commands work as before.
- 1.2.1: Both delays now default to 1.8 seconds instead of 2.5 for new installs. Delays you already saved are not changed.
