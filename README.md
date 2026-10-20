# Rev 4 Patch Engine for Prophet-5 and Prophet-10

![Screenshot](screenshot1.png?v=3)
![Screenshot](screenshot2.png?v=3)

A free, browser-based random patch generator, editor and librarian for the Sequential Prophet-5 and Prophet-10 (Rev 4). Generate a patch, nudge it with Mutate, tweak any control, read the patch on the synth into the app, save patches back to your synth, and keep a searchable library.

**Try it:** https://opt-fx.github.io/rev4-patch-engine/

**Unofficial tool. Not affiliated with or endorsed by Sequential. Prophet is a trademark of Sequential, LLC.**

## Just a web page

The whole app is one HTML file: nothing to install, no account, no server, and nothing is uploaded. Your browser talks to your synth directly. To use it offline, download `index.html` from GitHub (open it, then press the Download raw file button) and open it in Chrome or Edge.

Your Library is stored in that browser on that computer, so use **Export library** now and then as a backup.

## What you need

- **Chrome or Edge.** Safari and Firefox don't support Web MIDI, so they can't connect to a synth.
- A MIDI connection from your computer to the synth (USB or a MIDI interface).
- On the synth, in Globals:
  - **Param Rcv** set to **CC**.
  - **MIDI SysEx** set to the route you use (MIDI or USB), with MIDI Out sending on that route. This is needed for Read from synth, Save to synth and capturing.
  - **Param Xmit** set to **CC**, if you want the app to follow knobs and buttons you move on the synth.
- The app's **Channel** menu must match the synth's MIDI channel, and the synth must accept Program Change and Bank Select (needed for capturing and for Load on Library patches).

## Getting started

1. Open the page and click **Allow** when the browser asks for MIDI and SysEx access.
2. Choose your synth model, MIDI output and channel.
3. Press **Test note** to check the connection.
4. Press **Generate** for a random patch, or **Read from synth** to bring in the one that's on the synth.

Every knob and button sends to the synth as you move it. Nothing is saved on the synth until you save it (see Saving below).

## The editor

**Generate and Mutate**
- **Generate** makes a new random patch. Its name is just a random two-word label (such as "Velvet Lagoon"). **New name** changes only the label.
- The **Any sound** dropdown next to it (Pluck, Pad or Drone) shapes the envelopes so the new patch fits that style. Everything else stays random, and locked sections still win. Leave it on Any sound for fully random.
- Generate and Mutate never change **Oscillator A's frequency**, so the patch stays at the pitch you set. Generate sets **Oscillator B** relative to A: most often the same note, otherwise an octave, a fifth, a fourth, a third or a seventh away. A Pluck keeps B at or below a fifth above A. While a unison chord is on, B is limited to the same note or whole octaves, so the chord stays in tune. Mutate occasionally shifts B by an octave, a fifth or a fourth (octaves only with a chord).
- **Mutate** nudges the current patch by the amount you choose. Open **Mutate by section** for a separate slider per section: each follows the main amount until you move it, and 0% leaves that section alone. Click a section's name to make it follow the main amount again.
- The **padlock** on a section (amber when locked) keeps it unchanged through Generate and Mutate. **Keep unison** keeps your unison settings through Generate.
- **Back** undoes up to 20 Generate and Mutate steps, and **Forward** redoes them. Both grey out when there's nothing to go to, and both start afresh when the program changes.

**Following the synth**
- When you choose a program on the synth, the app reads it into the editor and refreshes that slot in the Library.
- With Param Xmit set to CC, knobs and buttons you move on the synth move on screen too.
- When you hold a chord and press Unison on the synth, the app picks up the voice count, chord and detune by itself. If it ever misses one, press **Sync** in the Unison box.
- The **‹** and **›** buttons (or the **[** and **]** keys) step to the previous or next program on the synth and read it into the editor. They need the app to know which program the synth is on, so select one on the synth once, or use Load on a Library entry. They replace the working patch, so use Back if you lose edits you wanted.
- The synth doesn't tell the app when you save with **Record**, so the program number can then be out of date. Once you've changed the sound, the number is marked **· edited** as a warning. Selecting any program refreshes it.

