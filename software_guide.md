# Zotero Reference Management Software / workflow guide



See here links to useful Zotero plugins, organized by workflow. These will be discussed in the workshop.



<table><caption>Software packages overview</caption>
<tbody>
<tr>
<th>Category</th>
<th>Task</th>
<th>Zotero software</th>
<th colspan="3">Adjacent software</th>
</tr>
<tr>
<th rowspan="5">Library maintenance</th>
<th rowspan="2">Importing from plain text references</th>
<td>Stylio</td>
<td colspan="3"></td>
</tr>
<tr>
<td></td>
<td colspan="3"><a href="#Anystyle">Anystyle</a>, GROBID, Scholarcy Ref</td>
</tr>
<tr>
<th rowspan="2">OCR</th>
<td><a href="#Zotero-OCR">Zotero OCR</a></td>
<td colspan="3"></td>
</tr>
<tr>
<td></td>
<td colspan="3">{ External OCR Program } { Tesseract, Adobe Acrobat, forthcoming macOS? }</td>
</tr>
<tr>
<th>Completing &amp; correcting metadata</th>
<td><a href="#DOI-Manager">DOI Manager</a></td>
<td colspan="3"></td>
</tr>
<tr>
<th rowspan="2">Collaboration</th>
<th>Shared Library</th>
<td rowspan="2">Zotero sync account</td>
<td colspan="3"></td>
</tr>
<tr>
<th rowspan="3">Writing with dynamic references</th>
<td colspan="3">Google Docs, Overleaf</td>
</tr>
<tr>
<th rowspan="4">Writing / citation</th>
<td>{ Native Zotero plugins / connector }</td>
<td colspan="3">Microsoft Word; LibreOffice</td>
</tr>
<tr>
<td>RTF scan</td>
<td colspan="3"></td>
</tr>
<tr>
<th rowspan="2">Writing with Citekeys</th>
<td rowspan="3"><a href="#BetterBibTex">BetterBibTex</a></td>
<td rowspan="2">Text editor, e.g. Markdown, LaTeX, or Word Processor</td>
<td>OS-wide Zotero bridge</td>
<td rowspan="3"><a href="#Pandoc">Pandoc</a></td>
</tr>
<tr>
<td rowspan="2">editor-specific Zotero bridge</td>
</tr>
<tr>
<th rowspan="4">Research:</th>
<th>Concept mapping</th>
<td><a href="#Obsidian">Obsidian</a></td>
</tr>
<tr>
<th>Extracting annotations and highlights from PDFs</th>
<td><a href="#ZotFile">ZotFile</a></td>
<td colspan="3"></td>
</tr>
<tr>
<th rowspan="2">Bibliometric analysis</th>
<td><a href="#Cita">Cita</a></td>
<td colspan="3"></td>
</tr>
<tr>
<td>Voyant-export</td>
<td colspan="3">Voyant</td>
</tr>
</tbody>
</table>

## Zotero* itself

`⚖️ 🧑‍⚖️ 🏛`If you work with primarily with (or at least with a lot of) ==**legal documents,**== note that you may want to instead install [Jurism](https://juris-m.github.io/release/), a specialized fork of Zotero. It is, for the most part, interchangeable with Zotero 

### Installation

The most important thing to install is [Zotero](https://www.zotero.org).

- For macOS and Windows, get the application from the [Zotero download page](https://www.zotero.org/download/).

- For Ubuntu or Debian-based Linux distributions, [follow the instructions here](https://github.com/retorquere/zotero-deb#installing-zotero):

  ```sh
  wget -qO- https://github.com/retorquere/zotero-deb/releases/download/apt-get/install.sh | sudo bash
  sudo apt update
  sudo apt install zotero
  ```

Make sure to also install the connector extension/s for your browser.

Go ahead and launch Zotero. Set up Zotero’s library. 

⚠️ ==CAREFUL:== Do not place Zotero’s library in a cloud-synchronised folder (such as Dropbox or iCloud). WebDAV is ok, though.

⚠️ However, **you are not done**! In order to get the most important benefits from using Zotero, you need to install and set up a few plugins:

## Plugins & other software

📍Note: you can find a list of additional plugins on the Zotero website.

### ZotFile

[[website](http://zotfile.com/) | [github](https://github.com/jlegewie/zotfile)]

#### Installation

1. Download the `.xpi` file of [the latest release](https://github.com/jlegewie/zotfile/releases/tag/v5.0.16) 

2. Install ZotFile:

   > `[in 🆉otero] `
   >
   > 1. In the main menu go to Tools > Add-ons
   > 2. Select ‘Extensions’
   > 3. Click on the gear in the top-right corner and choose ‘Install Add-on From File…’
   > 4. Choose .xpi that you’ve just downloaded, click ‘Install’
   > 5. Restart Zotero

#### Setup

### DOI Manager

[ [github](https://github.com/bwiernik/zotero-shortdoi) ]

1. Download the `.xpi` file for the [latest release](https://github.com/bwiernik/zotero-shortdoi/releases/latest).

2. Install Zotero DOI Manager:

   > `[in 🆉otero] `
   >
   > 1. In the main menu go to Tools > Add-ons
   > 2. Select ‘Extensions’
   > 3. Click on the gear in the top-right corner and choose ‘Install Add-on From File…’
   > 4. Choose .xpi that you’ve just downloaded, click ‘Install’
   > 5. Restart Zotero

### BetterBibTex

[[|github](https://github.com/retorquere/zotero-better-bibtex)]

📍BetterBibTex allows Zotero to integrate with a wider variety of programs through “LaTex citation keys” aka “citekeys”.

1.  Download [the latest release](https://github.com/retorquere/zotero-better-bibtex/releases/tag/v5.4.29) (the `.xpi` file)

2. Follow the [installation instructions](https://retorque.re/zotero-better-bibtex/installation/):

   > `[in 🆉otero] `
   >
   > 1. In the main menu go to Tools > Add-ons
   > 2. Select ‘Extensions’
   > 3. Click on the gear in the top-right corner and choose ‘Install Add-on From File…’
   > 4. Choose .xpi that you’ve just downloaded, click ‘Install’
   > 5. Restart Zotero

3. Perform an export. Pick a place that makes sense for it, where it will be easily accessible, such as ~. Unlike the Zotero data directory, you can put this in a cloud.

4. Set up the auto-export:



## Cita



## Pandoc

https://pandoc.org/

## Obsidian

[ [website](https://obsidian.md/) ]

### Necessary plugins

Installing 









