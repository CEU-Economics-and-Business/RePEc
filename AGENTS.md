# AGENTS.md

## Overview

This repository contains a RePEc (Research Papers in Economics) archive for the CEU Department of Economics working paper series. RePEc is a collaborative effort to enhance the dissemination of research in economics. This archive makes CEU working papers discoverable through RePEc services including IDEAS, EconPapers, and other bibliographic databases.

## Repository Structure

```
RePEc/
├── .github/
│   └── workflows/
│       └── index.yaml          # GitHub Action to auto-generate index.html files
├── metadata/
│   ├── ceuarch.rdf            # Archive-level metadata
│   ├── ceuseri.rdf            # Series-level metadata
│   ├── econwp/                # Working paper templates directory
│   │   ├── YYYY_N.rdf         # Individual paper metadata files
│   │   └── index.html         # Auto-generated directory index
│   └── index.html             # Auto-generated directory index
├── pdf/
│   ├── YYYY_N.pdf             # Working paper PDFs
│   └── index.html             # Auto-generated directory index
└── index.html                 # Root index
```

## RePEc Archive Components

### 1. Archive Metadata (ceuarch.rdf)

Location: `metadata/ceuarch.rdf`

Defines the top-level RePEc archive using the ReDIF-Archive 1.0 template. Contains:
- **Handle**: `RePEc:ceu` - Unique RePEc identifier for the archive
- **Name**: Descriptive name of the archive
- **Maintainer-Email**: Contact email for the archive maintainer
- **Description**: Brief description of the archive content
- **URL**: Base URL where metadata files are hosted

This file must be present at the root of the metadata directory and should rarely need updating unless contact information or URLs change.

### 2. Series Metadata (ceuseri.rdf)

Location: `metadata/ceuseri.rdf`

Defines the working paper series using the ReDIF-Series 1.0 template. Contains:
- **Handle**: `RePEc:ceu:econwp` - Unique series identifier
- **Name**: Series name (CEU Working Papers)
- **Provider-Name**: Publishing institution
- **Provider-Homepage**: Institution website
- **Provider-Institution**: RePEc handle for the institution
- **Maintainer-Name**: Person responsible for the series
- **Maintainer-Email**: Contact email
- **Type**: Template type for items in series (ReDIF-Paper)

This file defines the working paper series under which all individual papers are organized.

### 3. Paper Metadata (econwp/*.rdf)

Location: `metadata/econwp/YYYY_N.rdf`

Each file contains ReDIF-Paper 1.0 templates describing individual working papers. The naming convention is `YYYY_N.rdf` where:
- `YYYY` is the publication year
- `N` is the sequential paper number within that year

#### Required Fields

- **Template-Type**: Must be "ReDIF-Paper 1.0"
- **Author-Name**: Full author name (repeatable for multiple authors)
- **Title**: Paper title
- **Handle**: Unique identifier in format `RePEc:ceu:econwp:YYYY_N`

#### Strongly Recommended Fields

- **Author-Name-First**: Author's first name
- **Author-Name-Last**: Author's last name
- **Abstract**: Paper abstract (enhances discoverability)
- **Creation-Date**: Date paper was written (format: YYYY-MM-DD, YYYY-MM, or YYYY)
- **Number**: Paper number matching the handle (YYYY_N)

#### Optional but Useful Fields

- **Author-Email**: Author contact email
- **Author-Workplace-Name**: Author's institutional affiliation
- **Revision-Date**: Date of last revision
- **File-URL**: URL to the PDF file (repeatable for multiple versions)
- **File-Format**: MIME type (application/pdf)
- **File-Function**: Description of file version
- **Classification-JEL**: JEL classification codes (comma-separated)
- **Keywords**: Keywords for the paper
- **Length**: Number of pages
- **Publication-Status**: If published, format as "Published in [Journal], [Date], pages [X-Y]"
- **DOI**: Digital Object Identifier if available

#### Author Clusters

Each author should have a separate cluster starting with `Author-Name:`. Example:

```
Author-Name: Miklós Koren
Author-Name-First: Miklós
Author-Name-Last: Koren
Author-Email: example@ceu.edu
Author-Workplace-Name: Department of Economics, Central European University
```

#### File Clusters

Papers can have multiple file versions. Each version uses a repeatable File-* cluster:

