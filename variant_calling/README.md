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

# Diretório base de trabalho
export WD="/caminho/para/diretorio/base" 

# Caminho para as ferramentas utilizadas na análise
export PATH_TOOLS="/caminho/para/ferramentas"  

# Caminho para o arquivo FASTA do genoma de referência (GRCh38/hg38)
export REF_FASTA="/caminho/para/Homo_sapiens_assembly38.fasta"  

# Caminho para o painel de normal (PON) utilizado na filtragem de variantes somáticas
export PON="/caminho/para/somatic-hg38_1000g_pon.hg38.vcf.gz"  

# Caminho para a base de frequências alélicas do gnomAD para filtragem de variantes comuns
export GNOMAD="/caminho/para/af-only-gnomad.hg38.vcf.gz"  
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

✔️ Mutect2: Detecta variantes somáticas em RNA-seq.

✔️ Filtragem: Aplica filtros como FilterMutectCalls e bcftools norm para normalização.

### 🔹 3. Anotação

✔️ ANNOVAR: Anota as variantes com informações de bancos de dados como gnomAD, COSMIC, ClinVar, etc.

### 🔹 4. Pós-processamento

✔️ Filtragem de variantes PASS: Seleciona apenas variantes com flag PASS.

✔️ Conversão para CRAM: Converte arquivos BAM para CRAM para economia de espaço.



## 📌 Melhorias Futuras

🔹 Adicionar suporte para mais um chamador de variantes: Varscan.

🔹 No caso do COMMPASS, utilizar amostras de DNA do próprio projeto para construcao do Panel of Normals (PoN).

🔹 Implementar realinhamento de resgate com HISAT2 para melhorar a precisão.

## 📧 Contato

📌 Nome: [Seu Nome]

📩 E-mail: [seu-email@exemplo.com]

🔗 Repositório: [Link para o repositório]




