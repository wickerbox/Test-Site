---
layout: toolbox
title: Library Management
permalink: /circuits/the-weekly-circuit/library.html
image: /img/thumbs/kicad.png
num: 12
---

## 

Schematics are functional descriptions of circuits that don't have anything to do with the board layout. In a lot of companies, the people who actually lay out the board aren't the same people who design the schematic. 

When I started, I hated that Eagle forced me to pick a final footprint when I was still just trying to figure out how the schematic worked. It didn't feel natural in my design process. 

KiCad keeps symbols and footprints in separate libraries and doesn't force you to associated them until you want to. This was a big plus for me and it made the switch to KiCad a little easier.

Initially, I kept a pile of symbol libraries and a pile of footprint libraries. When the default KiCad libraries didn't have what I needed, I created new symbols and footprints for my own libraries. Everything was scattered around, which made it hard to keep track of.

Most of my libraries only had the variables for Name, Value, Footprint, and Datasheet.

Eventually, I had enough parts floating around my workroom that I needed some way to keep track of them, so I could buy in bulk and then use parts I already had in stock. 

The easiest way to keep track of interchangeable parts is to create a company-specific part numbering system and create symbols based on that numbering system. The end result here is to create a schematic and select symbols that have a company part number. They'll come immediately with information about the associated part, including footprint. 

Rheingold Heavy has an article about the making <a href="https://rheingoldheavy.com/kicad-bom-management-part-1-detailed-components/">detailed components</a> that will help generate KiCad Bills of Material (BOMs). 

I stopped thinking about symbols and footprints as two equal halves of a whole functional description of a part. Instead, I started seeing the symbols as most of the description. 

I have a company part number, a symbolic picture on the schematic, and an associated footprint. I realized that the symbolic picture (component) is most of the data.

A company part number could be associated with multiple symbolic pictures. In my bill of material system, all of these are valid at the same time: 

|Inventory Number|Symbol|Footprint|
|----------------|------|---------|
|0001|Symbol configuration #1|8DIP|
|0002|Symbol configuration #1|8SOIC|
|0001|Symbol with hidden pins|8DIP|

I thought back to my first electrical engineering job for ideas on what worked best. Combined with the Rheingold Heavy article suggestions, I came up with this list of additional 

The next step was to inventory my workroom. That took ... a while. I made up a spreadsheet with all of my component parts. Another name for inventory number is a SKU, or stock keeping unit.  

- Reference
- Value
- KiCad Footprint
- Datasheet
- WBox_SKU
- Description
- Package
- MF_Name
- MF_PN
- S1_Name
- S1_PN

Armed with this list, I did some reading from the Rheingold Heavy article and a couple others that were linked, including Spark's <a href="http://www.proto2prod.com/proto2prod/2015/3/11/creating-and-optimizing-a-bill-of-materials-1">Creating and Optimizing a Bill of Materials</a> and Bunnie Huang's <a href="http://www.bunniestudios.com/blog/?p=2776">The Factory Floor</a>.

I decided to use all of the above variables in my library parts in KiCad, but also to add a second source and part number. I was also going to look into how KiCad generates the position and orientation, and make sure my ultimate exported BOM included those values:  

- S2_Name
- S2_PN
- Position
- Orientation

Luckily, at some point if I'm ever ordering giant production runs, KiCad files are all text editable so I can go back and use sed to batch-insert other variables. For now, for prototype runs, the variables on this list will save me a lot of time and struggle at the moment when I need to make an order. 
  
### Add parts to the library

My parts would all be entirely described in a single symbol library (wickerlib.lib) and a single footprints library (wickerlib.pretty) because KiCad has a decent search function once you've opened a particular library. This way, I can also just add parts to the libraries and never have to worry about sorting, categorizing, or placing parts in the correct library.

I created a Python script that walked through a .csv version of my inventory. Every SKU should have at least one entry, though it may have multiple entries because there may be more than one symbol attached to that stock item. Each stock item necessarily only has one footprint. Components with different packages have different stock numbers. 

### Properties and Where to Find Them

Properties both in the symbol and the main KiCad schematic editor.

- Do schematic editor template options follow the schematic or are they attached to the KiCad program itself? Where are they stored?

Ideally, you would make up the list of properties you want them to have. That will be the first four plus some set of others.

### Setting up Schematic Editor Properties to Match What You Want

In the schematic editor, use `e` to open a component's properties. There should be the first four default fields of Reference, Value, Footprint, and Datasheet. 

<img src="/img/toolbox/component-properties-top4.png">

To add properties to this list, you have to add them in the KiCad Schematic Editor under Preferences > Schematic Editor Options, on the Template Field Names tab. 

<img src="/img/toolbox/template-field-names.png">

After you've done that, if you go back to the open a component's properties in the schematic, you'll see the whole list.

<img src="/img/toolbox/component-properties-all.png">

Adding a hundred components would suck, but I have a spreadsheet with all these column categories so I'm going to try scripting and just adding lots of entries to the raw symbol text files. 

