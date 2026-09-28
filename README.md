# Generate MLA Uploads Plugin

This repository is a Claude Code plugin that contains an [Agent Skill](https://agentskills.io) which can be used generate the necessary upload files for a Major Locus Allele genotyping project.

> [!WARNING]
> As with any AI-generated output, the generated upload templates should be thoroughly checked for mistakes and errors before adding the data to the database.

## Installation

The exact installation will depend on the AI Agent you are using.  These instructions are for installing the plugin using [Claude Code](https://code.claude.com/docs/en/discover-plugins):

```
claude plugin marketplace add TriticeaeToolbox/t3-marketplace
claude plugin install generate-mla-uploads@t3-marketplace
```

## Usage

1) Create a directory containing the raw breeder data you want to convert ( `~/data/` )
2) Change to the directory ( `cd ~/data/` )
3) Start claude code in that directory ( `claude` )
4) Use the `/generate-mla-uploads` command to run the skill.

## Output

After running the `/generate-mla-uploads` command and following any prompts that the AI agent may ask, you should have an `upload_templates/` directory that contains:

- a parsed VCF file that contains the Major Loci as markers and the allele values for the samples in the data
- an accession upload template containing the samples as accessions