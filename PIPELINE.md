# Pipeline de Análisis Metagenomico (rachis-qiime2-2026.10)

Este documento detalla el flujo de trabajo reproducible utilizado para el análisis de amplicones 16S rRNA. Incluye las adaptaciones necesarias para la versión 2026.10 de QIIME 2.

## 1. Preparación de Datos
Creación del archivo `manifest.tsv` con las rutas absolutas de los archivos FASTQ:

```bash
printf "sample-id\tforward-absolute-filepath\treverse-absolute-filepath\nSample_134716\t%s/fastq/secuencias Fastq/Sample_134716_R1.fastq.gz\t%s/fastq/secuencias Fastq/Sample_134716_R2.fastq.gz\nSample_134717\t%s/fastq/secuencias Fastq/Sample_134717_R1.fastq.gz\t%s/fastq/secuencias Fastq/Sample_134717_R2.fastq.gz\n" "$PWD" "$PWD" "$PWD" "$PWD" > fastq/manifest.tsv
```

Creación del archivo de metadatos:

```bash
printf "#SampleID\nSample_134716\nSample_134717\n" > sample-metadata.tsv
```

## 2. Importación y Control de Calidad
Importación de las secuencias a formato QIIME 2 (`.qza`):

```bash
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path fastq/manifest.tsv \
  --output-path demux-paired-end.qza \
  --input-format PairedEndFastqManifestPhred33V2
```

Resumen visual de la calidad:

```bash
qiime demux summarize \
  --i-data demux-paired-end.qza \
  --o-visualization demux-summary.qzv
```

## 3. Denoising (DADA2)
*Nota: La versión 2026.10 requiere el parámetro obligatorio `--o-base-transition-stats`.*

```bash
qiime dada2 denoise-paired \
  --i-demultiplexed-seqs demux-paired-end.qza \
  --p-trim-left-f 0 \
  --p-trim-left-r 0 \
  --p-trunc-len-f 0 \
  --p-trunc-len-r 0 \
  --o-table table.qza \
  --o-representative-sequences rep-seqs.qza \
  --o-denoising-stats denoising-stats.qza \
  --o-base-transition-stats base-transition-stats.qza
```

## 4. Filogenia y Taxonomía
Construcción del árbol filogenético:

```bash
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences rep-seqs.qza \
  --o-alignment aligned-rep-seqs.qza \
  --o-masked-alignment masked-aligned-rep-seqs.qza \
  --o-tree unrooted-tree.qza \
  --o-rooted-tree rooted-tree.qza
```

Clasificación taxonómica usando VSEARCH (para evitar errores de memoria con Naive Bayes):

```bash
qiime feature-classifier classify-consensus-vsearch \
  --i-query rep-seqs.qza \
  --i-reference-reads silva-138-99-seqs.qza \
  --i-reference-taxonomy silva-138-99-tax.qza \
  --o-classification taxonomy.qza \
  --o-search-results search-results.qza \
  --p-threads 0
```

Gráfico de barras taxonómicas:

```bash
qiime taxa barplot \
  --i-table table.qza \
  --i-taxonomy taxonomy.qza \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization taxa-bar-plots.qzv

```

## 5. Exportación de Resultados
Conversión de los archivos `.qza` a formatos abiertos para su análisis en Excel/R:

```bash
# Crear carpeta
mkdir -p exportados

# Exportar tabla (formato BIOM)
qiime tools export --input-path table.qza --output-path exportados/tabla_asvs

# Convertir BIOM a TSV
biom convert -i exportados/tabla_asvs/feature-table.biom -o exportados/tabla_asvs/feature-table.tsv --to-tsv

# Exportar taxonomía
qiime tools export --input-path taxonomy.qza --output-path exportados/taxonomia

# Exportar estadísticas DADA2
qiime tools export --input-path denoising-stats.qza --output-path exportados/estadisticas_dada2
```
