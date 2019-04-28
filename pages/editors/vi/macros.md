[return to editors](../../editors.md)

# Vim Macros

Command | Effect
--- | ---
`qCHAR` | start recording a macro into register `CHAR` (eg `qa`)
`q` | stop macro recording
`@CHAR` | run the macro stored in register `CHAR` (eg `@a`)
`NUM@CHAR` | run the macro stored in register `CHAR` `NUM` times (eg `5@a`)
`"CHARp` | paste the macro in register `CHAR` into the buffer at the cursor. (eg `"ap`)
`"CHARY` | yank text back into a macro stored at register `CHAR` (eg `"aY`)
