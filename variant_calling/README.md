# 📌 Pipeline de Chamada de Variantes Somáticas em RNA-seq


Pipeline para análise de variantes somáticas em dados de RNA-seq, utilizando ferramentas como GATK Mutect2, Picard, Samtools, bcftools e ANNOVAR.
📅 Última atualização: 23/01/2025


## 🔍 Visão Geral 

O pipeline é dividido nas seguintes etapas principais:

### 🛠️ Pré-processamento:

✅ Verificação de qualidade dos alinhamentos

✅ Filtragem de cromossomos alternativos

✅ Marcação de duplicatas

✅ Recalibração de bases (BQSR)


### 🧬 Chamada de Variantes:

✅ Execução do GATK Mutect2 para detecção de variantes somáticas

✅ Filtragem e normalização das variantes

### 🔬 Anotação:

✅ Anotação das variantes utilizando ANNOVAR

### 📑 Pós-processamento:

✅ Filtragem de variantes com flag PASS

✅ Conversão de arquivos BAM para CRAM

## 📦 Requisitos
### 🔧 Dependências

- GATK 4.6.0.0
    
- Picard 3.2.0
    
- Samtools
    
- bcftools
    
- ANNOVAR
    
- GNU Parallel

### 🗄️ Bancos de Dados

- Genoma de referência: Homo_sapiens_assembly38.fasta

- Panel of Normals (PoN): somatic-hg38_1000g_pon.hg38.vcf.gz

- gnomAD: af-only-gnomad.hg38.vcf.gz

- dbSNP: dbsnp_151.hg38.vcf.gz

- Indels conhecidos: Mills_and_1000G_gold_standard.indels.hg38.vcf

## 📂 Estrutura do Repositório

```bash
├── variant_calling/            # Scripts e arquivos relacionados à chamada de variantes
├── fusion_calling/             # Scripts e arquivos relacionados à chamada de fusões
├── script_variant_calling.sh   # Script principal para chamada de variantes
├── script_fusion_calling.sh    # Script principal para chamada de fusões
├── README.md                   # Este arquivo
└── dados/                      # Dados de entrada e saída (exemplo)
```


## 🚀 Como Executar o Pipeline
### 🔹 Passo 1: Configuração do Ambiente

Certifique-se de que todas as dependências estão instaladas e os bancos de dados configurados corretamente.

Defina as variáveis de ambiente para o script: 

```bash

# 1️⃣ Diretórios principais
##########################
export WD="caminho/para/diretorio/base"  # Diretório base do projeto
export PATH_TOOLS="caminho/para/ferramentas"  # Caminho para ferramentas e arquivos de referência


# 2️⃣ Listas e Arquivos de Controle
##########################
export BAM_LIST="${WD}/ALLsamples"  # Lista de arquivos BAM a serem processados
export MANIFEST="${WD}/your_manifest.txt"  # Arquivo de manifesto do GDC
export GDC_TOKEN="${WD}/gdc-user-token.********"  # Token de autenticação do GDC


# 3️⃣ Configuração de Recursos
##########################
export MEM=100  # Memória máxima alocada por tarefa (Cuidado: Ao definir, verifique o espaco disponivel  na maquina e considerare execução paralela)
export JOBS=2  # Número de jobs em execução simultânea


# 4️⃣ Diretórios de saída e logs
##########################
export PREPROCESSING_DIR="${WD}/preprocessing_result"  # Diretório de saída do pré-processamento
export OUTPUT_DIR="${WD}/Result_Mutect2.GATK4.6"  # Diretório de saída do Mutect2
export LOG_DIR="${WD}/Arquivos_log"  # Diretório para enviar os arquivos logs
export PREPROCESS_LOG="${LOG_DIR}/preprocessing_$(date +%Y%m%d).log"  # Arquivo log para a etapa de pré-processamento
export MUTECT_LOG="${LOG_DIR}/Pipeline_Mutect2_$(date +%Y%m%d).log"  # Arquivo log do Mutect2 e pos processamento 


# 5️⃣ Arquivos de Referência Genômica
##########################
export REF_FASTA="/home/projects2/LIDO/molPathol/oncoseek/nextseq/hg38/Homo_sapiens_assembly38.fasta"  # Genoma de referência
export TARGET="${WD}/exons_basic_hg38v47_chr.bed"  # Regiões-alvo para análise(limita a regiao de analise)
export PON="${WD}/somatic-hg38_1000g_pon.hg38.vcf.gz"  # Painel de normais (PoN)
export GNOMAD="${PATH_TOOLS}/references/af-only-gnomad.hg38.vcf.gz"  # Frequência de variantes na população gnomAD

# Alternativa GNOMAD:
# export GNOMAD="${WD}/references/af-only-gnomad.SABE1171.Abraom.hg38.vcf.gz"


# 6️⃣ Ferramentas
##########################
export GATK="${PATH_TOOLS}/tools/gatk-4.6.0.0/gatk"  # Caminho do GATK 4.6
export PICARD="${PATH_TOOLS}/tools/picard-3.2.0/picard.jar"  # Caminho do Picard
export ANNOVAR="${PATH_TOOLS}/tools/annovar/table_annovar.pl"  # Caminho do ANNOVAR
export ANNOVAR_DB="${PATH_TOOLS}/humandb/"  # Banco de dados do ANNOVAR


# 7️⃣ Arquivos de Anotação e Cruzamento
##########################
export CROSS_REFERENCE="${PATH_TOOLS}/references/refGene_TARGET_COSMICv82CensusGene_F1.txt"  # Arquivo de referência cruzada


# 8️⃣ Banco de Variantes Conhecidas
##########################
export INDEL_KNOWN="/home/projects2/LIDO/molPathol/oncoseek/nextseq/references/Mills_and_1000G_gold_standard.indels.hg38.vcf"  # Indels conhecidos
export DBSNP="${PATH_TOOLS}/references/dbsnp_151.hg38.vcf.gz"  # Banco de variantes do tipo SNP do banco de dados dbSNP

```

