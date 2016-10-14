---
layout: post
title: KiCad Tutorial
permalink: /wickerlib/tutorial-draft/
---

KiCad is a free, open source PCB design tool that has no board area limits and no commercial restrictions.

This article is almost a tutorial in that it's written to be a reference for someone (me) working through a small, complete design from start to finish. It introduces each set of concepts in a spiral pattern instead of a linear one. For example, it talks about project organization right up front but waits to tackle library organization until it's time to talk about adding or editing parts. 

<hr> 
##### Attitude

No CAD tool can magically read your mind and create the perfect schematic and layout, so I recommend approaching KiCad with a curious, playful, patient attitude and give it a couple of weeks and several tiny projects to get the hang of it. 

It's different from Eagle, gEDA, or DesignSpark, and I've started to really like using it. When in doubt, Google <em>how to do this nonobvious but better thing in KiCad</em> and you'll see that, in most cases, somebody has solved your problem. 

<hr> 

##### Installing KiCad 4

I'm using the KiCad new stable, version 4.0.1, which has all the new 2015 features.

<a href="http://kicad-pcb.org/download/">KiCad download information is here</a> for Mac, Windows, and Linux. 

I'm running Linux Mint 17.3, Rosa, which is based on Ubuntu 14.04 LTS. 

<hr>

##### My installation

Installing KiCad put the binary in `/usr/bin/kicad`. 

As part of the default install, KiCad automatically created `template` and `scripting` folders under `/usr/share/kicad/`, but it didn't immediately download and link local copies of all the symbols and footprints.

This is partly because the maintainers of the official KiCad libraries have set them up as Github repositories. The symbols have to be downloaded locally, at least for now, but the footprints can be added directly from Github while you're in the middle of designing the schematic or layout. 

Since I prefer to work offline, I had to figure out how to download and maintain lots of libraries on my local computer. To do that, first I had to figure out what KiCad thinks a library actually is. 

<hr>

##### What is a Parts Library?

**Components**

For components (symbols), KiCad considers a 'library' file to be the `libraryname.lib` file, and it also expects a `libraryname.dcm` file to be in the same folder. KiCad doesn't care if lots of different component libraries are all collected in the same folder. 

Each `libraryname.lib` file contains lots of symbols. For example, the official list of symbol libraries <a href="https://github.com/KiCad/kicad-library/tree/master/library">here</a> contains the official `atmel.lib` library file along with its corresponding `atmel.dcm` file in the same folder. When you include and open this `.lib` file in the Schematic Parts editor, it shows you a list of all the included parts so you can choose the one that best fits your schematic.

**Modules**

For modules (footprints), KiCad considers a 'library' file to be a folder that ends in `.pretty` that contains one or many module files that end in `.kicad_mod`. 

Unlike the component symbols, which are written text hidden inside one `.lib` text file, the new KiCad solution for footprints is to create a new and separate `.kicad_mod` file for each footprint. This way, editing or removing one doesn't affect any of the other files. 

The .pretty libraries are actually full git repositories themselves under the <a href="https://github.com/KiCad">KiCad</a> project on Github, so the repository for the `Capacitors_SMD.pretty` is <a href="https://github.com/KiCad/Capacitors_SMD.pretty">here</a>. 

The entire .pretty folder is itself the library that contains module files called `C_0402.kicad_mod`, `C_0603.kicad_mod`, `C_0603_HandSoldering`, and so on. The `.kicad_mod` module files are the different packages that you can choose when you associate a footprint with a symbol.

<hr>

##### What is a Parts Library?

**Components**

For components (symbols), KiCad considers a 'library' file to be the `libraryname.lib` file, and it also expects a `libraryname.dcm` file to be in the same folder. KiCad doesn't care if lots of different component libraries are all collected in the same folder. 

Each `libraryname.lib` file contains lots of symbols. For example, the official list of symbol libraries <a href="https://github.com/KiCad/kicad-library/tree/master/library">here</a> contains the official `atmel.lib` library file along with its corresponding `atmel.dcm` file in the same folder. When you include and open this `.lib` file in the Schematic Parts editor, it shows you a list of all the included parts so you can choose the one that best fits your schematic.

**Modules**

For modules (footprints), KiCad considers a 'library' file to be a folder that ends in `.pretty` that contains one or many module files that end in `.kicad_mod`. 

Unlike the component symbols, which are written text hidden inside one `.lib` text file, the new KiCad solution for footprints is to create a new and separate `.kicad_mod` file for each footprint. This way, editing or removing one doesn't affect any of the other files. 

The .pretty libraries are actually full git repositories themselves under the <a href="https://github.com/KiCad">KiCad</a> project on Github, so the repository for the `Capacitors_SMD.pretty` is <a href="https://github.com/KiCad/Capacitors_SMD.pretty">here</a>. 

The entire .pretty folder is itself the library that contains module files called `C_0402.kicad_mod`, `C_0603.kicad_mod`, `C_0603_HandSoldering`, and so on. The `.kicad_mod` module files are the different packages that you can choose when you associate a footprint with a symbol.

