# Snakemake Intro With HiChIP Specifics
Author: Joaquin Reyna

This introduction is not a replacement for the Snakemake ReadTheDocs tutorial, rather,
it’s is my personal quickstart tutorial which should help our HiChIP Database project.
Please refer to the original tutorial to learn many more exciting features that
Snakemake has to offer. 

The whole point of Snakemake is to construct a pipeline whether it includes two pieces
of softwares linked together or many more. Snakemakes uses a special notation to
generalize different commands for easy application and scaling to other samples/files.
Snakemake is very powerful and takes some time to learn but once you have it down it
becomes very simple to organize your code and re-run it in the future. 

Install snakemake and setup directory tree
Follow instructions 1) Installation via Conda/Mamba and 2) Full installation from: 
https://snakemake.readthedocs.io/en/stable/getting_started/installation.html 


The basic structure is framed by a “rule” 

rule extract_significant_loops:
	input:
		‘Loops.bed’
	output:
		‘Loops.sig.bed’
	shell:
		“””
			awk ‘{{if($7 < 0.05) print $0}} {input} > {output}’
“””

snakemake --cores 1 extract_significant_loops 

Recommended structure

<make the directory tree and snapshot>


Full Tutorial can be found here: 
https://snakemake.readthedocs.io/en/stable/tutorial/tutorial.html 


Specifics for HiChIP-DB Project