### Editing Symbols by Hand

This is an example component entry in the DCM file, which holds descriptions, keywords, aliases, and reference links for library symbols.

{% highlight bash %}
#
$CMP ATTINY261V-20SOIC
D  IC MCU 8BIT 8KB FLASH 20SOIC 
K AVR 8bit Microcontroller tinyAVR
F http://www.atmel.com/...datasheet.pdf
$ENDCMP
{% endhighlight %}

This is an example component entry in the LIB file, which is linked to the item of the same name in the DCM. The fields F0 through F10 contain the four default field values defined in plus the fields 

{% highlight bash %}
#
# ATTINY261V-20SOIC
#
DEF ATTINY261V-20SOIC IC 0 40 Y Y 1 F N
F0 "IC" -850 950 50 H V C CNN
F1 "ATTINY261V-20SOIC" 600 -950 50 H V C CNN
F2 "" -950 850 50 H V C CIN
F3 "http://www...datasheet.pdf" 50 1500 50 H I C CNN
F4 "Digikey" -350 1450 60 H I C CNN "S1_Name"
F5 "0002" -350 1400 60 H I C CNN "Wbox_SKU"
F6 "IC MCU 8BIT" 300 2050 60 H I C CNN "Description"
F7 "20SOIC" -300 1650 60 H I C CNN "Package"
F8 "Atmel" -350 1750 60 H I C CNN "MF_Name"
F9 "ATTINY861V-10SU" 250 1750 60 H I C CNN "MF_PN"
F10 "ATTINY..ND" 400 1850 60 H I C CNN "S1_PN"
DRAW
ENDDRAW
ENDDEF
{% endhighlight %}

- F0 looks like Reference
- F1 looks like Value
- F2 looks like 
- F3 looks like the datasheet link

The other field entries occur out of order and in no particular order. 

You can only edit the DCM values by opening the part in the Part Library Editor and 'Edit Component Properties' using the ABC/gear symbol. 

You can use the black T symbol to 'Add and Edit fields and edit field properties' manually, but that takes a lot of time and effort.

<img src="/img/toolbox/editor.png">

The DCM file has nothing to do with the values in LIB. The Documentation File Name entry there is only reflected in the DCM. The only reason to use the DCM is because the desciprtion is used to search, which is incredibly useful to find the parts in KiCad's search. The documentation link there doesn't go anywhere else, and F3 is the datasheet link from in the schematic editor.  

<img src="/img/toolbox/dcm-source.png">

You can add any fields you want to the actual physical LIB entry, and KiCad will add them whether or not the schematic template contains the list of fields. It also includes any additional ones from KiCad's schematic editor options.  

##### Questions

You can't determine which category based on the F-number; F0, F1, etc. You have to determine if a line is a field entry and read the last element in the line to determine the name of the category. 

- What's the difference between F in DCM and Datasheet entry in LIB? F in DCM has nothing to do with anything, it just sits in the documentation file. No effect on datasheet entry in LIB. I'm going to exclusively bother with the datasheet entry in LIB and only use the DCM entries for keywords for searching. 

- What happens when you manually add field entries that are duplicate F-numbers? Doesn't seem to matter.
- What happens when you manually add field entries out of order in text and then open that entry in the library editor? F1, F2, F8, F3, F7, F5, F4, F6?  Doesn't seem to matter. 


I entered them all like this, with all additional entries as F5: 

{% highlight bash %}
#
# ATTINY261V-20SOIC
#
DEF ATTINY261V-20SOIC IC 0 40 Y Y 1 F N
F0 "IC" -850 950 50 H V C CNN
F1 "ATTINY261V-20SOIC" 600 -950 50 H V C CNN
F2 "" -950 850 50 H V C CIN
F3 "http://www.agoof.html61-attiny461-attiny861_datasheet.pdf" 50 1500 50 H I C CNN
F5 "Digikey" -350 1450 60 H I C CNN "S1_Name"
F5 "0002" -350 1400 60 H I C CNN "Wbox_SKU"
F5 "IC MCU 8BIT 8KB FLASH 20SOIC " 300 2050 60 H I C CNN "Description"
F5 "20SOIC" -300 1650 60 H I C CNN "Package"
F5 "Atmel" -350 1750 60 H I C CNN "MF_Name"
F5 "ATTINY861V-10SU" 250 1750 60 H I C CNN "MF_PN"
F5 "ATTINY861V-10SU-ND" 400 1850 60 H I C CNN "S1_PN"
{% endhighlight %}

In the schematic file, they get placed in order based on the Template FIeld Names order: 

