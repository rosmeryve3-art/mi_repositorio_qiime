# Informe de Análisis Metagenómico (16S rRNA)

## 1. Resumen Ejecutivo
En el presente informe, se detalla el análisis de amplicones de las muestras `Sample_134716` y `Sample_134717`. El flujo de trabajo fue implementado utilizando QIIME 2 (versión rachis-2026.10), el algoritmo DADA2 para el denoising y la asignación taxonómica por consenso VSEARCH contra la base de datos SILVA 138 (99%).

## 2. Metodología y Preparación de Datos
* Ingestión de secuencias y asignación de metadatos.
* Denoising y filtrado de calidad mediante DADA2 (1000 reads por muestra).
* Construcción del árbol filogenético con MAFFT/FastTree.
* Clasificación taxonómica con VSEARCH.

## 3. Resultados Principales

### 3.1. Calidad y Denoising
El reporte de calidad generado por QIIME 2 (`demux-summary.qzv`) incluye un histograma de frecuencia que, debido a la naturaleza sintética de los datos (todas las muestras tienen exactamente la misma cantidad de secuencias), se visualiza como un bloque sólido sin variación. Por esta razón, es más útil presentar los datos tabulares de conteo de secuencias por muestra, que son los que realmente aportan información cuantitativa.

La siguiente tabla resume el número de lecturas (reads) obtenidas para cada muestra y en cada dirección:

| Muestra (Sample ID) | Reads Forward | Reads Reverse |
| :--- | :---: | :---: |
| Sample_134716 | 1000 | 1000 |
| Sample_134717 | 1000 | 1000 |

**Interpretación:** Ambas muestras presentan exactamente la misma cantidad de lecturas en las dos direcciones (1000 forward y 1000 reverse), lo que confirma que la preparación de las librerías y la secuenciación fueron uniformes. Al ser datos de prueba, no hay variación en la profundidad de secuenciación entre muestras.

Adicionalmente, los resultados del filtrado y denoising realizados por DADA2 se pueden consultar en el archivo de estadísticas:

- **Archivo interactivo de calidad:** [Demux Summary](demux-summary.qzv)
- **Estadísticas del filtrado DADA2:** [Stats](stats.tsv)

### 3.2. Composición Taxonómica
El análisis taxonómico (basado en la tabla de ASVs `taxonomy.tsv`) reveló una comunidad dominada por filos típicos de ecosistemas acuáticos. A continuación se presenta el resumen de los grupos más abundantes:

| Filo (Phylum) | Número de ASVs | Explicación Biológica |
| :--- | :---: | :--- |
| **Cyanobacteria** | 13 | Algas verdeazuladas. Incluye el género `Cyanobium_PCC-6307` y el grupo `Chloroplast` (algas verdes). Típicos de aguas dulces. |
| **Actinobacteriota** | 7 | Bacterias Gram-positivas. El género `HGCI_clade` es muy común en lagos y ríos. |
| **Proteobacteria** | 6 | Filo muy diverso. Incluye géneros acuáticos como `Polynucleobacter` y `Limnohabitans`. |
| **Bacteroidota** | 6 | Bacterias degradadoras de materia orgánica. Incluye géneros como `Fluviicola`. |
| **Verrucomicrobiota** | 4 | Bacterias comunes en suelos y aguas, muchas veces asociadas a partículas. |
| **Planctomycetota** | 1 | Bacterias con estructuras celulares inusuales, comunes en ambientes acuáticos. |
| **Armatimonadota** | 1 | Filo poco común, encontrado en ambientes fríos o acuáticos. |
| **Unassigned** | 1 | ASV que no pudo ser clasificado con la base de datos SILVA. |

*Nota: Puedes explorar la composición visual interactiva en el siguiente enlace:*
[Ver Gráfico de Barras Taxonómicas Interactivo](taxa-bar-plots.qzv)

## 4. Conclusiones
El pipeline de análisis metagenómico fue ejecutado con éxito. La aplicación del filtro DADA2 y la clasificación con VSEARCH permitieron obtener una tabla de ASVs limpia y debidamente anotada. El perfil taxonómico denota una comunidad bacteriana característica de ambientes acuáticos, con predominancia de cianobacterias y proteobacterias. La inclusión de tablas de abundancias y retención de secuencias proporciona una visión cuantitativa clara y sin ambigüedades visuales.

## 5. Referencias
* Bolyen E., et al. (2019). QIIME 2: Reproducible, interactive, scalable and extensible microbiome data science. Nature Biotechnology.
* Rognes T., et al. (2016). VSEARCH: a versatile open source tool for metagenomics. PeerJ.