<hr>

##### Setting up my Library Folders

Once I knew what KiCad wanted a library to look like, I had to figure out how the libraries are arranged on my computer. I have basically three sources of KiCad library files: 

1. The official KiCad libraries
2. My wickerlib libraries
3. Other libraries from random people

Every folder in this list contains one of those three possible sources: 

{% highlight bash %}
# official libraries
~/proj/kicad/library-repos/symbols/     
~/proj/kicad/library-repos/footprints   
~/proj/kicad/library-repos/templates   

# wicker libraries
~/proj/kicad/wickerlib-kicad/symbols/   
~/proj/kicad/wickerlib-kicad/footprints
~/proj/kicad/wickerlib-kicad/templates  

# other random libraries
~/proj/kicad/example_board_lib1/       
~/proj/kicad/example_board_lib2/      
~/proj/kicad/and-so-on-lib/          
{% endhighlight %}

**Official KiCad Libraries**

Other people leave the KiCad repositories on Github but I like to work offline so I created a folder called `~/proj/kicad/library-repos/` and followed the instructions from the <a href="https://github.com/KiCad/kicad-source-mirror/blob/master/scripts/library-repos-install.sh">libgrary-repos-install script here</a> to copy all of the KiCad footprint libraries to a directory called `~/kicad_sources`. 

Then I moved all the `.pretty` folders in that directory to `~/proj/kicad/library-repos/footprints/`.

To get the symbols, I cloned the <a href="https://github.com/KiCad/kicad-library">kicad-library</a> github repo. I moved and renamed the `library` folder to `~/proj/kicad/library-repos/symbols` and the `template` folder to `~/proj/kicad/library-repos/templates`. 

**My wickerlib Libraries**

Because I'm leaving the official KiCad libraries as read-only references, I'll either use them as-is or I'll copy them into my own `wickerlib` set of libraries before editing. That way, there's no confusion about what's proven and what's not.

The `~/proj/kicad/wickerlib-kicad` folder contains a public, forkable git repository called <a href="https://github.com/wicker/wickerlib-kicad">wickerlib-kicad</a> that's where I keep my component (symbols), module (packages/footprints), and project templates. 

This git repository looks something like this:

{% highlight bash %}
footprints/esp8266.pretty/esp01.kicad_mod
footprints/esp8266.pretty/esp12e.kicad_mod
footprints/rf24.pretty/nRF24.kicad_mod
symbols/rf24.dcm
symbols/rf24.lib
symbols/wicker-crystal.dcm
symbols/wicker-crystal.lib
{% endhighlight %}

**Other project libraries**

As I download other projects or collaborate with other folks, I'll keep the libraries for these projects more or less in the same root `~/proj/kicad` folders.

<hr>

The only trick with setting up my folders this way is how to tell KiCad where to find them.

For the symbols, I edited the `LibDir` variable under `[eeschema]` in each  KiCad Project file. 

Since I use a template to create new projects, I opened the .pro file of my template project in a text editor and set all the library-related variables to match my folders.

{% highlight bash %}
[eeschema]
version=1
LibDir=/home/wicker/proj/kicad/
[eeschema/libraries]
LibName1=kicad-library/power
LibName2=kicad-library/device
LibName3=kicad-library/transistors
LibName4=kicad-library/conn
LibName5=kicad-library/linear
LibName6=kicad-library/regul
LibName7=kicad-library/74xx
LibName8=kicad-library/cmos4000
LibName9=kicad-library/adc-dac
LibName10=kicad-library/memory
LibName11=kicad-library/xilinx
LibName12=kicad-library/microcontrollers
LibName13=kicad-library/dsp
LibName14=kicad-library/microchip
LibName15=kicad-library/analog_switches
LibName16=kicad-library/motorola
LibName17=kicad-library/texas
LibName18=kicad-library/intel
LibName19=kicad-library/audio
LibName20=kicad-library/interface
LibName21=kicad-library/digital-audio
LibName22=kicad-library/philips
LibName23=kicad-library/display
LibName24=kicad-library/cypress
LibName25=kicad-library/siliconi
LibName26=kicad-library/opto
LibName27=kicad-library/atmel
LibName28=kicad-library/contrib
LibName29=kicad-library/valves
LibName30=wickerlib-kicad/symbols/wicker-dds
LibName31=wickerlib-kicad/symbols/wicker-crystal
LibName32=wickerlib-kicad/symbols/wicker-test
{% endhighlight %}

For the footprints, KiCad uses the list of libraries in `~/.config/kicad/fp-lib-table` along with the system variable `KISYSMOD`, so I added a line in my `~/.bashrc` to set that system variable:

{% highlight bash %}
export KISYSMOD="/home/wicker/kicad_sources/library-repos/"
{% endhighlight %}

First, I cloned my own <a href="http://github.com/wicker/wickerlib-kicad">wicker/wickerlib-kicad</a> git repo. 

For the official libraries, I had to get the files from Github before I could move them into my 'official' `kicad-library` folders.

