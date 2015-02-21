# Import your Sublime Text 2/3 Environment Fast and Easy #

This guide will help you easily and quickly import your own packages and settings into a fresh Sublime environment


#### The following sections quickly discuss each aspect of customization ####

* Deployment of Environment
* Package Control.sublime-settings
* Preferences.sublime-settings
* Snippets
* Macros
* Key Bindings
* Builds

### Deploy Your Environment ###

Fork my repo! (*or build your own since its just an example*)

Once you have added the snippets, macros, settings, packages, bindings and builds you want up on your repo, the final process can proceed as follows:

  - Locate the `Packages/User` folder either by opening Sublime Text and going to [Preferences] -> [Browse Packages] -> [User]
    **or**
  - Attempt to find it on the command line with
     - Linux/Mac: `find ~/ -type d -name "Packages"`
     - Windows: `dir Packages /AD /s`

Once you have located Sublime's `Packages/User` folder, clone down your repo using the following command:

**SSH**
```
 git clone git@github.com:*your_github_profile_name*/sublime-javascript-snippets.git .
```
**HTTP**
```
 git clone https://github.com/*your_github_profile_name*/sublime-javascript-snippets.git .
```

***Absolutely paramount is the `.` at the end, which allows you to clone it without the encapsulating folder***

After you've cloned down your repo, clean up that messy .git folder:
  - On Linux/Mac: `rm -rf .git`
  - On Windows: (*Not sure, if anyone can help, feel free to email me*)


### Package Control.sublime-settings ###

The crux of getting all your packages downloaded without you lifting a finger.

An example package control settings file might look like this:
```
{
	"in_process_packages":
	[
	],
	"installed_packages":
	[
		"AdvancedNewFile",
		"AngularJS",
		"Autoprefixer",
		"BracketHighlighter",
		"DocBlockr",
		"Emmet",
		"GitGutter",
		"HTML-CSS-JS Prettify",
		"JavaScript & NodeJS Snippets",
		"Markdown Preview",
		"Package Control"
	]
}
```

The magical thing about Package Control is that if it's installed, (which it should be), if Package Control.sublime-settings is present, it will auto-download all your packages once you open Sublime Text *if it does not have it already*.

Don't let "installed_packages" fool you. In this case, it becomes a wishlist for Package Control to fulfill.

### Preferences.sublime-settings ##

A typical preferences file will be quite short and contain only the following

```
{
	"font_size": 14,
	"ignored_packages":
	[
		"Vintage"
	]
}
```

Use this file to add preferences that coincide with your organization's style guide (like wrapping at 80 columns for example)


