# EditorConfigNodeJs

## Demo project to illustrate problem with EditorConfig not working in MSVS 2017

The new built-in editorconfig support appears to be broken, at least for Node.js projects. 

When I indent code manually using the tab key, the .editorconfig indent_size value is used. However, as soon as I press return at the end of the line, some kind of auto-format feature kicks in, and re-indents my line to some default indent size. 

Similarly, if I manually indent a block of code (select lines and hit tab), the .editorconfig indent_size value is used. However, when I select that block of code and chose Edit -> Advanced -> Format Selection, the code is reformatted to use 4 spaces instead of the .editorconfig indent_size value. I get similar results if I use Format Document and format on paste.

Steps to Repro:
1) Create a new node.js project.
2) Create an .editorconfig file in the project root folder. Use the settings below (note the nonstandard indent_size of 27):

```
root = true
[*.js]
indent_style = space
indent_size = 27
```

3) Close and reopen the project. This seems necessary in order for .editorconfig changes to be recognized.
4) write an empty Javascript function in the default app.js file, e.g.:

```javascript
function foo() {
}
```

5) Place the cursor after the top brace and press enter to create a new line. Note that cursor starts out automatically indented 4 spaces (not 27):

```javascript
function foo() {
    |   // cursor denoted by pipe symbol here
}
```

6) Press the tab key on the same line to indent. Note that the cursor is indented to 27 spaces, as per .editorconfig indent_size:

```javascript
function foo() {
                           |  // cursor denoted by pipe symbol here
}
```

7) Write some code at the cursor position. Characters appear as expected.:

```javascript
function foo() {
                           var x = 0|  // cursor denoted by pipe symbol here
}
```

8) WIth the cursor at the end of the new line of code, press return to start a new line. Note that current line re-indents back to 4 spaces, and cursor starts with indent 4 on new line:

```javascript
function foo() {
    var x = 0  // existing line shifted to indent 4
    |  // cursor denoted by pipe symbol here
}
```

9) Place the cursor in front of the existing line of code and press tab to indent it again. Note that the cursor is indented to 27 spaces, as per .editorconfig indent_size:

```javascript
function foo() {
                           var x = 0
}
```

10) Select the entire function, and choose Edit -> Advanced -> Format Selection. Note that line of code re-indents to 4 spaces.

```javascript
function foo() {
    var x = 0
}
```

11) Repeat steps 9 & 10, but use Edit -> Advanced -> Format Document. Note same result.
12) Repeat steps 9 & 10, but cut the selected function to the clipboard, then paste it (to invoke format on paste). Note same result.

I hope this clearly illustrates the problem and steps to reproduce. 


