Bidirectional Rich Text Sync Across Iframes

## Overview

This project demonstrates real-time bidirectional synchronization between two rich text editors running inside separate iframes.

Each iframe contains an independent `contenteditable` editor with formatting controls for **Bold**, **Italic**, and **Strikethrough**. Since iframes are isolated browsing contexts and cannot directly access each other’s DOM, communication is handled through `window.postMessage`, with the host page acting as a message broker.

Whenever a user edits or formats content in one frame, the host captures the message and relays the change to the other frame, ensuring both editors remain synchronized in real time.

---

## How It Works

Once the application is opened, you’ll see:

* **Frame A** on the left
* **Frame B** on the right
* A bridge panel in the center showing sync activity

### Basic Usage

1. Select text in either editor and click **B / I / S** to apply formatting.
   The formatting is instantly reflected in the other editor.

2. Type normally inside either editor.
   Text changes are synchronized automatically after a short debounce (300 ms).

3. Press **Ctrl + Z** to undo changes.
   Undo actions are synced across both frames.

4. Keyboard shortcuts supported:

   * `Ctrl + B` → Bold
   * `Ctrl + I` → Italic
   * `Ctrl + Shift + X` → Strikethrough

5. Visual sync indicators:

   * A **green dot** flashes on the receiving frame
   * Bridge arrows highlight the sync direction
   * Sync latency is displayed in milliseconds

6. Additional controls:

   * **Pause Sync** → Temporarily stop relaying updates
   * **Export Log** → Download complete event logs as `.txt`

---

## Features Implemented

### Core Requirements

✅ Two iframes with independent rich text editors
✅ Bold / Italic / Strikethrough formatting toolbar
✅ `postMessage` communication between iframe and host
✅ Host acts as message broker between frames
✅ Full bidirectional synchronization
✅ Infinite-loop prevention using receive guards

---

### Nice-to-Have Features

✅ Origin validation for secure message handling
✅ Active toolbar state based on cursor selection
✅ Visual sync indicators

---

### Bonus Features

✅ Real-time text synchronization
✅ Cursor position preservation after sync
✅ Undo / Redo synchronization
✅ Live action/event logging

---

## Extra Enhancements

I also added several usability improvements beyond the assignment requirements:

* Keyboard shortcut support for faster editing
* Live word and character counters
* Sync pause/resume mode
* Downloadable log export
* Message counter badge in header
* Sync latency measurement
* Animated bridge arrows showing direction
* Editor flash animation on incoming sync
* Color-coded logs by event type
* Log auto-trimming (capped at 200 entries)
* Undo / Redo toolbar buttons

These additions help visualize system behavior and improve debugging.

---

## Technical Architecture

```text
Frame A → postMessage → Host → relay → Frame B
Frame B → postMessage → Host → relay → Frame A
```

### Message Format

```javascript
{
  type: "FORMAT_SYNC",
  action: "bold" | "italic" | "strikeThrough",
  html: "<p>The <strong>quick</strong> brown fox...</p>"
}
```

---

## Design Decisions

### Host as Single Source of Communication

The iframes never communicate directly.
All messages pass through the host page, making the system easier to debug and extend.

### Preventing Infinite Loops

Incoming sync updates should not trigger outgoing sync messages again.
A `receiving` flag is used to prevent rebroadcast loops.

### Debounced Text Sync

Typing generates frequent updates, so text synchronization is debounced by **300 ms** to reduce message flooding and improve performance.

### Cursor Preservation

Replacing editor HTML normally resets caret position.
To avoid disrupting user experience, cursor location is preserved using character-offset traversal.

---

## Tech Stack

* HTML
* CSS
* JavaScript
* `window.postMessage`
* `contenteditable`

---

## Notes

This project was built with a strong focus on:

* real-time synchronization
* browser messaging architecture
* user experience
* performance optimization

Beyond meeting the base requirements, I aimed to explore edge cases such as cursor handling, undo synchronization, and message throttling to build a more production-like implementation.