To get the symbols and templates, I cloned <a href="https://github.com/KiCad/kicad-library">KiCad/kicad-library</a>, and copied the `library` folder into `symbols` and the `template` folder into `templates`. In the future, I could pull updates and rsync them over if I wanted, but for now I'm not worried about it. 

To get the footprints, I copied the .pretty libs into `kicad-library/footprints` by following the instructions in the <a href="https://github.com/KiCad/kicad-source-mirror/blob/master/scripts/library-repos-install.sh">library-repos-install.sh</a> script. 

I ran `./library-repos-install.sh --install-or-update` and ended up with all of the .pretty repos in a folder in `/home/wicker/kicad_sources/library-repos/kicad-library` so I was able to copy just what I needed into `~/proj/kicad/kicad-library/footprints`. 

Finally, to link everything up, I had to edit the fp-lib-table file.

KiCad uses the Library Tables file in each project as part of the PCB Footprint Wizard so it can find the .pretty module (footprint) libraries. I always use the Wizard when I'm starting a new layout in PCBNew, so I copied the Library Table file from `~/proj/kicad/library-repos` `template/fp-lib-table.for-pretty` into KiCad's config file at `~/.config/kicad/fp-lib-table`. 

KiCad needs `KISYSMOD` to point to the library repos. In a standard installation, it points at `/usr/local/modules`. 

Finally, the last step was to edit `/etc/profile.d/kicad.sh` to set `KISYSMOD` to `~/proj/kicad/library-repos`

Now KiCad wil look for `*.pretty` footprint files in `~/proj/kicad/library-repos`. The burden of maintaining and checking for library updates falls on me, but that's fine. 

<hr>

##### Working Folders

Now that KiCad is installed and the libraries are available, the next step is figuring out how to organize each project folder. First, here's how I typically organize the working directories on my Linux computer:

{% highlight bash %}
~/proj/adafruit/random-adafruit-library
~/proj/desertlizard/lizardware
~/proj/eagle/wickerlib-eagle
~/proj/kicad
~/proj/kicad/library-repos
~/proj/kicad/wickerlib-kicad
~/proj/wicker/project1
~/proj/wicker/project2
~/proj/wicker/wicker.github.io
~/scratch
~/tools/arduino-1.6.5
~/tools/eagle-6.6.0
{% endhighlight %}

All of my current hardware and software projects are captured in git repositories in my ~/proj directory. I'll often clone github repos from places like Sparkfun or Adafruit into their own subfolders so I can keep track of where I got stuff.

The scratch folder can be deleted at any time, and nothing seriously important stays in there. It's a temporary working directory. When it gets too full, it gets deleted.


I install my tools in `~/tools` when they're not in `/usr/local/bin`. Certainly, I keep the source of the tools in `~/tools` for future reference.  

The `wickerlib-eagle` and `wickerlib-kicad` folders are my own repositories of footprints for Eagle and KiCad. I'm excited by the KiCad .pretty libs being on Github because it means I can start contributing proven footprints back to the main libraries!

<hr>

##### Anatomy of a KiCad Github Project Repo

Let's say `~/proj/wicker/project1` is where I want to create a KiCad project, and I want to upload it to Github for version control and safekeeping. This is how I generally organize the files in my KiCad project repositories: 

{% highlight bash %}
.gitignore  # don't track kicad cruft 
README      # overall project documentation!
LICENSE     # usually the CERN OHL v1.2
.pro        # KiCad project file
.sch        # EESchema schematic
-cache.lib  # local cached copy of schematic symbols
.kicad_pcb  # PCBNew layout 
.net        # netlist
.xml        # bill of materials 
.dxf        # mechanical outline drawings
symbols/ 
    .lib    # info to draw the symbol
    .dcm    # symbol documentation info
footprints/
    .pretty # library of .kicad_mod parts
gerbers/
    name-Edge.Cuts.gm1  # board outline
    name-F.Cu.gtl       # top copper
    name-F.Mask.gts     # top mask
    name-F.SilkS.gto    # top silk
    name-B.Cu.gbl       # bottom copper
    name-B.Mask.gbs     # bottom mask
    name-B.SilkS.gbo    # bottom silk
    name-In1.Cu.g2      # internal plane 1 (if 4-layer)
    name-In2.Cu.g3      # internal plane 2 (if 4-layer)
    name.drl            # drill file
print/
    sch.pdf # pdf of the schematic
    pcb.pdf # pdf of the PCB layout
{% endhighlight %}

The .gitignore file is incredibly simple:

{% highlight bash %}
# Backup files
_autosave*
*.bak
*.kicad_pcb-bak
{% endhighlight %}

To simplify keeping track of my library parts, I've decided to avoid the old KiCad .mod files entirely. If there's an old footprint that appears to be exactly what I need, I'd rather take the time to make a .pretty and share that instead.  

<hr>

##### Distributing KiCad Files

I'm a little worried about how to distribute KiCad project git repos when I'm internally referencing .pretty repos and library files that aren't local. I know we have -cache.lib files, but what about .pretty? I haven't actually reached a satisfactory solution for this yet. 

