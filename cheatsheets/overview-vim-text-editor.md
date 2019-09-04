# New User Tutorial: Overview of the Vim Text Editor

*Reading Time: 2 minutes*

---

Many articles in this knowledge base advise editing configuration files. We usually recommend using your preferred text editor. Personally I prefer nano.

## Normal Mode

The biggest concept that beginning vim users need to know is the concept of modes. Different editor functions, such as moving the cursor around and inserting text, are accomplished in different modes. Open up a file in vim:

```bash
vim example.conf
```

You are immediately put into **normal** mode. Normal modes does not seem normal at first. If you press “j” in normal mode, the letter “j” does not appear. Instead, the letter “j” moves the cursor down one line. Positioning the cursor in this mode is completely done with letter keys. The most basic are:

- **j** – one line down
- **k** – one line up
- **h** – one character left
- **l** – one character right

While this may seem odd at first, switching from mode to mode actually speeds typing up, as you do not have to move your right hand over to the arrow keys and back every time you wish to reposition the cursor.

### Insert Mode

To leave normal mode and start typing text, press “i” to enter **insert mode**. Insert mode behaves much more like the default mode in other text editors. There should not be any surprises.
When you are done with insert mode, hit the **ESC** key to get back to normal mode.

### Command-line Mode

Command-line mode is used to perform a wide range of commands. To enter vim’s command line, hit “:” (the colon) in normal mode. This will drop the cursor to the bottom of the terminal. Here you can do things like:

Save your changes (write):

```bash
:w
```

Quit out of vim:

```bash
:q
```

Quit without saving changes:

```bash
:q!
```

You can also combine commands to run them together. This is commonly done to save the file and quit vim at the same time:

```bash
:wq
```

After you have run a command, vim will place you back in normal mode.

### vimtutor

An article like this can only scratch the surface of using vim. The best way to pick up a new skill like this is to dive in headfirst. Vim comes with a program to help you do just that. **vimtutor** is an instruction manual that runs inside vim. You navigate through it using vim commands, and practice new commands as you learn them. If you really want to learn vim, this is the place to go after reading this article. Simply run ‘vimtutor’ at the command prompt, and you will be off to the races.
