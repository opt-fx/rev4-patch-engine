# Rev 4 Patch Engine for Prophet-5 and Prophet-10

![Screenshot](screenshot.png)

A free, browser-based random patch generator, editor and librarian for the Sequential Prophet-5 and Prophet-10 (Rev 4). Generate a new patch, nudge the current one with Mutate, tweak any control, read the patch that's on the synth into the app, and keep a searchable library of your patches.

**Try it:** https://opt-fx.github.io/rev4-patch-engine/

**Unofficial tool. Not affiliated with or endorsed by Sequential. Prophet is a trademark of Sequential, LLC.**

## Just a web page, and it works offline

The whole app is one HTML file. There's nothing to install, no account and no server, and nothing is uploaded: your browser talks to your synth directly. You can use it from the link above, or download `index.html` from GitHub (open it, then use the Download raw file button), save it anywhere and open it in Chrome or Edge, with no internet connection needed. The Library is stored in that browser on that computer, so use Export library for backups.

## What you need

- **Chrome or Edge.** Safari and Firefox don't support Web MIDI, so they can't talk to a synth.
- A MIDI route from your computer to the synth (USB or a MIDI interface).
- On the synth, in Globals: **Param Rcv** set to **CC**. For Read from synth, Send with name and the Library's capture, also set **MIDI SysEx** to the route you use (MIDI or USB) and make sure MIDI Out is sending on that route.
- To have the app follow knobs and buttons you move on the synth, also set **Param Xmit** to **CC** in Globals. NRPN isn't supported for this.
- For capturing patches and Go to slot, the synth must accept Program Change and Bank Select, and the app's **Channel** menu must match the synth's MIDI channel.

## Using the editor

1. Open the page and click **Allow** when the browser asks for MIDI (and SysEx) access.
2. Pick your synth model (Prophet-5 or Prophet-10), your MIDI output, and your MIDI channel. The app tries to recognise a Prophet-5 automatically.
3. Press **Test note** to check the connection.
4. **Generate** makes a new random patch. Its name is just a random label made from two words (such as "Velvet Harp"); it says nothing about the sound, and **New name** only changes the label. **Mutate** nudges the current one by the chosen amount. **Back** undoes up to 20 steps.
5. Every knob and button sends to the synth as you move it.
6. **Read from synth** pulls the current patch (including its name and unison settings) into the app. With Param Xmit set to CC, knobs and buttons you move on the synth then move on screen too (untick "Follow the synth's knobs and buttons" to stop that). After changing program on the synth, press Read from synth again to load the new patch. The synth doesn't report a chord being learned over MIDI, so after holding a chord and pressing Unison on the synth, press **Sync** in the Unison box to bring the voice count, chord and detune into the app.
7. **Send with name** sends the whole patch, including its name and unison chord notes, to the synth's working patch and reads it back to check it arrived. To keep it, press **Record** on the synth once, then the program button of the slot you want (pressing Record twice stores nothing). This overwrites whatever is in that slot, so capture it first if you want to keep it. The name is stored with the patch. If the synth doesn't accept the patch dump, the app falls back to sending the controller values, which can't carry the name.
8. The **padlock** on a section's top border (amber when locked) keeps it unchanged when you Generate or Mutate. **Keep unison** keeps your unison settings through Generate.
9. **‹ and ›** (or the **[** and **]** keys) select the previous or next program on the synth and read it into the editor. They step through programs 1 to 8, then banks, then groups, within the current set (200 programs, wrapping round). They need the app to know which program the synth is on: select one on the synth once, or use Go to slot. They replace the working patch, so use Back if you stepped away from edits you wanted. The keys don't work while you're typing in a field or on the Library tab.
10. **Save** and **Load** use small JSON files for a single patch. **Favourites** (the chips under the buttons) are patches you've starred in the Library.

**Mutate by section:** under the buttons, this opens one Mutate slider per section. Each follows the main Mutate amount until you move its slider; 0% leaves that section as it is, and clicking a section's name makes it follow the main amount again. Locked sections stay unchanged whatever their slider says.

**Layout:** on a wide window (about 1100 pixels or more) the editor uses a compact layout with the sections in three rows, and the knobs and spacing grow with the window's height, so a large display shows the whole editor at once. On a MacBook-sized window the bottom row may sit just below the fold. The Library lists patches in two columns, with its filters kept in view as you scroll. Narrower windows, such as phones and tablets, use a single stacked column.

Knobs: drag up or down (hold Shift for fine control), use the mouse wheel or arrow keys while a knob is focused, double-click to reset. The number under each knob is the synth's own value (mostly 0 to 120), not the 0 to 10 printed on the panel.

## The Library

The **Library** tab is a searchable list of patches stored in your browser.

**Adding patches**
- **Save current patch** adds what's in the editor.
- **Capture patches from the synth** steps the synth through a set (Factory or User) and a range of groups (5 groups per set, each 5 banks of 8 programs) using Bank Select and Program Change, reads each one, and adds it to the library. It changes the program on the synth while it runs. By default it then puts the synth back on the program it was on and sends the editor's patch back. The app only knows which program the synth is on if the synth has reported it (select a program on the synth once before capturing); otherwise the synth is left on the last program captured. Before it starts it reads the first and last program of the range and stops if they come back identical, because that means the synth isn't changing program.
- **Import** merges in a library file (or a single patch saved with the Save button) without duplicating what's already there.

The list is split by where each patch lives: **Factory** patches on the left, **User** patches on the right, and anything saved or imported from outside the synth in a **Saved and imported** section underneath (on a narrow window they stack one above the other). Choosing a set in the filter shows just that one.