<hr>

##### Using a Template for New Projects

<a href="http://oshpark.com">OSH Park</a> is my fab house of choice, so I've created a project template that matches OSH Park's design rules and contains my most commonly used sizes of tracks and drills. I also have some of the Project Settings filled in for the schematic and PCB layout frames, so I don't have to repeat myself every time I create a new project. 

The template also automatically hides the extra layers I don't want to deal with, both in the layer setup screen and in the render tab under layout. I'm not concerned with mass fabrication at the moment, so I've hidden a lot of the fab and assembly-related layers.

It's important to me to make the transaction cost as small as possible for a KiCad design session. If I sit down and have to do a lot of tedious typing, it's more likely I'll make a mistake, or just skip the session all together. 

Also, if someone's going to pay me for five hours of design work, I don't want to work for an hour for free just trying to set up the project correctly. To that end, I wrote a script called `makenewkicadpro` that I use to start up a new project. I run it in the terminal in Linux in the parent folder where I want to place the new project folder. 

`makenewkicadpro 2 projectname`

This creates a directory called `projectname`, changes directory into it, copies over the template contents from the 2-layer template, and renames all of the project files to match the projectname. Then I can just run `kicad projectname.pro` and I'm ready to work.

The template and script are available from the <a href="https://github.com/wicker/wickerlib-kicad">wickerlib-kicad repository</a> on Github.

<hr>

##### Using KiCad with OSH Park

I've written a help page on the OSH Park Docs site about things to consider when using KiCad to design a board and create gerbers. 

- <a href="http://docs.oshpark.com/design-tools/kicad/">Using KiCad with OSH Park</a> (general info)
- <a href="http://docs.oshpark.com/design-tools/kicad/generating-kicad-gerbers/">Generating Gerbers and Drills</a> (with screenshots!)

<hr>

##### KiCad Suggested Workflow 

I highly recommend the high-level workflow image in the KiCad Getting Started guide, since it's really does cover all the options. 

<a href="http://docs.kicad-pcb.org/en/getting_started_in_kicad.html">Here's the full getting started guide</a> and <a href="http://docs.kicad-pcb.org/en/images/kicad_flowchart.png">the flowchart</a>.

<hr>

##### My Workflow

I generally use a much simplified flow that matches that chart, with the exception of the GerbView preview images. I use GerbV to check my previews since there are some resolution differences in the drills between GerbView and GerbV, but I know that OSH Park fabrication matches 100% what I see in GerbV. 

You can get the free, open source <a href="http://gerbv.geda-project.org/">GerbV</a> here. It's a part of the gEDA project.

Personally, here's the rough order of how I design in KiCad:

1. I use my script to create the project from a template.
1. Open EESchema and edit the project settings.
1. Create the schematic in EESchema.
1. Add symbols and create them as necessary.
1. Wire everything up.
1. Annotate the schematic.
1. Perform a rule check.
1. Use CvPCB from inside EESchema to associate footprints. 
1. Create footprint in Footprint Editor if necessary.
  - Create a new library
	- Use the Footprint Library Wizard to find the library
	- Select active library
	- Save footprint in active library
1. Generate the netlist in EESchema.
1. Open the layout in PCBNew and edit the project settings.
1. Read Netlist in PCBNew
1. Place the footprints in PCBNew.
1. Add ground and VCC planes where appropriate.
1. Create layout in PCBNew, routing traces and adding text. 
1. Perform Design Rule Check.
1. Create Gerbers using Plot.
1. Inspect Gerbers in GerbV
  - I don't use KiCad's GerbView! I use <a href="http://gerbv.geda-project.org/">gEDA's GerbV</a> for OSH Park!
1. Upload the Gerbers to OSH Park to check preview images. 
1. Make changes (particularly silk/mask) and go to Step 16.
1. Generate the BOM.
1. Order parts, and then, only then:
1. Order boards.
1. Create PDFs if desired.
1. Save the project files and commit to version control.

<hr>

##### Project Settings

So when I run `makenewkicadpro` and open up my project for the first time in KiCad, there will only be the minimal structure. 

<img src="/img/kicad/simple-new.png">

KiCad's menus are set up so you'll need everything in them generally from left to right, as you go along. In this case, the menu items are, in order: EESchema, the Symbol Library editor, PCBNew, the Footprint Editor, GerbView, Bitmap2Component, Trace Calculator, and Worksheet Editor.

I only ever use EESchema, PCBNew, the trace calculator, and the worksheet editor from this main project manager window. In order to keep things simple, I only open the symbol and footprint editors from inside a schematic or layout.

So, therefore, the first thing I do is open up EESchema to look at my schematic.

<hr>

##### EESchema Workflow

I used a template so my Electrical Rules Checker is already set properly, and the schematic is initialized with a grid in Inches with spacing of 50 mils. 

The last thing before starting the schematic is to go to `File > Page Settings` and add in the Issue Date, Revision, and Title. Everything else is already filled in so I don't have to worry about it.