```
File-URL: https://ceu-economics-and-business.github.io/RePEc/pdf/2024_1.pdf
File-Format: application/pdf
File-Function: Full text

File-URL: https://ceu-economics-and-business.github.io/RePEc/pdf/2024_1_revised.pdf
File-Format: application/pdf
File-Function: Revised version
```

## Workflow

### Adding a New Working Paper

1. **Prepare the PDF**: Add the PDF file to the `pdf/` directory with filename `YYYY_N.pdf`

2. **Create RDF metadata**: Create a new file `metadata/econwp/YYYY_N.rdf` with the paper metadata following the template structure

3. **Commit and push**: The GitHub Action will automatically generate index.html files for all directories

4. **Wait for indexing**: RePEc services automatically crawl the archive periodically (typically daily) to integrate new papers

### Updating an Existing Paper

1. **For minor corrections**: Edit the existing RDF file with updated metadata

2. **For revised versions**: 
   - Add the new PDF to `pdf/` (e.g., `YYYY_N_revised.pdf`)
   - Add a new File-* cluster to the RDF file
   - Update the Revision-Date field

3. **Commit and push**: Changes will be picked up during the next RePEc crawl

### Removing a Paper

To withdraw a paper from RePEc while maintaining the record:
- Keep the RDF file but remove all `File-*` clusters
- Do NOT delete the entire RDF file as this preserves the paper's bibliographic record

## GitHub Actions

### index.yaml Workflow

This workflow automatically generates `index.html` files for all directories whenever changes are pushed. It:

1. Finds all directories in the repository
2. For each directory, generates an HTML file listing all contents
3. Links to subdirectories via their index.html files
4. Links directly to files
5. Commits and pushes the generated index files

This ensures the archive remains browsable via web browser.

## RePEc Services Integration

Once the archive is set up and registered with RePEc:

- **Automatic crawling**: RePEc services visit the archive regularly to check for updates
- **Indexing**: Papers are indexed in IDEAS, EconPapers, and other RePEc services
- **Discovery**: Papers become searchable and citeable through RePEc databases
- **Author profiles**: Authors are automatically linked to their papers in RePEc Author Service
- **Citation tracking**: Papers are tracked for citations via CitEc

## Best Practices

1. **Consistent naming**: Always use the `YYYY_N` format for paper numbers and filenames

2. **Complete metadata**: Include all recommended fields, especially abstracts and JEL codes, to maximize discoverability

3. **Author information**: Provide complete author information including first/last names to ensure proper author identification in RePEc services

4. **UTF-8 encoding**: Use UTF-8 encoding for RDF files to properly handle special characters and diacritics

5. **Date formats**: Use ISO date formats (YYYY-MM-DD preferred, YYYY-MM or YYYY acceptable)

6. **File URLs**: Ensure all File-URL entries point to stable, accessible URLs

7. **Version control**: Commit RDF and PDF files together to maintain synchronization

8. **Testing**: Validate RDF syntax using the [RePEc ReDIF validator](https://ideas.repec.org/cgi-bin/validate.cgi) before committing

## References

- [RePEc Homepage](http://repec.org/)
- [RePEc Step-by-Step Tutorial](https://ideas.repec.org/stepbystep.html)
- [Working Paper ReDIF Template](https://ideas.repec.org/t/papertemplate.html)
- [ReDIF Documentation](http://ideas.repec.org/p/rpc/rdfdoc/redif.html)
- [RePEc Archive Validator](https://ideas.repec.org/cgi-bin/validate.cgi)

## Maintainer Information

- **Archive Handle**: RePEc:ceu
- **Series Handle**: RePEc:ceu:econwp
- **Maintainer**: Anita Apor (AporA@ceu.edu)
- **Base URL**: https://ceu-economics-and-business.github.io/RePEc/

## Technical Notes

### ReDIF Format

ReDIF (Research Documents Information Format) is a simple line-oriented format where:
- Each field is on a new line in format `Field-Name: value`
- Multi-line values continue on subsequent lines (indentation is preserved)
- Templates are separated by blank lines
- Field names are case-sensitive
- The format is plain ASCII/UTF-8 text

### Handle Structure

RePEc handles follow a hierarchical structure:
- Archive: `RePEc:ceu`
- Series: `RePEc:ceu:econwp`
- Paper: `RePEc:ceu:econwp:2024_1`

This ensures global uniqueness of all items in the RePEc database.