**Finding patches**
- Search matches names, tags, notes and slot numbers (such as F117 or U558). Sort by synth slot, name or newest.
- Filter by set (Factory, User, or patches not from the synth), group, origin (captured, saved or imported) and tag.
- Tick boxes to show only favourites, only patches **changed on the synth**, or only patches you've renamed or annotated.
- **Hide user copies of factory** hides user patches that are still identical to the factory patch in the same position, so you see only the ones you've changed. Copies you've starred, renamed, tagged or annotated stay visible. It needs the factory patches captured too.
- **Clear filters** resets them all.

**Using patches**
- **Go to slot** (on patches captured from the synth) selects that program on the synth with Bank Select and Program Change, then reads it into the editor. The synth is then on the real program, and nothing in the working patch is changed.
- **Load** sends a patch's sound to the synth and into the editor, so you can audition it and keep editing. It changes only the working patch, not which program is selected on the synth. Chord notes can't be sent this way; use Send with name for those.
- Star a patch to make it a favourite. Open **Details** to rename it, add tags and notes, send it with its name, or delete it.

**Keeping track of changes**
- The row for the program the synth is on is highlighted and marked **On synth**. A patch you've loaded from the library into the editor is marked **Loaded**. A red **edited** appears when the sound has been changed since (the same information as the "· edited" warning next to the patch name, so it has the same limit: the app can't see a save made with Record). **Find current** clears the filters and scrolls to the marked row.
- If you rename a patch in the app, recapturing keeps your name. Details shows the name the synth has, with a button to use it instead.
- If a recapture finds that a slot's patch is different from the stored copy, the entry gets a **changed** badge. The badge stays until you open the patch's **Details** and press **Mark as seen**.

**Backups**
- **Export library** saves everything to one JSON file. The library lives in this browser only, so clearing site data deletes it: export now and then.

## Notes and limits

- Controller numbers come from Sequential's published Prophet-5 MIDI Implementation (v1.4).
- Rev (filter type), Velocity and Aftertouch routing are stored with each patch on the synth. Generate and Mutate leave them alone so they can't override your setup, and they're only sent once you've pressed their button or read from the synth. **Reset** is the exception: it sends them too, returning them to their init values.
- When you select a program on the synth, the app shows whether it is Factory or User, and its number (for example "Factory 113"), next to the patch name, as long as the synth is sending Program Change and Bank Select.
- The synth doesn't tell the app when you save a patch with Record, so after a save the program number can be out of date. Once you've changed the sound since the synth last reported its program, the number is marked **· edited** as a warning. Selecting any program on the synth refreshes it. (This also affects the "put the synth back" step after a capture: if you've saved into a different slot, select that program on the synth before capturing.)
- Unison on/off, voice count and detune are handled. If a patch uses a unison chord, the voice count shows as **Chord**, showing the intervals above the lowest key (for example "Chord 0·4·9") with the chord's name on hover. The chord notes can't be edited here: set them on the synth by holding the keys and pressing Unison. To clear a chord, hold a single key and press Unison on the synth, then press **Sync**.
- The synth doesn't store anything when you use the app's controls. Everything the app sends goes to the working patch until you save it on the synth with Record.
- **Prophet-10:** the app uses the same controller numbers and dump layout as the Prophet-5, because Sequential publishes one MIDI document for both. That hasn't been verified on a Prophet-10. If something reads or sends wrongly, please open an issue.
- Safari and Firefox users can still look at the page, but can't connect to a synth.
- Developed and tested on a Prophet-5 Rev 4 (OS 2.1.0) with a Blokas Midihub. Other setups should work but haven't been tried.
- The synth answers patch requests on device ID 0x32 on the tested unit, though Sequential's document says 0x31, so the app tries the possible IDs and remembers the one that answers.
- Unison detune isn't listed in Sequential's documentation. It sits at byte 54 of the unpacked edit-buffer SysEx dump (unison on is byte 52, voice count byte 53, counted from zero so 0 means one voice, and 10 means chord mode, with the chord's note offsets stored in bytes 55 to 64 and `7F` marking unused slots, and the patch name in bytes 65 to 84), found by comparing dumps.

## Troubleshooting

- **No MIDI outputs listed:** use Chrome or Edge and allow MIDI access when asked. If you blocked it earlier, click the padlock in the address bar and allow it.
- **Knobs move but the synth doesn't respond:** check Param Rcv is set to CC in Globals and that the MIDI channel matches.
- **"No reply from the Prophet" on Read from synth:** check MIDI SysEx in Globals matches your route, and that nothing in between (a MIDI router such as a Midihub) is filtering SysEx in either direction.
- **Capture stops with "the synth returned the same patch":** the synth isn't changing program. Check that the Channel menu matches the synth's MIDI channel, that Program Change receive is on, and that nothing in between filters Program Change or Bank Select.
- **Send with name says the synth did not take the patch dump:** the name couldn't be written. The controller values were sent instead. Check MIDI SysEx in Globals.
- **A message says the library can't be stored:** the browser is blocking site storage (a private window, for example). Use Export to keep your patches.
- **Param Xmit switched itself to Off on the synth:** this was seen once or twice after changing banks and the cause isn't known. Set it back to CC to keep the app following your knobs.

## Running it

It's a single HTML file with no dependencies. Open it in Chrome, or host it anywhere that serves static files, such as GitHub Pages. MIDI access needs a secure (https) page or a local file.

## Licence

Released under the MIT licence (see the LICENSE file). Made by OPT.