<img src="/img/kicad/page-settings.png">

What this does is fill in the lower right-hand corner box with my information for this schematic. I only have to do this once, I immediately feel very professional and legitimate, and I never have to worry about it again. 

<img src="/img/kicad/worksheet-settings.png">

<hr>

##### Custom Hotkeys

The KiCad libraries are excellent and have a lot of what I need, so I've been using them for most components. They're added in much the same way as Eagle with the `a` key.

I hate moving my hand back and forth from the keyboard to the mouse to click, so I use my laptop touchpad to hover the mouse and hotkeys for all the actions, like grabbing, placing wires, or adding labels. 

I've been slowly mapping hotkeys to be entirely left hand, so I can use my right on the touchpad and everything else with the left.

To that end, the very first hotkey I suggest changing is removing a component form `Delete` to `d`. 

My other hotkey notes

- `d`   deletes a part
- `shift+d`  deletes a node
- `a`		adds a part
- `g`   grabs a part along with its connected wires
- `m`		moves a part separate from its connected wires
- `r`		rotates a part
- `w`		adds a wire
- `l`	  adds a label, effectively naming a net
- `x`		flips part across x axis (up/down)
- `y`		flips part across y axis (left/right)

<hr>

##### Frustrating Differences from Eagle

Like a lot of people, I started out using Eagle and ran into some confusing differences. For one thing, the schematic editor is actually a program of its own called EESchema that's wrapped up in KiCad, so it can be launched on its own from the command line. I'm basically never doing that; I'm afraid of breaking the links between the schematic and layout, for example, so I always launch the editor programs from inside KiCad.

For another, you can't just type full names of things in a command line bar at the top of EESchema like you could in Eagle.There's no way to type `grid inch` or `grid 0.01` to swap the grid from metric to inches or to set it to a grid of 10 mils, for example. There aren't even hotkeys, so you're forced instead to click the mouse for some of the meta settings changes. 

In this case, a right click gives you the option to change the grid. 

Also, the KiCad grid default is 0.05" (50 mils) which can be a shock when you're used to using 0.1". It's best to stick with 0.05" in EESchema because lots of the default component symbols are created using that spacing and if you're using 0.01" it's possible to end up being unable to match wires to pins in the EESchema grid. 

Another big difference is that `m` to move a part won't automatically bring along the rest of the attached wires. For that, you need to think of the grab tool, `g`, as equivalent to the `move` tool of Eagle.

Now, crazily, I'm tempted to explore using `End` and `Return` for mouse clicks. I could see that allowing me to keep all ten fingers on the keypad for touch typing, and just drop a thumb to the touchpad for mouse positioning. But muscle memory of the mouse click is strong, so...

Hitting `Ctrl` before selecting will let you drag and keep wires attached. To move a wire, right click and hit `break wire` and then use `Ctrl` to select and drag that wire. 

Preset trace names were really useful, since I could map them to Power and Default trace settings.

I have to explicitly add wires/connections. Can't just move parts and overlap them and pull them away. That's annoying.

When I need to edit or add a part, I'm careful to only open the Part Library Editor from inside EESchema. Having an established flow makes it less likely I'll make a mistake later when I'm in a rush, tired, or not paying attention.

<hr>

##### Schematic Parts Editor

Adding a component is easy. Editing a component is confusing because it's not clear what and where the editing is happening, not is it clear what exactly should be loaded into the library editor. 

First, before using the Schematics Parts Editor or Footprint Editor to edit libraries, I had to remember some things about KiCad and libraries. figure out what KiCad thinks a library is. Now that I know where the libraries are, I'm ready to add and edit them. This works as long as I'm very clear about what I'm trying to do. 

First off, when I need to edit or add a part, I open my schematic in EESchema and go to `Tools > Library Editor`.

When it opens, the editor will say `Part Library Editor: no library selected` in the title area at the top of the program. 

<img src="/img/kicad/nolibrary.png">

What I do next depends on whether I'm working with an existing or new library. I can: 

1. Edit an existing part in an existing library.
1. Add a new part to an existing library.
1. Add a new part to a new library.

**Existing Library**

To edit or add a component to an existing library, I first go to `File > Current Library` and select a library from the list. If I'm using a standard KiCad library, the library will open but the title bar will now say `Parts Library Editor:` with the location and name of the repo, along with `[Read Only]`. 

<img src="/img/kicad/libraryreadonly.png">

I actually think stopping me from writing over the existing official KiCad libraries is a good thing. I can open the components and copy them into my own wickerlib libraries, but I can't replace them with unproven component information. 

If I wanted to edit an existing component from this `atmel.lib` library, I would have to create a new library and either a) copy the symbol over, or b) reproduce it from scratch. 

The important thing to know here is that the `File` menu only deals with **libraries** and it's the little red op amp menu symbols that actually save, upload, or download individual **symbols**. I often want to hit `Ctrl+S` and save the symbol, but that's going to be frustrating because KiCad thinks I want to save the **library**. Instead, I have to choose the correct **symbol** icon.

