---
name: generate-mla-uploads
description: Generate a Major Locus Alleles genotyping project VCF to upload to a breedbase instance
allowed-tools: Bash(mkdir:*) Bash(python:*) Bash(python3:*) Read
---

## Overview

This skill is used to generate a VCF file to upload a new Major Locus Alleles genotyping project to a breedbase instance.  The generated VCF file should follow the standard VCF file conventions.  The marker names will be the names of the major loci.  But instead of nucleotide sequences for the ref and alt alleles, the alleles will be the functional allele names for each marker.

## Assets

The `assets/` directory contains reference information about the Major Loci and their function alleles as well as an example .vcf file that is properly formatted.

- `MLA Specification - August 2026.xlsx` - contains the most recent set of major loci and alleles.  The workbook is divided into two worksheets:
	- The `Genes` worksheet contains information about the major loci.  Each major locus will be added as a marker in the Major Locus Alleles genotyping protocol.  The `gene` column includes the primary marker name (this is the name that should be used in the generated output). The `gene_synonym` column includes additional names for the gene that may be seen in breeder data.  The marker chromosome positions are arbitrarily added to the `ML` chromosome at the position specified in the `position` column.  The `description`, `categories`, and `references` columns include additional metadata about the markers (these are not added to the .vcf file).
	- The `Alleles` worksheet contains information about the allele values for each major locus / marker / gene.  Use the `allele` column as the functional allele name (this is used as the allele value in the .vcf file).  The `allele_synonym` columns include additional names for an allele that may be found in breeder data.  The `index` column indicates whether the allele is a reference allele (index = 0) or an alternate allele (index >= 1).  Each major locus / marker / gene must have one and only one reference allele set (if a marker is missing a reference allele or has more than one set warn the user of this problem).
- The `example-input.csv` contains example data from the USDA-Eastern genotyping lab.  The file contains the raw breeder data as a matrix of functional allele values for a set of samples.  The `Sample_Name` column includes the accession names for each sample.  The column headers are the major locus / marker / gene names.  The cell values are the functional allele names for each pair of genes and samples.
- The `example-output.vcf` contains a generated .vcf file from the data in the `example-input.csv` file.

## Instructions

The purpose of this skill is to generate a .vcf file for a set of breeder marker data.  The VCF file should have the following initial columns:
	- `#CHROM` - the marker's chromosome name, set to 'ML' for all markers
	- `POS` - the marker's chromosome position, read from the `position` column in the MLA specification
	- `ID` - the standardized marker name, matched to a value in the `gene` column in the MLA specification
	- `REF` - the name of the reference allele, matched from the `allele` column in the MLA specification where the index = 0
	- `ALT` - a comma separated list of all alternate alleles, matched from the `allele` column in the MLA specification where the index >= 1 in order of the index calue
	- `QUAL` - can be set to '.'
	- `FILTER` - can be set to 'PASS'
	- `INFO` - can be set to '.'
	- `FORMAT` - can be set to 'GT'

All additional columns in the .vcf file are the sample / accession names.

The value for each sample / marker pair should be the allele index values in the form of 'n/n', where n is the index number of the allele.  For example, if the sample is homozygous for the reference allele for a marker, the value will be '0/0'.  If the sample is homozygous for the first alternate allele for a marker, the value will be '1/1'.  If the sample is homozygous for the second alternate allele for a marker, the value will be '2/2', etc.  If the sample is heterozygous, use a combination of the allele indices (for example, '0/1' or '1/2').  If there is no data for a marker / sample pair, use './.'  as the value.

It is your job to match the marker / gene and allele names in the breeder data to the standardized marker / gene and allele names in the MLA specification.  Only use the standardized marker / gene and allele names from the MLA specification in the output .vcf file.  If you are not sure of a match, ask the user to verify if it is correct.  Before generating the .vcf file, print a table of all of the matched gene and allele names for the user to approve.  Once approved, you can generate the final .vcf file.

## Output

The final output should include two directories:

- `./bin/` - should include any code or scripts that are used to generate the .vcf file.  Always generate a `run.sh` script that can be used to regenerate the .vcf file.
- `./output/` - should contain any generated output files.  Always generate a `parsed.vcf` file that contains the generated VCF data.  Also include tables in a markdown file that show all of the matched genes and alleles from the original source data to the gene and allele names in the MLA specification.