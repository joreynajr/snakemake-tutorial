# Snakemake Intro With HiChIP Specifics
**Author:** Joaquin Reyna **Date:** 2022.03.08

This introduction is not a replacement for the Snakemake ReadTheDocs tutorial, rather,
it’s is my personal quickstart tutorial which should help our HiChIP Database project.
Please refer to the original tutorial to learn many more exciting features that
Snakemake has to offer. The whole point of Snakemake is to construct a pipeline 
whether it includes two pieces of softwares linked together or many more. Snakemakes
uses a special notation to generalize different commands for easy application and scaling
to other samples/files. Snakemake is very powerful and takes some time to learn but once
you have it down it becomes very simple to organize your code and re-run it in the future. 

Note: For the full tutorial visit: https://snakemake.readthedocs.io/en/stable/tutorial/tutorial.html

## Install Snakemake
Follow instructions 1) Installation via Conda/Mamba and 2) Full installation from: 
https://snakemake.readthedocs.io/en/stable/getting_started/installation.html 

## Rules are the Basic Building Blocks of Snakemake

The most important concept of Snakemake is the rule which defines the inputs, outputs, parameters, logs, resources, etc. Below is an example rule where we are taking a dummy BEDPE file with chrA, startA, endA, chrB, startB, endB, and p-values and extract those interactions with a significant p-value (p-value < 0.05). You can go ahead and copy and paste this rule into your Snakemake files. 
```
rule extract_significant_loops:
	input:
		'loops.bed'
	output:
		'loops.sig.bed'
	shell:
		"""
			awk ‘{{if($7 < 0.05) print $0}} {input} > {output}’
		"""
```

We can run this basic command by invoking snakemake as below:

```
snakemake --cores 1 extract_significant_loops 
```

##  Recommended Directory Tree
<img src="https://user-images.githubusercontent.com/14253227/157296331-d92bda4a-0215-4dc3-b3a4-742707bf176d.png" alt="drawing" width="200"/>

config - directory to store configuration and samplesheets
	- config.yaml - configuration path which is recommended for Snakemake 
docs - directory to store different manual types of documents which are not produced/used by Snakemake directly
README.md - information about this tutorial
results - directory for all results which are output
workflow - directory of all codes
	- envs - directory with environment files 
	- notebooks - directrory with jupyter notebooks 
	- profiles - directory with different profiles (commandline settings)
	- reports - directory which lists how to produce Snakemake reports
	- scripts - directory to store all scripts needed in your rules
	- Snakefile - main Snakemake file

## Specifics for HiChIP-DB Project
My idea is to start the Snakemake Rules from HiCPro output. The reason for this is that HiCPro is also a workflow type of programs but uses Make instead of Snakemake and it's going to be messy to combine the two especially when you are first learning Snakemake. Once you have run HiC-Pro you can use those files within different Snakemake rules. 

## Other Resources 
- Understanding Snakemake - https://vincebuffalo.com/blog/2020/03/04/understanding-snakemake.html
- More Resources (from ReadTheDocs) - https://snakemake.readthedocs.io/en/stable/project_info/more_resources.html