<img src="/img/kicad/symbolicons.png" style="max-width: 158px;float:right;">
There are four of them:

1. Add a new component.
2. Load an existing component to edit from the current library.
3. Create a new component from the current component.
4. Update the current component in current library.

Since I'm in an existing, read-only library, I can either create a new component, but not save it here (option #1) or load an existing component from this library and choose to create a new component from the current one (option #2, then option #3). 

In both cases, I end up with a component in the window that I'm ready to save. 

If I want to edit a component in a library that's not read-only, I'll choose the second option and select the component. Then, depending on how major the changes to the symbol are, I can either use the third option to create a new component alongside the existing one, or I can overwrite the existing component. 

Usually, if the changes are minor or desired to modify a current schematic, I'll overwrite.

Otherwise, if the changes are significant and might affect previous schematics, I'll create a new component to be safe. 

Because the library already exists, I can close the Parts Editor and all I have to do back in the EESchema is hit `F3` to redraw the schematic to redraw any existing components. 

If I added a new component, it will automatically be available in my library so I just have to hit `a` to add and find it. 

**New Library**

If I'm adding a brand new kind of part and I don't have a suitable library for it, I have to open the Library Editor and create a new component. I usually just add an outline of something and then go to `Export Component` in the menu bar and save it with the same naming scheme with my other custom libraries.

In this case, I chose to call it `wicker-test.lib` and to save it in my `~/proj/kicad/wickerlib-kicad/symbols/` directory.

Now that the library exists, I have to make it available for use by going to `Preferences > Component Libraries` and using the top `Add` button to `Add a new library after the selected library and load it`.

I navigate to the right directory and select the `wicker-test.lib` file. When I scroll down to the bottom of the list, my new library will be there.

Now the library exists, so I can go to `File > Current Library` and select it from the list. The title bar at the top will now say `Part Library Editor` with the name of the library.

Any further work I do is now the same as with an existing library. The componentI loaded is already present, so all I have to do is edit and `update current component in current library.`

<hr>

##### Using the Schematic Parts Editor

The user interface and hotkeys are very similar to the schematic editor.

- `g` grabs and lets you move the side of a box
- `m` moves things
- `p` adds pins
- `r` rotates things

The default grid in KiCad is 0.05" (50 mils) so I stick with that or 0.1" (100 mis) for all of my created parts so there's no conflict in EESchema with a 0.05" grid. If the parts are created with 0.05" pin spacing and EESchema is set to use 0.1" (which was what I always used in Eagle), then wires might not be able to be hooked up to pins because of the grid offset. 

There don't appear to be any hotkeys to draw boxes, but you can resize boxes with the `g` grab tool. The graphics here are somewhat limited. 

KiCad doesn't really care about the pin names, so I usually just match them to the datasheet. What KiCad <em>really</em> cares about is the pin numbers, though it doesn't matter until I'm actually associating footprints. But it really matters then.  

On giant parts, it's handy to batch import pins from a spreadsheet, but I havn't figured out how to do that yet. There's an option to hide power pins, but for now I like to have all the pins visible. 

Since the number of symbol pins have to match the number of pads in the footprint, there's a visual trick where you can stack sets of pins that are the same on top of each other. This can be handy for GND or VCC pins, but I like to see at a glance what all my pins are doing, so I leave them separate. 

Keep in mind that the pin numbers connect a symbol to its footprint; adding labels to the traces will allow you to see that trace name on the layout while you're routing traces to connect different packages together. The pin number on one package does not have to match the pin number on another package!

<hr>

##### Using the new Symbols

If it was a new library, EESchema (being a separate program and all) doesn't know that there's a new library. I have to go to `Preferences > Component Libraries` and `Add` it to the list.

If it was just a part added to an existing library, EESchema will update automatically when I use the `a` hotkey to look for components to add.

<hr>

##### Power Flags

This is a little odd but KiCad's electrical rules checker (ERC) expects each power rail and each ground/return rail to be marked somewhere with a Power Flag. This isn't the usual ground or vcc flag, which I use all over the place, but it's an additional symbol accessible in the same menu that's literally called PWR_FLAG. 

A power rail is also called a power net, and it includes all the instances of that signal everywhere in the schematic. In the image below, I only have to put one power flag on each power net. I have two PWR_FLAGs, one each on`+5V` and `GND`, but the second instance of +5V doesn't need a flag since that net already has one. 

I usually leave placing this for last, to satisfy the rules checkers, since I ususally end up tweaking the schematic placement quite a bit and extra symbols are just annoying to keep track of.

There's also a convention in schematics to always place a junction where more than two wires meet, but never have more than three wires meet there.

<img src="/img/kicad/powerflags.png">

<hr>

##### Text

I don't see enough text around open source schematics on the web, so I just want to point out that the point of a schematic is to functionally describe the circuit so someone else can go and (with the help of the datasheets) lay out the board separately from the original schematic designer.