{% highlight bash %}
$Comp
L ATTINY261V-20SOIC IC?
U 1 1 56F8432A
P 9000 2200
F 0 "IC?" H 8150 3150 50  0000 C CNN
F 1 "ATTINY261V-20SOIC" H 9600 1250 50  0000 C CNN
F 2 "" H 8050 3050 50  0000 C CIN
F 3 "http://www.agoof.html61-attiny461-attiny861_datasheet.pdf" H 9050 3700 50  0001 C CNN
F 4 "Digikey" H 8650 3650 60  0001 C CNN "S1_Name"
F 5 "0002" H 8650 3600 60  0001 C CNN "Wbox_SKU"
F 6 "20SOIC" H 8700 3850 60  0001 C CNN "Package"
F 7 "Atmel" H 8650 3950 60  0001 C CNN "MF_Name"
F 8 "ATTINY861V-10SU" H 9250 3950 60  0001 C CNN "MF_PN"
F 9 "ATTINY861V-10SU-ND" H 9400 4050 60  0001 C CNN "S1_PN"
F 10 "IC MCU 8BIT 8KB FLASH 20SOIC " H 9300 4250 60  0001 C CNN "Description"
  1    9000 2200
  1    0    0    -1  
$EndComp
{% endhighlight %}

- What if the list of field categories in the part doesn't match the list that's in KiCad schematic view? I added an extra field called Extrafield. The library part doesn't get updated with this, but the .sch file does. Cache lib does not. 

Adding extra fields in the schematic editor only touches the .sch file. 

{% highlight bash %}
$Comp
L ATTINY261V-20SOIC IC?
U 1 1 56F84468
P 8850 2175
F 0 "IC?" H 8000 3125 50  0000 C CNN
F 1 "ATTINY261V-20SOIC" H 9450 1225 50  0000 C CNN
F 2 "" H 7900 3025 50  0000 C CIN
F 3 "http://www.agoof.html61-attiny461-attiny861_datasheet.pdf" H 8900 3675 50  0001 C CNN
F 4 "0002" H 8500 3575 60  0001 C CNN "Wbox_SKU"
F 5 "20SOIC" H 8550 3825 60  0001 C CNN "Package"
F 6 "Atmel" H 8500 3925 60  0001 C CNN "MF_Name"
F 7 "ATTINY861V-10SU" H 9100 3925 60  0001 C CNN "MF_PN"
F 8 "Digikey" H 8500 3625 60  0001 C CNN "S1_Name"
F 9 "ATTINY861V-10SU-ND" H 9250 4025 60  0001 C CNN "S1_PN"
F 10 "IC MCU 8BIT 8KB FLASH 20SOIC " H 9150 4225 60  0001 C CNN "Description"
F 11 "Foo" H 8850 2175 60  0001 C CNN "Extrafield"
  1    8850 2175
  1    0    0    -1  
$EndComp
{% endhighlight %}

- What if the schematic editor has less information than the library? What if I don't want to propagate Wbox_SKU values out into the world? 

It gets added automatically to the component properties!

<img src="/img/toolbox/test-nosku.png">

<img src="/img/toolbox/test-nosku-resultproperties.png">

WBox_SKU shows up in the .sch file:

{% highlight bash %}
L ATTINY261V-20SOIC IC?
U 1 1 56F84552
P 8125 1875
F 0 "IC?" H 7275 2825 50  0000 C CNN
F 1 "ATTINY261V-20SOIC" H 8725 925 50  0000 C CNN
F 2 "" H 7175 2725 50  0000 C CIN
F 3 "http://www.agoof.html61-attiny461-attiny861_datasheet.pdf" H 8175 3375 50  0001 C CNN
F 4 "Digikey" H 7775 3325 60  0001 C CNN "S1_Name"
F 5 "0002" H 7775 3275 60  0001 C CNN "Wbox_SKU"
F 6 "20SOIC" H 7825 3525 60  0001 C CNN "Package"
F 7 "Atmel" H 7775 3625 60  0001 C CNN "MF_Name"
F 8 "ATTINY861V-10SU" H 8375 3625 60  0001 C CNN "MF_PN"
F 9 "ATTINY861V-10SU-ND" H 8525 3725 60  0001 C CNN "S1_PN"
F 10 "IC MCU 8BIT 8KB FLASH 20SOIC " H 8425 3925 60  0001 C CNN "Description"
{% endhighlight %}

- What if schematic and library explicitly disagree?

Library overrides the schematic editor. I set the value of MF_PN to be `TESTSOURCE` and it didn't show up in the .sch or the -cache.lib. The value in wickerlib.lib entirely overrode it.

- What if F0-F3 values are named wrong? 

- What information is kept in the template? 

### Next

- Update all of the designs released through Wickerbox to depend on those libraries, so to use them all anybody has to do is download the KiCad project and the latest wickerlib files.

- With all that in place, I created a Python script that created a BOM. There are a couple of different BOM formats to output, but for now just the simple Github BOM.

### Thoughts

Somebody wants a design. I work out of Wickerlib to start, adding parts and adding SKU numbers even if I don't have any in stock. I update my inventory and I sync my library.

When I'm ready to create gerbers, I create a BOM based on whichever set of info I want. 

I can create a new library and rename everything and clean it up for those other people. 

Or I can just save the .sch, .pro, .kicad_pcb, .net, -bom.csv and call it good.
