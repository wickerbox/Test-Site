---
layout: page
title: Wickerlib
permalink: /wickerlib/
---

### Philosophy

- Wickerlib is not for other people. Wickerlib is for me in my quest to turn around projects as quickly and with as little uncertainty as possible. I haven't seen a lot of people talking about this approach and certainly not a lot of people using KiCad yet, so I think the information might be valuable as folks switch and build their experience levels with circuits. 

- I like circuits. I want to learn more about circuits. I’m lazy and I don’t like doing things twice.

- I use KiCad. I think part of why it’s hard to learn is because designing circuits requires thinking a certain way, and we don’t do a good job of communicating that.

- If someone comes to me and says, I'd like you to build me ... X ... I want to have those tools at my disposal. As a generalist, I want enough to get the job. To talk knowledgeably about every small part of things. 

- Tools I use:
  - Github
  - Workflowy
  - Gmail
  - Phone
  - Tmux
  - Mnemosyne
  - Skype
  - YNAB
  - Dropbox


### Process and Workflow

1. Balancing Client Projects and Personal Projects
1. Philosophy. Why bother?
1. KiCad Templates simplify starting a new project
1. Wickerlib Symbol and Footprint Library Management
1. Maintaining an Inventory
1. First Schematic and Editing Components
1. Netlist and Footprint Association
1. Generating Bills of Material
1. Pre-fabrication Checklist
1. KiCad Board Layers
1. First Board and Editing Footprints
1. Generate Gerbers
1. Generate Paste Layers for Stencils
1. Generate Assembly Diagrams
1. Makefile Magic
1. Order Parts
1. Assembling the Boards
1. Notes on Testing and Bringing up Boards

### Templates

### Wickerlib

- Symbols (.lib, .dcm)
- Footprints

Collected kicad_mod files in a folder. 

Built it from an inventory.

- wtf any way to batch-invisible the pin names? how is this happening in original kicad symbol libs?  

### Makefiles and Scripts

### Reference Circuits

{% assign sorted_articles = site.wickerlib | sort: 'num'  %}
{% for article in sorted_articles  %}
  - <a href="{{ article.url }}">{{ article.title }}</a> {{ article.num }}
{% endfor %}