To that end, I like adding text (sometimes as a note to myself!) to remind myself what I was thinking. Clear, simple explanations can save a lot of hassle later.

This is the same schematic as used above with the power flags, but note that I only had to add a PWR_FLAG to the GNDA rail. I already have one on the +5V and the GNDD rails. Just use one per net! 

<img src="/img/kicad/text.png">

<hr>

##### Running the ERC

Do it! 

<hr>

##### Connecting Symbols and Footprints 

When the schematic is done and saved, the next step is to create the netlist. 

Then, I'll use CvPCB to connect my footprints. I've heard the next version of KiCad will change this step, but it works for now, sort of.

If a footprint isn't available and I have to create one, I'll open the Footprint Editor from inside EESchema. I don't want to go to the PCBNew layout side of things until I've got everything ready. 

At this point, before I create or select the footprints, I create my Bill of Materials (BOM) and figure out where I'm going to buy the parts, since it's just a huge waste of time to decide on a part and then find out later that it's not in stock anymore. 

Once I have the BOM, I can figure out whether the footprints for my chosen parts are available or whether I'll have to create them. I open up CvPCB and usually put this in the bottom of my screen, and open up the Footprint Preview using the icon in the top menu bar. As I move through the parts, the preview updates automatically, so I don't have to open the Footprint Editor. It's all right there.

<img src="/img/kicad/cvpcb.png">

The parts can be filtered by keyword, pin count, or library -- or any combination of the three. This is still super confusing and hard to get the hang of for me.

<hr>

##### Visible Menu and Layer Colors

One of the first things to realize in the Footprint Preview, since it'll be the same in the Footprint Library Editor and in PCBNew, is to know exactly what the color scheme is for each of the layers. 

In PCBNew, there's a `Visibles` menu on the righthand side of the screen. It has two tabs: `Layer` and `Render`.

My default template hides a lot of the assembly-related layers in the `Layer` tab, since I'm only interested in the basic layers that I need to get the board fabricated. If I were doing even a small production run, I'd be paying attention to those other layers to help guide the assembly process. 

The `Render` tab is useful for turning on and off things like pads, different kinds of vias, the unrouted wires (ratsnest), the grid, and other things. It's good to know it's there when something's gone missing. 

<img src="/img/kicad/visiblesmenu.png">

<hr>

##### Footprint Editor

Anyway, back to the Footprint Editor. If I need to add a footprint, the first thing to do is open up the editor. I can get to it from EESchema or from PCBNew, and I'll edit footprints from either place. 

<img src="/img/kicad/eeschematop.png">

Just like the schematic editor, it says no active library; that's OK, because I'm going to select one. I'm not creating anything, just browsing, so it doesn't matter if my libraries are read-only.

Remember, the `File` menu opens and saves the `.pretty` library itself. The green-padded footprint icons in the top bar open, copy, and work on the `.kicad_mod` footprint files themselves.

<img src="/img/kicad/footprinteditor-top.png">

I basically just open the Footprint Library, go to `File > Set Active Library` and select the library I'm thinking about editing. 



<hr>

##### Creating Footprints

Most of the hotkeys are the same. The template should take care of whether it's mm or inches. 

Click and dragging to select seems to work for moving, though it doesn't grab.

Holding shift or control while clicking and dragging makes a copy of the whole group.

Holding shift and control while clicking and dragging deletes everything that was selected.

I use 'd' for delete, because it lets me move with the right hand on the mousepad and use the left hand. All my hotkeys should be like this.

Would like to just hit enter from anywhere in the Footprint Editor Footprint Text Editor window.

`w` for next track width
`shift+w` for previous track width
`i` existing select copper track
`v` toggles between top and bottom copper layers

<hr>

##### Finish up CvPCB

Go back and assign any new footprints. Then, most importantly, in order: 

1. Run the ERC again.
2. Generate the netlist.
3. Run the BOM if you want.
4. Click on `PCBNew` to go to the layout.

<hr>

##### PCBNew Page Settings

For those of us coming from Eagle, the very first thing you see when you open up PCBNew is a real "WTF?!" moment, because it's just a blank page with a fab drawing outline. That's OK, because the very first thing I do is go to `File > Page Settings` and fill in the worksheet information like I did the first time I opened EESchema.

<img src="/img/kicad/pcbnew-page.png">

Another note here is that I started out using the black background for routing and that's what I'm comfortable with, so all my templates use it automatically. I don't know if there's a way to change it but this is just How I Was Taught and it's fairly standard practice.  

<hr>

##### Importing the Netlist

Once I've updated the page settings, I'm ready to tell PCBNew what my schematic already knows: which footprints to use and which pads are connected. My schematic created this information when I added symbols and drew all the wires; the information itself is saved in the netlist, usually called `myprojectname.net`.

I tell PCBNew about the netlist by clicking on the `NET` icon:

<img src="/img/kicad/netlist.png">

PCBNew brings up the Netlist menu and I leave all the default options in place before clicking `Read Current Netlist` at the upper right. If everything went OK, the `Messages` window at the bottom will have no red writing. Everything will be green to indicate success or gray to indicate information.

