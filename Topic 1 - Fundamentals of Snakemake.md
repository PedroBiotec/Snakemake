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
- Project Structure:
  ```
  snakemake_tutorial
      data/
          sample1.txt
          sample2.txt
      results/
      Snakefile
  ```
- `data/sample1.txt`:
```
apple
banana
orange
```
- `data/sample2.txt`:
```
cat
dog
mouse
```
- Snakefile:
```
rule all:
    input:
        "results/sample1.processed.txt"
        "results/sample2.processed.txt"
rule preprocess:
    input:
        "data/{sample}.txt"
    output:
        "data/{sample}.processed.txt"
    run:
        with open(input[0], "r") as input_file, open(output[0], "w") as output_file:
            for line in input_file:
                output_file.write(line.upper())
```
- `run:` lets you write python code directly in the Snakefile.
- `input[0]` = the first (and only) input file → "data/sample1.txt" or "data/sample2.txt".
- `output[0]` = the first output file → "results/sample1.processed.txt" or "results/sample2.processed.txt".
- `open(input[0], "r")` → open the input file for reading (`"r"` = read mode).
- `open(output[0], "w")` → open the output file for writing (`"w"` = write mode, overwrites if file exists).
- `with ... as ...:` ensures the files are automatically closed after the block finishes.

  ### The results:
  - For running it, in terminal of VS Code, enter in the environment where the snakemake is installed and write:
```
snakemake --cores 2
```
Snakemake will:
1. Read Snakefile.
2. Notice that rule all requires two outputs.
3. Run rule preprocess twice (once for sample1, once for sample2).
4. Generate:
- `results/sample1.processed.txt`
- `results/sample2.processed.txt`

Now, in folder results, you must find the output files with all text in upercase
