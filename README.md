# Tutorial Automation

The goal of this project is to use Jinja2 templates to convert from:
- A tutorial in markdown in separate files per step with sidecar JSON files for metadata
or
- A tutorial written in Google Docs exported to markdown with `claat`

converted to
- A single markdown file with metadata in the frontmatter and step tags in the body

## Work In Progress

[Asana project with current wip](https://app.asana.com/share/cisco/tutorial-automation/5557457880942/c2c04dcdc734c9cde69c68d785e6b8a5)

current task:
expand J2 template to be modular and feed in from multiple markdown / string inputs rendering steps automatically