## 🔹 Passo 2: Execução do Script

O script principal (script_variant_calling.sh) recebe dois argumentos:
1️⃣ Lista de amostras: Arquivo contendo os nomes das amostras a serem processadas.
2️⃣ Diretório SCRATCH: Diretório temporário para armazenar arquivos intermediários.

Execute o script da seguinte forma:

```bash
bash script_variant_calling.sh lista_de_amostras.txt /caminho/para/scratch
```

## 🔹 Passo 3: Verificação dos Resultados

Os resultados serão armazenados nos seguintes diretórios:

📁 Resultados do Mutect2: ${OUTPUT_DIR}/Mutect2/

📁 Anotações: ${OUTPUT_DIR}/annotation/

📁 Arquivos filtrados: ${OUTPUT_DIR}/FILTER_PASS/


## 🔬 Etapas Detalhadas do Pipeline
### 🔹 1. Pré-processamento

✔️ Filtragem de cromossomos alternativos: Remove contigs não padrão (ex: chrUn, alt scaffolds).

✔️ Marcação de duplicatas: Utiliza o Picard para marcar reads duplicados.

✔️ Recalibração de bases (BQSR): Ajusta as qualidades das bases com base em padrões de erro conhecidos.

### 🔹 2. Chamada de Variantes

✔️ Mutect2: Ferramenta de chamada de variantes somaticas (SNVs e INDELs) do GATK

✔️ Filtragem: Aplica filtros como FilterMutectCalls e bcftools norm para normalização.

### 🔹 3. Anotação

✔️ ANNOVAR: Anota as variantes com informações de bancos de dados como gnomAD, COSMIC, ClinVar, etc.

### 🔹 4. Pós-processamento

✔️ Filtragem de variantes PASS: Seleciona apenas variantes com flag PASS.

✔️ Conversão para CRAM: Converte arquivos BAM para CRAM para economia de espaço.



## 📌 Melhorias Futuras

🔹 Adicionar suporte para mais um chamador de variantes: Ex Varscan.

🔹 No caso do COMMPASS, utilizar amostras de DNA do próprio projeto para construcao do Panel of Normals (PoN).

🔹 Implementar realinhamento de resgate com HISAT2 para melhorar a precisão.

## 📧 Contato

📌 Nome: [Seu Nome]

📩 E-mail: [seu-email@exemplo.com]

🔗 Repositório: [Link para o repositório]




