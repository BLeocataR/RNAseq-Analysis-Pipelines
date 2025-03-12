# 📌 Pipeline de Análise de Fusões em RNAseq

Este repositório contém um pipeline completo para a análise de fusões gênicas em dados de RNAseq, com foco em pacientes com **Mieloma Múltiplo**. O pipeline utiliza ferramentas como **STAR**, **RNApeg**, **Cicero** e **Docker** para detectar e refinar fusões gênicas em amostras de RNAseq.

## 🔍 Visão Geral

O pipeline é composto pelas seguintes etapas:

1. 🛠️ **Pré-processamento:**
   - Download de dados SRA e conversão para FASTQ.
   - Controle de qualidade com **FastQC** e **MultiQC**.
   - Remoção de adaptadores e filtragem de baixa qualidade com **Trimmomatic**.

2. 🧬 **Alinhamento:**
   - Alinhamento das reads ao genoma de referência **GRCh38** usando **STAR**.
   - Geração de arquivos **BAM** ordenados e indexados.

3. 🔬 **Detecção de Fusões:**
   - Detecção inicial de fusões gênicas com **RNApeg**.
   - Refinação das fusões detectadas utilizando **Cicero**.

4. 📑 **Pós-processamento:**
   - Consolidação dos resultados em um único diretório para fácil visualização.

## 📦 Requisitos

### 🔧 Dependências
- **SRAToolkit** – para download de dados SRA.
- **FastQC** – para controle de qualidade das reads.
- **MultiQC** – para análise visual do controle de qualidade.
- **Trimmomatic** – para trimming de adaptadores e filtragem de baixa qualidade.
- **STAR** – para alinhamento de RNAseq.
- **Samtools** – para manipulação de arquivos BAM.
- **Docker** – necessário para execução das ferramentas **RNApeg** e **Cicero**.

### 🗄️ Bancos de Dados
- **Genoma de referência**: GRCh38 (baixado automaticamente pelo script).
- **Arquivos de anotação**: gencode.v44.primary_assembly.annotation.gtf.

## 📂 Estrutura do Repositório

```bash
.
├── fusion_calling/             # Scripts e arquivos relacionados à chamada de fusões
   ├── script_variant_calling.sh   # Script principal para chamada de variantes  
   ├── README.md                   # Este arquivo
   └── dados/                      # Dados de entrada e saída (exemplo)                    
```

## 🚀 Como Executar o Pipeline

### 🔹 Passo 1: Configuração do Ambiente

Certifique-se de que todas as dependências estão instaladas e que os bancos de dados estão configurados corretamente. Defina as variáveis de ambiente no script script_fusion_calling.sh:

```bash
export wd="/caminho/para/diretorio/base"
export MANIFEST="${wd}/manifest_list.txt"
export LOG_FILE="${wd}/Pipeline_RNAseq_$(date +%Y%m%d).log"
export ADAPTERS_FILE="${wd}/Adapters_sequences.fa"
export THREADS=15
export JOBS=4
export ref_GRCh38="${wd}/STAR_alignment/Reference"
```

### 🔹 Passo 2: Execução do Script

Execute o script principal para realizar todas as etapas automaticamente:

```bash
bash script_fusion_calling.sh
```

### 🔹 Passo 3: Verificação dos Resultados

Os resultados serão armazenados nos diretórios definidos no script:

📁 Resultados do STAR: ${wd}/STAR_alignment/

📁 Resultados do RNApeg: ${wd}/output_RNApeg/

📁 Resultados do Cicero: ${wd}/output_Cicero/

📁 Resultados consolidados: ${wd}/output_Cicero_ALLsamples/

## Etapas Detalhadas do Pipeline
### 🔹 1. Pré-processamento

✔️ Download de dados SRA: Baixa os arquivos SRA e converte para FASTQ.

✔️ Controle de qualidade: Executa FastQC para verificar a qualidade das reads e MultiQC para gerar um relatório consolidado.

✔️ Trimming: Usa Trimmomatic para remover adaptadores e filtrar reads de baixa qualidade.

### 🔹 2. Alinhamento

✔️ Indexação do genoma: Cria um índice do genoma de referência utilizando STAR.

✔️ Alinhamento: Mapeia as reads ao genoma de referência GRCh38 e gera arquivos BAM ordenados e indexados.

### 🔹 3. Detecção de Fusões

✔️ RNApeg: Detecta fusões gênicas a partir dos arquivos BAM gerados pelo alinhamento.

✔️ Cicero: Refina as fusões detectadas, aumentando a precisão das chamadas.

### 🔹 4. Pós-processamento

✔️ Consolidação: Organiza os resultados finais em um único diretório, criando links simbólicos para facilitar a visualização e interpretação.

## 📌  Melhorias Futuras

🔹 Suporte para ferramentas adicionais de detecção de fusões.

🔹 Integração com análises de expressão gênica.

🔹 Melhoria na automação, documentação e modularidade do código.


## 📧 Contato

Para dúvidas ou sugestões, entre em contato:

📌 Nome: [Seu Nome]

📩 E-mail: [seu-email@exemplo.com]

🔗Repositório: [Link para o repositório]
    






