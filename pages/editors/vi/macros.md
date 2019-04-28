---
layout: page
title: macros
---

[return to editors](../../editors.html)

# Vim Macros

Macros make VIM a powerful editing platform. 

The `q` key acts as a record button, and a stop button for recording macros. To begin recording a macro, we target a register by typing `q[REGISTER]` where REGISTER is a lower or uppercase letter. The word "recording" should appear in the status line. Once recording starts, all actions are saved into the register. To finish the recording, you press `q` again, and the macro is done.

To apply the macro, you simply type `@[REGISTER]`. To apply it multiple times, you type `[NUM]@[REGISTER]`. We can look at the contents of the macro by inspecting the register, which can be done via `:reg [REGISTER]` (as described above). The macro text is not particularly readable. One thing to note is that the `^` key stand for `<ESC>`, not `<CTRL>`. Once we apply the macro, we can repeat the invokation of the last invoked macro via `@@`.

Command | Effect
--- | ---
`qCHAR` | start recording a macro into register `CHAR` (eg `qa`)
`q` | stop macro recording
`@CHAR` | run the macro stored in register `CHAR` (eg `@a`)
`NUM@CHAR` | run the macro stored in register `CHAR` `NUM` times (eg `5@a`)
`"CHARp` | paste the macro in register `CHAR` into the buffer at the cursor. (eg `"ap`)
`"CHARY` | yank text back into a macro stored at register `CHAR` (eg `"aY`)
`@@` | Re-invoke the last invoked macro

# Vim Autocommands
You can automate commands to run based on events. For more information, 
type `:help au`
