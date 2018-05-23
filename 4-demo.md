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

## Navigating OpenRefine

- Creating a project
  - check character encoding, options
  - Refine never over writes your original data, it creates a copy!
  - information is *not* sent over internet

- Manipulating columns
  - renaming and removing columns
  - changing column order
  - collapsing columns

- Exporting a project or a data set
  - OpenRefine project
  - many formats!
  - templating
  - export is always a new copy of data, never alters original!

- Automating tasks
  - Undo/Redo copy `Extract` to txt file (use text editor, not Word)
  - create new project with original file
  - Undo/Redo paste saved extract into `Apply`

## Exploring and Cleaning Data

- Cleaning the simple stuff
  - a lot of options in the drop-down menus
  - get rid of white space
    - one column at a time: `Edit cells` > `Common transforms`
    - entire sheet: `All` > `Transform` > value.trim() and value.replace(/\s+/,' ')
  - `Edit cells` > `Transform...` is very powerful
  - GREL = General Refine Expression Language
  - GREL [documentation](https://github.com/OpenRefine/OpenRefine/wiki/General-Refine-Expression-Language) and [recipes](https://github.com/OpenRefine/OpenRefine/wiki/Recipes) are available on the OpenRefine wiki.

**Sample GREL Recipes**

 - Remove duplicate comma separated entries in a cell
   - value.split(", ").uniques().join(", ")
 - Replace string in cells
   - value.replace("+", "")
   - value.replace("~"", "").replace(",", "").replace("-", "")
 - Clean-up character encoding problems
   - value.unescape("url")
 - Convert number with text to number
   - toNumber(value.replace(" million", ""))*1000000

- Splitting, faceting, and clustering
  - multi-valued fields can be a barrier to data cleaning
    - `Edit cells` > `Split multi-valued cells...`
    - record view vs. row view
    - `Facet` > `Text facet`
    - manual cleaning and clustering
    - `Edit cells` > `Join multi-valued cells`

## Enhancing with Data from Other Sources

- Reconciling from other data sources
  - Vocabulary reconciliation is a process where automated systems use terms from unstandardized metadata to search controlled vocabularies and return URIs.
  - OpenRefine has built in tools to reconcile data with [Wikidata](https://www.wikidata.org/)
  - Other data services can be added

- OpenRefine's Wikidata Service
  - Reconciling the names
    - `Reconcile` > `Start reconciling...`
    - choose `Wikidata Reconciliation for OpenRefine (en)`
    - choose `human` and `Auto-match candidates with high confidence`
    - matches some automatically, but often requires some manual review
  - OpenRefine 2.8 added querying and extracting tools
    - Select `matched` from the judgement facet
    - `Edit column` > `Add columns from reconciled values`
    - Add country of citizenship, occupation, place of birth, place of death, place of burial, and VIAF ID
    - `Add Property` filter to include date of birth and date of death
  - Review the results of your reconciliation
    - date of birth > `Facet` > `Timeline facet`
  - Extract the Wikidata id
    - `Edit column` > `Add column based on this column...`
    - name column: wikidata_id
    - cell.recon.match.id

- Adding more data based on extracted dataset
  - Geographic Coordinates for places
    - place of birth > `Edit column` > `Add columns from reconciled values`
    - Add coordinate location

- Compare the collected and extracted VIAF ids
  - Clean up collected VIAF ids
    - `Facet` > `Customized facets` > `Facet by blank`
    - On rows view, choose `false` to select rows with viaf ids
    - `Filter` - regex: ^(?!http://).+
    - Transform cells to remove final "/" temporarily - value.replace(/\/$/, "")
    - Transform cells to apply final "/" because that is what VIAF expects - value+"/"
  - Make extracted VIAF id numbers into VIAF URLs
    - `Edit column` > `Add column based on this column...`
    - name column: viaf_url
    - "http://http://viaf.org/viaf/" + value + "/"
  - Build a facet to compare the two columns
    - value == cells["viaf id"].value

- Other data services:
  - Users can set up their own data services or use other existing data services.
  - Some sample services are available at the [OpenRefine Wiki - Reconcilable Data Sources](https://github.com/OpenRefine/OpenRefine/wiki/Reconcilable-Data-Sources) and at [http://refine.codefork.com/](http://refine.codefork.com/).
  - Reconciliation can be taxing on host servers and data sources. Documentation for hosting your own service are available at and on the [conciliator GitHub](https://github.com/codeforkjeff/conciliator).

## Exporting your cleaned and expanded dataset

- Export project
- TSV, CSV, HTML table, Excel (two flavors), ODF spreadsheet
- Templating - Create XML exports!