When I click `Close`, I get another shock: KiCad likes to put all the footprints on top of each other at the center of the screen. First thing is to zoom out, then the second is to get these footprints separated. This is where the canvas options come in.

<hr>

##### Default Canvas for Auto Placement

The canvas has to be in Default mode for the autoplacement to work, so the first thing is to hit `F9` or go to `View > Switch Canvas to Default`. This is what this canvas looks like, with all the parts on top of each other.

<img src="/img/kicad/stacked-parts.png">

Next, I make sure Mode is selected. 

<img src="/img/kicad/mode.png">

Then, I can right-click anywhere in the layout area and select `Global Spread and Place`. Then, I've never had much luck with the option to `Automatically Place All Footprints` so I just click `Spread Out All Footprints`. 

PCBNew will ask me if I really want to move all the components that aren't explicitly locked in place, and I'll say `Yes`. 

<img src="/img/kicad/spread.png">

Finally, that's something I can work with! Now, the only other thing to really talk about before starting to route is the Visibles tab on the right-hand side of the screen.

<hr>

##### Default Trace and Via Sizes

The cool thing about a template is it lets you set your DRC rules and standard track sizes. They're easy to switch between.  

<img src="/img/kicad/defaults.png">

<hr>

##### OpenGL Canvas for Interactive Routing

One of the coolest new features of the last two years has been the addition of interactive, dynamic routing. It only works on the OpenGL canvas, so the first thing is to hit `F11` or go to `View > Switch Canvas to OpenGL`. I prefer this canvas in general so I usually just use the Default canvas for the initial placement, and then I'll switch to OpenGL the rest of the time.

There's a <a href="https://www.youtube.com/watch?v=CCG4daPvuVI">good explanatory routing video made by the CERN folks</a> on YouTube. 

<hr>

##### PCBNew General Stuff

The lefthand menu shows things that have already been placed. The righthand menu has the action buttons to place things like traces, footprints, polygon fills, artwork, and text.

For routing in general, I do a lot of `x` and `escape` to start and stop the routing. 

Global spread and place might be useful, but my workflow is often to grab individual sets of components. I like `t` to get and move a particular component. 

<hr>

##### Updating and Changing Footprints 

Footprints can be updated easily by editing the library, editing the CvPCB association, regenerating the netlist in EESchema, and re-importing the netlist in PCBNew. Think of EESchema as the driver and PCBNew as the passenger; EEschema knows about the function of the circuit, while PCBNew just has to interpret the instructions by connecting footprint pads. 

<hr>

##### Board Outline (Edge Cuts)

At some point, I need to put down the outline. It's an annoying box since it's broken up into straight line pieces and it's not easily resized, so I don't usually worry about it until I've got the placement worked out and I'm ready to start routing properly.

I place the outline by selecting Edge.Cuts as the active layer, and using the graphics for circles, arcs, and lines from the righthand menu.

<img src="/img/kicad/outline.png">

OSH Park in particular processes board outlines a little differently from most fabricators, and wants to see a watertight board outline on the Edge.Cuts layer <em>by itself</em>, with no extra text or anything. Cutouts, slots, and notches are OK but any extra measurement text or fab notes are interpreted by the website upload system as part of the board outline to be milled. It's generally better to enable one of the assembly-related documentation layers and put those notes there instead. 

<hr>

##### Polygon Fills

This is the fill zone icon -->

What menu comes up depends on which layer is highlighted when the fill zone icon gets clicked.

And polygon fills leads right into the Design Rules Check because fills don't automatically update, so you can either get in the habit of hitting `b` (redraw fills) a lot, which is a good habit, but you also should definitely run DRC before generating any plots. 

<hr>

##### DRC

Have to deal with the DRC. Do it. 

And polygon fills leads right into the Design Rules Check because fills don't automatically update
Mask/silk overlap? 
<hr>

##### Videos and References

I used the free videos of Contextual Electronics along with the KiCad Like a Pro and associated ebook to get the hang of KiCad. It took a couple of long, dedicated sessions of active learning with a notepad close at hand. 

- <a href="http://kicad-pcb.org/">KiCad Homepage</a>
- <a href="http://kicad-pcb.org/help/documentation">KiCad Official Documentation</a>
- <a href="http://kicad-pcb.org/help/external-tools/">External Tools and Footprint Generators</a>
- <a href="http://kicad.info/">KiCad.info User Pages</a> 
- <a href="https://groups.yahoo.com/neo/groups/kicad-users/info">kicad-users Yahoo Group</a>
- <a href="http://docs.oshpark.com/design-tools/kicad/">Using KiCad with OSH Park</a>
- <a href="https://contextualelectronics.com/course/software-tutorials/kicad-tutorial/">Contextual Electronics Tutorials</a>
- <a href="https://www.udemy.com/kicad-pro/">KiCad Like a Pro Udemy Course</a>
- <a href="https://rheingoldheavy.com/design-assembly-kicad/">Design for Manufacture</a>

