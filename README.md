# Fluxograma_da_Bioinfo
1ª etapa: carregamento das sequências com as duas extremidades pareadas

2ª etapa: pré-processamento das sequências utilizando o fastp, onde será feita a remoção dos adaptadores e a filtragem de qualidade
conda install -c bioconda fastp
sudo apt install fastp
fastp -i input_R1.fastq -I input_R2.fastq -o output_R1_trimmed.fastq -O output_R2_trimmed.fastq -q 20 -l 50 --detect_adapter_for_pe # comando que específica os arquivos de entrada e de saída, define a qualidade mínima de cada leitura, define o comprimento mínimo das leituras após a filtragem e ativa a detecção automática de adaptadores em dados pareados, respectivamente.

3ª etapa: gerar relatório de controle de qualidade com o fastp
fastp -i input_R1.fastq -I input_R2.fastq -o output_R1_trimmed.fastq -O output_R2_trimmed.fastq -q 20 -l 50 --detect_adapter_for_pe -h report.html -j report.json

4ª etapa: mapeamento (bwa)
sudo apt update
sudo apt install bwa #instalação do bwa (burrows-wheeler aligner)
sudo apt install samtools # instalação da ferramenta 
bwa index <genoma_referencia.fasta> #indexação do genoma de referência
bwa mem <genoma_referencia.fasta> <sequencias_tratadas.fastq> > <output.sam> # alinhamento das sequências utilizando sempre os nomes dos arquivos salvos
samtools view -Sb <output.sam> > <output.bam> # conversão do arquivo SAM para BAM
samtools sort <output.bam> -o <output_sorted.bam> # ordene o arquivo BAM
samtools index <output_sorted.bam> # indexação do arquivo BAM
samtools flagstat <output_sorted.bam> # verificar estatística do alinhamento

