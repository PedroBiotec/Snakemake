# Topic 1 - Fundamentals of Snakemake
## 1.1 What is Snakemake?

Snakemake is a **workflow management system** designed for bioinformatics and scientific data analysis.
It helps you **automate**, document, and reproduce complex pipelines that involve multiple steps and dependencies.

Instead of manually running commands, you describe the rules in a Snakefile, and Snakemake:
- Figures out which steps are required to build your final output.
- Runs them in the right order, without repetition.
- Parallelizes tasks when possible.

## 1.2 Core concepts:
### 🔹Rule:
A rule describes how to transform input files into output files.
It is the **building block** of Snakemake workflows.

```python
rule example:
    input:
        "data/input.txt"
    output:
        "results/output.txt"
    shell:
        "cp {input} {output}"
```

This means:
- **Input**: data/input.txt
- **Output**: results/output.txt
- **Command**: copy input to output
  
**Snakemake will only run this rule if the output file is missing or outdated.**
  
  
### 🔹Input and Output:
- **Input files:** dependencies needed for the rule to run.
- **Output files:** results that the rule produces.

Snakemake uses these to build a dependency graph (called a DAG = Directed Acyclic Graph).

### 🔹Wildcards:
Wildcards `({name})` allow rules to be **generalized**.
Instead of writing separate rules for each sample, you can write one rule that adapts.

Example:
```python
rule trim:
    input:
        "raw/{sample}.fastq"
    output:
        "trimmed/{sample}.fastq"
    shell:
        "trimmomatic SE {input} {output} SLIDINGWINDOW:4:20"
```

- {sample} is a placeholder.
- Snakemake replaces it with actual sample names (like sample1, sample2) when running.

  ### 🔹The "all" rule
Usually, workflows define a special target rule called `all`.  
It specifies the **final outputs you want**. Snakemake will automatically figure out everything needed to reach those outputs.

Example:
```python
rule all:
    input:
        "results/sample1.final.txt",
        "results/sample2.final.txt"
```
Now when you type:
```bash
snakemake --cores 2
```
- Snakemake will generate all listed outputs, building the required steps.

🔹 Execution

To run Snakemake, you open a terminal inside your project folder and run:
```bash
snakemake --cores N
```

`--cores N`: number of CPU cores Snakemake can use in parallel.
- Snakemake automatically decides which jobs to run simultaneously, based on dependencies.

### 1.3 hands-on Example
