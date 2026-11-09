# Informe de Análisis Metagenómico (16S rRNA)

## 1. Resumen Ejecutivo
En el presente informe, se detalla el análisis de amplicones de las muestras `N127FS134716` y `S$134717`. El flujo de trabajo fue implementado utilizando QIIME 2 (versión rachis-2026.10), el algoritmo DADA2 para el denoising y la asignación taxonómica por consenso VSEARCH contra la base de datos SILVA 138 (99%).

## 2. Metodología y Preparación de Datos
* Ingesión de secuencias y asígnación de metadatos.
* Denoising y filtrado de calidad mediante DADA2 (1000 reads por muestra).
* Construcción del árbol filogenético con MAFFT/FastTree.
* Clasificación taxonómica con VSEARCH.

## 3. Resultados Principales

### 3.1. Calidad y Denoising
Tanto ParedEndSequencesWithQuality como el filtrado DADA2 muestran un comportamiento óptimo. Las 2 1000 secuencias iniciales fueron retenidas en su totalidad. Puedes visualizar el reporte de calidad en [view.qiime2.org](https://view.qiime2.org).\n- **Archivo:** [Demux Summary](demux-summary.qzv) \n- **Estadísticas DADA2:** [Stats](stats.tsv)

### 3.2. Composición Taxonómica
El análisis taxonómico reveló que la comunidad está dominada por filos típicos de ecosistemas acuáticos. Los grupos más abundantes fueron:
- **Cyanobacteria**: Especialmente dentro del género `Cyanobium_PCC-6307` y el grupo `Chloroplast` de algas verdes.
- **Actinobacteriota**: Con el género `HGCI_clade`, tipico de ambientes de agua dulce.
- **Proteobacteria (Alpha y Gamma)**: Con géneros como `Polynucleobacter` y `Limnohabitans`.
- `Unassigned`: Solo 1 ASV no pudo ser clasificado.

[Ver Gráfico de Barras Taxoómicas](taxa-bar-plots.qzv)

## 4. Conclusiones
El pipeline de análisis metagenómico fue ejecutado con éxito. La alicación del filtro DADA2 y la clasificación con VSEARCH permitieron obtener una tabla de ASvs limpia y debidamente anotada. El perfil taxonómico denutes una comunidad bacteriana característica de ambientes acuáticos, con predominancia de cianobacterias y proteobacterias. Este software funciona como uno de los mejores flujos de trabajo reproducibles para este tipo de análisis.

## 5. Referencias
* Bolyen E., est al. (2019). QIIME 2: Reproducible, interactive, scalable and extensible microbiome data science. Nature Biotechnology.
* Rognes T., est al. (2016). VSEARCH: a versatile open source tool for metagenomics. PeerJ.