**Saving**
- **Save to synth…** opens a line with Group, Bank and Program dropdowns for a user slot. **Save** writes the patch (with its name and unison chord) to that slot, reads it back to check, and only then adds it to the Library. It overwrites that program for good, so the app shows what's there first. Only user slots are offered: factory programs are protected on the synth.
- If you save on the synth itself with Record, the app can't see it. Press that program button again and the app reads it and refreshes the Library.
- **Export program** and **Import program** save and load a single patch as a small file.
- The **Scratchpad** (the chips under the buttons) is a holding area for sounds you want to keep for now. **Save** puts the editor's patch there without adding it to the Library. Click a chip to load it, **+** to add it to the Library, or **×** to delete it.

**Controls**
- Knobs: drag up or down (hold Shift for fine control), use the mouse wheel or arrow keys when a knob is focused, or double-click to reset.
- The number under each knob is the synth's own value (mostly 0 to 120), not the 0 to 10 on the panel.
- The editor is laid out for a desktop window about 1440 pixels wide. On a wider window it stays centred at that size; on a narrower one the page scrolls sideways. It is not designed for phones or tablets.

## The Library

The **Library** tab is a searchable list of patches stored in your browser. It has three columns: **Factory**, **User**, and **Saved and imported**. Choosing a set in the filter shows just that one.

**Adding patches**
- **Capture patches from the synth** steps the synth through a set and a range of groups (each set has 5 groups of 5 banks of 8 programs), reads each program and adds it. The synth's program changes while it runs. By default it puts the synth back on the program it was on, and sends the editor's patch back. For that, the app needs to know the program the synth is on, so select one on the synth first. Capture stops if the first and last program of the range come back identical, because that means the synth isn't changing program.
- **Save to synth…** and the Scratchpad's **+** also add patches.
- **Import library** merges in a library file (or a single program from Export program) without duplicating what's already there.
- **Delete captured patches in this selection** removes only the captured patches in the chosen set and groups. Saved and imported patches stay.

**Finding patches**
- Search matches names, tags, notes and slot numbers (such as F117 or U558). Sort by synth slot, name or newest.
- Filter by set, group, origin (captured, saved or imported) and tag.
- Tick boxes show only favourites, only patches **changed on the synth**, or only patches you've renamed or annotated.
- **Hide user copies of factory** hides user patches that are still identical to the factory patch in the same position, so you see only the ones you've changed (it needs the factory patches captured too). Copies you've starred, renamed, tagged or annotated stay visible.
- **Clear filters** resets everything.

**Using patches**
- **Load** sends the patch to the synth and switches to the Editor. If the patch has a synth slot, the app first selects that program on the synth, then loads the patch into it. Nothing is written to the slot until you save. A patch with a unison chord is sent as a whole patch, so its chord notes arrive too (this needs MIDI SysEx working; otherwise only the knob and button values are sent, and the app says so).
- Star a patch to make it a favourite. **Details** lets you rename it, add tags and notes, copy it, or delete it (saved and imported patches only).
- **Copy to user slot** (in Details) writes a Library patch, with its name, into any user program on the synth and checks it by reading it back. It's how you copy a factory patch into user memory, or put a patch back after a backup. It starts on the patch's own numbers.
- **Rename on synth** (next to the Name field in Details, for patches in a user slot) writes just a new name to that program. The app reads the program from the synth first and writes it back with only the name changed, so the sound stays as it is on the unit, even if your Library copy is out of date. It's greyed out until the name differs from the one on the synth.

**Keeping track**
- A small **chord** tag marks patches that carry a unison chord. Hover over it to see the notes.
- **Details** also shows a quiet **Detected** line: unison (with voice count), sync, poly-mod, noise, glide, and rough sound types worked out from the envelopes: pluck, pad and drone. They are guesses, are never saved, and you can filter by them from the tag dropdown.
- The program the synth is on is highlighted and marked **On synth**. A patch you loaded into the editor is marked **Loaded**. A red **edited** appears once the sound has changed since. **Find current** clears the filters and scrolls to the marked row.
- If you rename a patch in the Library, recapturing keeps your name until you use Rename on synth.
- If a recapture finds that a slot's patch differs from the stored copy, the entry gets a **changed** badge. It stays until you open the patch's Details and press **Mark as seen**.

