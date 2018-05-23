---
layout: default
title: 4-Demo
nav: true
---

# Demo: Digital Mitford Data

In this demo we are going to play with a data set extracted from [Digital Mitford](http://digitalmitford.org/). The original data was a [TEI](http://www.tei-c.org/index.xml) encoded index of names used on the site. The <listPerson sortKey="histPersons"> node was extracted into a TSV file for simplicity.

Download <a href="images/histPerson_data.tsv" target="_blank">`histperson.tsv`</a>

The data fields are
- xml id
- gender
- name
- occupation
- viaf id

- Create project
    - check character encoding, options
    - Refine never over writes your original data, it creates a copy!
    - information is *not* sent over internet

- Column basics
    - text filter
    - faceting
    - edit cells / facets
    - transform, `value.unescape('url')`, find & replace, `value.replace("old","new")`
    - undo/redo
    - clustering
	- split into multiple columns
    - combine columns (*tricky* because combining blank cells results in an error)
        - facet by blank, combine only non-empty cells
        - transform, `value + " " + cells["col 2"].value`
    - remove / reorder columns

- Export basics
	- OpenRefine project
    - many formats!
    - templating
	- export is always a new copy of data, never alters original!

- Automate basics
	- Undo/Redo copy `Extract` to txt file (use text editor, not Word)
	- create new project with original file
	- Undo/Redo paste saved extract into `Apply`

- More!
	- Star / Flag & remove rows
	- create new column with transform `length(value)`, numeric facet
	- deduplicate
		- sort by, permanent reorder
		- blank down / fill down
    - fetch URLs
        - basic geo code lookup, `"http://maps.google.com/maps/api/geocode/json?sensor=false&address=" + escape(value, "url")
with(value.parseJson().results[0].geometry.location, pair, pair.lat +", " + pair.lng)`
