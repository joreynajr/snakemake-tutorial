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

Note: For the full tutorial visit https://snakemake.readthedocs.io/en/stable/tutorial/tutorial.html

## Install Snakemake
Follow instructions 1) Installation via Conda/Mamba and 2) Full installation from: 
https://snakemake.readthedocs.io/en/stable/getting_started/installation.html 

## Rules are the Basic Building Blocks

The most important concept of Snakemake is the rule which defines the inputs, outputs, parameters, logs, resources, etc. Below is an example rule where we are taking a dummy BEDPE file with chrA, startA, endA, chrB, startB, endB, and p-values and extract those interactions with a significant p-value (p-value < 0.05). You can go ahead and copy and paste this rule into your Snakemake file(s).
```
rule extract_significant_loops:
	input:
		'results/loops.bed'
	output:
		'results/loops.sig.bed'
	log:
		'results/loops.{sample}.log'
	shell:
		"""
			awk '{{if($7 < 0.05) print $0}}' {input} > {output} 2> {log}
		"""
```

We can run this basic command by invoking Snakemake as below:

```
snakemake --cores 1 extract_significant_loops 
```

## Wildcards Generalize Your Rules
I made another example dataset for the loop files and a corresponding rule to try out. You can go ahead and copy and paste this rule into your Snakemake file(s).
```
rule extract_significant_loops_per_sample:
	input:
		'results/loops.{sample}.bed'
	output:
		'results/loops.{sample}.sig.bed'
	log:
		'results/loops.{sample}.log'
	shell:
		"""
			awk '{{if($7 < 0.05) print $0}}' {input} > {output} 2> {log}
		"""
```

We can run this rule for the heart sample using:

```
snakemake --cores 1 results/loops.heart.sig.bed
```

You can repeat this Snakemake invocation using the lung sample:
```
snakemake --cores 1 results/loops.lung.sig.bed
```

## Specifics for HiChIP-DB Project
My idea is to start the Snakemake Rules from HiCPro output. The reason for this is that HiCPro is also a workflow type of programs but uses Make instead of Snakemake and it's going to be messy to combine the two especially when you are first learning Snakemake. Once you have run HiC-Pro you can use those files within different Snakemake rules. As you'll see after `git clone` of this repository you will have a particular directory tree structure. It might be tricky to use at first but this structure, althought not required, is very recommended by the creator of Snakemake [link](https://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html#distribution-and-reproducibility). There are many files + folders but the main idea is to write your rules under `Snakefile` and `workflow/rules/`, scripts under the `workflow/scripts/`, setup the configuation file with softwares we need (`config/config.yaml`) and lastly set up a profile file to run rules with qsub. In the next section you'll see what directory structure you need and you can go ahead and build it.

Tip: you can used `mkdir -P <dir1>/<dir2>/<dir3>/.../<dirN>` to create a whole directory branch. 

##  Recommended Directory Tree
<img src="https://user-images.githubusercontent.com/14253227/157296331-d92bda4a-0215-4dc3-b3a4-742707bf176d.png" alt="drawing" width="200"/>

- config - directory to store configuration and samplesheets
	- config.yaml - configuration path which is recommended for Snakemake 
- docs - directory to store different manual types of documents which are not produced/used by Snakemake directly
- README.md - information about this tutorial
- results - directory for all results which are output
- workflow - directory of all codes
	- envs - directory with environment files 
	- notebooks - directory with jupyter notebooks 
	- profiles - directory with different profiles (commandline settings)
	- reports - directory which lists how to produce Snakemake reports
	- scripts - directory to store all scripts needed in your rules
	- Snakefile - main Snakemake file

## Other Resources 
- Understanding Snakemake - https://vincebuffalo.com/blog/2020/03/04/understanding-snakemake.html
- More Resources (from ReadTheDocs) - https://snakemake.readthedocs.io/en/stable/project_info/more_resources.html