## Good to know

- **Glide**, **Velocity** and **Aftertouch** live together in the **Response** panel. Like Rev, Keyboard and the Unison settings, they are stored with each patch; Generate keeps them and Mutate leaves them alone, so those panels have no lock icon. The **Vintage** knob works the same way: Generate and Mutate never touch it, so it has no lock either. The **Sync** button in the Unison box reads the voice count, chord and detune from the synth.
- Rev (filter type), Velocity and Aftertouch routing are stored with each patch. Generate and Mutate leave them alone so they can't override your setup. **Reset** is the exception: it sends them too, returning them to their init values.
- When you choose a program on the synth, the app shows whether it's Factory or User and its number (for example "Factory 113") next to the patch name, as long as the synth is sending Program Change and Bank Select.
- Unison on/off, voice count and detune are handled. A unison chord shows as **Chord** with its intervals above the lowest key (for example "Chord 0·4·9"). You can't edit the notes here: hold the keys on the synth and press Unison. To clear a chord, hold a single key and press Unison; the app follows, or press **Sync**.
- The **Chord** row in the middle of the Unison box makes a unison chord for you. Pick a type from **New chord** (Major, Minor, Suspended, Seventh, Add 9, Power, Stacked fifths or Octaves), or press the amber dice for a random one. The notes are measured from the lowest key you play, so the key you hold is the root. It sends the whole patch to the synth, so read one patch from the synth first. The voice box shows the notes, with the chord's name beside it. Because a chord only sounds right with Oscillator B on the same note as A or a whole number of octaves away, choosing a chord (from the dropdown, the dice, or by setting the voice box to Chord) moves B to the nearest octave of A if it was on another interval, and says so in the status line.
- **Prophet-10:** the app uses the same controller numbers and dump layout as the Prophet-5, because Sequential publishes one MIDI document for both. That hasn't been tested on a Prophet-10. If something reads or sends wrongly, please open an issue.
- Developed and tested on a Prophet-5 Rev 4 (OS 2.1.0) with a Blokas Midihub. Other setups should work but haven't been tried.

## Troubleshooting

- **No MIDI outputs listed:** use Chrome or Edge and allow MIDI access when asked. If you blocked it earlier, click the padlock in the address bar and allow it.
- **Knobs move but the synth doesn't respond:** check Param Rcv is set to CC and that the MIDI channel matches.
- **"No reply from the Prophet":** check MIDI SysEx matches your route, and that nothing in between (such as a Midihub) is filtering SysEx in either direction.
- **Capture stops with "the synth returned the same patch":** the synth isn't changing program. Check the Channel menu, that Program Change receive is on, and that nothing in between filters Program Change or Bank Select.
- **"The library can't be stored":** the browser is blocking site storage (a private window, for example). Use Export to keep your patches.
- **Param Xmit switched itself to Off on the synth:** seen once or twice after changing banks, cause unknown. Set it back to CC to keep the app following your knobs.

## For the curious

- Controller numbers come from Sequential's published Prophet-5 MIDI Implementation (v1.4).
- The tested unit answers patch requests on device ID 0x32, although Sequential's document says 0x31, so the app tries the possible IDs and remembers the one that answers.
- Unison detune isn't in Sequential's documentation. It's at byte 54 of the unpacked edit-buffer dump, found by comparing dumps. Around it: unison on is byte 52, voice count byte 53 (counted from zero, so 0 is one voice and 10 is chord mode), chord note offsets bytes 55 to 64 (`7F` marks an unused slot), and the patch name bytes 65 to 84.

## Running it

It's a single HTML file with no dependencies. Open it in Chrome, or host it anywhere that serves static files, such as GitHub Pages. MIDI access needs a secure (https) page or a local file.

## Licence

Released under the MIT licence (see the LICENSE file). Made by OPT.