(If you have used Vim before *or are just crazy*, you can comment Vintage out to use Vim's home row centric multi-mode style in Sublime Text)

### Snippets (files ending in *.sublime-snippet) ###

The magical methods to make text appear out of nothing with `tab`! Many plugins revolve around just giving you a zipped up library of these, so it's incredibly beneficial to know how to construct them.

A typical snippet looks like this:
```
<snippet>
	<content><![CDATA[
${1:functionName}(function(${2:arguments}) {
	${3}
});
]]></content>
	<tabTrigger>eafn</tabTrigger>
	<scope>source.js</scope>
	<description>enclosed anonymous function</description>
</snippet>
```

To break it down:

| Portion				| Explanation|
| ----------------------|-----------|
| `<content></content>` | Contains the actual textual portion of your snippet |
| `<![CDATA *text of shippet here* ]]>` | Required for the python code driving ST to recognize the text correctly |
| `${*1*:*functionName*}`  | This structure places the text "functionName" in the snippet and creates a tab index for it. After inserting the snippet, pressing `tab` once will fully select that element. (twice for "arguments", etc) |
| `<tabTrigger></tabTrigger>` | The text entered here is required to be entered before `tab` will insert the snippet |
| `<scope></scope>` | Typically the type of document (for this example, a javascript file) the snippet can execute in |
| `<description></description>` | The text that will show in the popup menu after you have typed the tabTrigger text |

### Macros (files ending in *.sublime-macro) ###

The keystrokes recorded from a specific series of actions. Where snippets are about creating a large amount of text (or a stub), macros are about reproducing functionality (and typically binding them to keyboard shortcuts).

Macros are exceedingly useful if you would like to create a way to easily reproduce functionality you manually (like putting semicolons at the end of a line and going to the next line).

To create a macro, start recording your actions by going to [Tools] -> [Record Macro] or using `ctrl+alt+q`. After you have finished recording your actions, choose [Tools] -> [Stop Recording Macro] or press `ctrl+alt+q` again. Now you have the option to go to [Tools] -> and [Save Macro]. (no keyboard shortcut).

A simple keyboard shortcut might look like this:
```
[
    {"command": "move_to", "args": {"to": "hardeol"}},
    {"command": "insert", "args": {"characters": "\n"}}
]
```

| Portion				| Explanation|
| ----------------------|-----------|
| `"command"` | Specifies that its value is an understood action by Sublime's backend libraries |
| `"move_to"` | An function involving cursor movement recognized by the backend |
| `"args"`  | Additional qualifiers like "to" or "extend" that move_to uses as a function |
| `"to"` | Specifies a destination for the cursor movement function |
| `"hardeol"` | Sets the destination of cursor movement to the end of the line |
| `"insert"` | Function to place text at the current location |
| `"characters"` | Options argument to specify character code |
| `"\n"` | Characters to put in a new line |

*Please keep in mind that if you are simply recording these macros, you don't need to know any of the syntax. This is only for understanding its composition.*

### Key Bindings/Shortcuts (files ending in .sublime-keymap) ###

Fortunately Sublime does not allocate every conceivable keyboard shortcut out of the box, which gives us a lot of freedom to assign them as we see fit.

First, there are two groups of Key Bindings (found under the [Preferences] tab):

 - *Default* - Built in keyboard shortcuts for your OS version (File: Default(*OS*).sublime-keymap)
 - *User defined* - Keyboard shortcuts default by you (File: Default(*OS*).sublime-keymap)

You might be looking at that going....."What? They're the same...This is not going to end well...."
Fortunately, you're wrong. The lunatics rule the asylum. Black is white. Up is down.

Your custom binding file will be appended onto the existing one. 
(*If you found a way to use custom file names, email me so I can update this, but I haven't found it yet.*)

Another tip: Don't add your custom bindings on to the Default. It works, but it's bad and you should feel bad.

A custom key binding file might look like this:

```
[
  {
    "keys": ["alt+;"],
    "command": "run_macro_file",
    "args": {"file": "Packages/User/semiEnder.sublime-macro"}
  },
  { 
    "keys": ["ctrl+up"], 
    "command": "move", 
    "args": {"by": "stops", "empty_line": true, "forward": false} 
  },
  { 
    "keys": ["ctrl+down"], 
    "command": "move", 
    "args": {"by": "stops", "empty_line": true, "forward": true} 
  },
  {
    "keys": ["ctrl+shift+9"],
    "command": "run_macro_file",
    "args": {"file": "Packages/User/enclosedBraces.sublime-macro"}
  }
]
```

If you read the macros section, some of this should look familiar to you. Keybindings can be formed in two different ways.
 
 - A keybinding will always begin with `"keys": ["*keys*"]` . From there, you can **either**
    - Use a single `"command"` function with the associated `"args"` as discussed in the Macro section **or**
    - Link to an existing Macro file by passing it's `"file"` location as an argument
       (*Note that the filepath is based on the root folder of Sublime Text*)

Also important is to note that you haven't overriden any existing keybindings. A good way to do this is to open the console via `ctrl+\``. This should list any collisions that that system might be experiencing.

### Build Systems (files ending in .sublime-build) ###

Since Sublime Text is, for the most part, a text editor, it does not have any way to inherently run compilers. It simply uses build files that guess based on where a compiler would normally be installed. In some cases this works spectacularly and in some, fails spectacularly.

Here is a breakdown of what a typical build system looks like (I'll use a node build system as that doesn't currently exist)

```
{   
  "cmd": ["/usr/bin/node", "$file"],   
  "selector": "source.js"   
}
```

For the most part, it is exceedingly simple.

| Portion				| Explanation|
| ----------------------|-----------|
| `"cmd"` | Sublime will take the associated array, put it together, and use it at command line (Shell,Powershell,Command Prompt) |
| `["/usr/bin/node",` | The path to the compiler executable (*Linux would use /usr/bin/nodejs*) |
| `"$file]"`  | This will be filled in at command line with the name of the current file you're working in |

That's it. There are a number of other flags and options you can use to customize it, but that's all you need to function.

For a more comprehensive guide on build systems and more examples, head over to

http://addyosmani.com/blog/custom-sublime-text-build-systems-for-popular-tools-and-languages/


### The End ###

Yay!

### Contributing ###

Please note that the packages/preferences/macros/builds/snippets/keybinds I chose were personal preferences of mine and therefore may not be relevant to a large number of peoples. They are used here mostly for example.

Having said that, if you would like to add or correct any part of this guide that you feel needs it, please feel free to email me with your ideas or corrections and I'll be happy to add you as a contributor.
