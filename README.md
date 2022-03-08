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

The most important concept of Snakemake is the rule which defines the inputs, outputs, parameters, logs, resources, etc. Below is an example rule where we are taking a dummy BEDPE file with chrA, startA, endA, chrB, startB, endB, and p-values and extract those interactions with a significant p-value (p-value < 0.05). You can go ahead and copy and paste this rule into your Snakemake file(s) (within `workflow/Snakefile`).
```
rule extract_significant_loops:
	input:
		'results/loops.bed'
	output:
		'results/loops.sig.bed'
	log:
		'results/loops.log'
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

##  Recommended Directory Tree
Johann Koster, the creator of Snakemake, highly recommends using a particular workflow template and I'll go over that here (you can find out more by visiting the [link](https://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html#distribution-and-reproducibility)). It might be tricky to use at first but this structure facilitates the sharing of your pipeline with others. I would also like to note that I've added a few extra directories that are compatible with Johann's recommendation. So, there are many files + folders but the main idea is to write your rules under `Snakefile` and `workflow/rules/`, scripts under the `workflow/scripts/`, setup the configuation file with softwares we need (`config/config.yaml`) and lastly set up a profile file to run rules with qsub.

<img src="https://user-images.githubusercontent.com/14253227/157336834-59042b4f-e361-4b3f-b7a7-a124f05c980b.png" alt="drawing" width="200"/>

- config - directory to store configuration and samplesheets
	- config.yaml - configuration path which is recommended for Snakemake 
- docs - directory to store different manual types of documents which are not produced/used by Snakemake directly
- README.md - information about this tutorial
- results - directory for all results which are output
	- main - directory with data analyzed per sample
	- refs - directory with reference files and their derivatives
	- tests - directory for output of test rules/other
	- ... (of course you might run into scenarios where you need to add directories, do so with discretion to keep the code as tidy as possible)
- workflow - directory of all codes
	- envs - directory with environment files 
	- notebooks - directory with jupyter notebooks 
	- profiles - directory with different profiles (commandline settings)
	- reports - directory which lists how to produce Snakemake reports
	- scripts - directory to store all scripts needed in your rules
	- Snakefile - main Snakemake file

## Specifics for HiChIP-DB Project
My idea is to start the Snakemake Rules from HiCPro output. The reason for this is that HiCPro is also a workflow type of pipeline but uses Make instead of Snakemake and it's going to be messy to combine the two especially when you are first learning Snakemake. Once you have run HiC-Pro you can use those files within different Snakemake rules. As you'll see after cloning this repository you will have a particular directory tree structure that matches the one I mentioned above.

One last thing I want to mention is where and how to store the results. There are lots of ways of going about this but since this is a group effort I think it's best to be on the same page. The most important part is to use the directory structure described above and, when generating **results**, save the results using the naming scheme that Ferhat suggested. For example, you'll be running peak calling followed by loop calling so it might look like this:

<img src="https://user-images.githubusercontent.com/14253227/157333623-4d7e6a2e-2357-4343-b4e2-288169e42de9.png" alt="drawing" width="300"/>

Right now you can see the different analyses done for the heart sample and depending on the different analyses you'll use some particular naming scheme. This example is very basic and needs to fit Ferhat's suggestions but you'll be doing something like this. We can talk more about it if you want and write some documentation here that describes our naming conventions more. 

Comment: I just want to say that I think analyzing each sample independently is very important because bioinformatics programming errors can come as much from the coding side of things as well as the data side of things. By analyzing each sample in it's own folder you can better track and debug what step failedd for a particular sample. 

Tip: you can used `mkdir -P <dir1>/<dir2>/<dir3>/.../<dirN>` to create a whole directory branch. 

## Other Resources 
- Understanding Snakemake - https://vincebuffalo.com/blog/2020/03/04/understanding-snakemake.html
- More Resources (from ReadTheDocs) - https://snakemake.readthedocs.io/en/stable/project_info/more_resources.html




