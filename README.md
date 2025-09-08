# Snakemake
Studies of snakemake


Plano de Estudos para Snakemake
### 1. Fundamentos
🎯 Objetivo: entender a filosofia do Snakemake.

O que é workflow / pipeline.

Por que usar Snakemake em bioinformática.

Conceito de regra (rule), input/output, wildcards, dependências automáticas.

Como rodar: snakemake --cores N.

📌 Exercício:

Escrever seu primeiro Snakefile com uma regra simples que transforma um .txt em outro .txt com cat.


### 2. Estrutura de um workflow

🎯 Objetivo: organizar projetos reprodutíveis.

Estrutura de pastas (data/, results/, scripts/, Snakefile).

Regra all como alvo final.

Como Snakemake resolve dependências sozinho.

📌 Exercício:

Fazer um mini-workflow:

Copiar arquivo bruto (data/) →

Processar (scripts/process.py) →

Gerar resultado final (results/).


### 3. Wildcards e expand

🎯 Objetivo: evitar repetição manual.

Uso de {sample}.

expand() para rodar em várias amostras.

Configuração com arquivo config.yaml.

📌 Exercício:

Criar workflow que roda em 3 datasets diferentes automaticamente.

### 4. Integração com scripts externos

🎯 Objetivo: chamar R/Python do Snakefile.

shell: para rodar comandos.

script: para executar um script .R ou .py.

params: para passar parâmetros.

📌 Exercício:

Escrever um scripts/myscript.R que faz uma estatística simples em um .csv e chamar no Snakefile.

### 5. Recursos computacionais

🎯 Objetivo: aprender a otimizar jobs.

threads: (núcleos de CPU).

resources: (memória, tempo).

Como Snakemake respeita --cores.

📌 Exercício:

Criar regra com threads: 4 e rodar com diferentes --cores para ver o impacto.

### 6. Visualização e Debug

🎯 Objetivo: diagnosticar e documentar.

snakemake --dag | dot -Tpdf > dag.pdf (grafo do workflow).

snakemake -np (dry-run, simular sem rodar).

Logs automáticos.

📌 Exercício:

Gerar um gráfico do seu workflow e abrir no VS Code.

### 7. Casos bioinformáticos reais

🎯 Objetivo: aplicar em pipelines típicos.

Workflow de RNA-seq (FastQC → Trim → Alinhamento → Contagem).

Workflow de Metagenômica (QC → Assembly → Binning → Anotação).

📌 Exercício:

Reproduzir um workflow já pronto do GitHub (ex.: nf-core, snakemake-workflows).

### 8. Escalando para clusters/nuvem

🎯 Objetivo: preparar para HPC.

Perfil de execução (profiles/).

--cluster, SLURM/PBS.

Containers e conda.

📌 Exercício:

Criar um workflow que roda com conda (conda: dentro das regras).

### 9. Boas práticas e versionamento

🎯 Objetivo: deixar tudo organizado no GitHub.

Documentação no README.md.

Organização em pastas (data/, results/, scripts/).

Versionar Snakefile + config.yaml + environment.yaml.

📌 Exercício:

Criar um repositório no GitHub com seu primeiro workflow documentado.

### 10. Projeto final

🎯 Objetivo: consolidar.

Montar um workflow real (exemplo: QC de RNA-seq).

Documentar tudo no GitHub (README, DAG, exemplos).

Testar reprodutibilidade (rodar do zero em outra máquina).
3 → Tópicos 7 e 8 (bioinfo real + cluster).

Semana 4 → Tópicos 9 e 10 (boas práticas + projeto final).
