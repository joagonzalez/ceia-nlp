
# CEIA 18Co2024 - Procesamiento de Lenguaje Natural
Este repositorio contiene los resultados de cuatro desafíos enfocados en el modelado de lenguaje y traducción automática. Se utilizaron corpus clásicos y arquitecturas basadas en embeddings, RNNs, LSTMs y modelos Seq2Seq con y sin embeddings preentrenados.

## Tabla de Contenidos

- [Desafío 1: Clasificación de Documentos y Análisis de Similaridad](#desafío-1-clasificación-de-documentos-y-análisis-de-similaridad)
- [Desafío 2: Word2Vec sobre textos clásicos](#desafío-2-word2vec-sobre-textos-clásicos)
- [Desafío 3: Modelado de Lenguaje con RNNs, LSTM y GRU](#desafío-3-modelado-de-lenguaje-con-rnns-lstm-y-gru)
- [Desafío 4: Traducción automática (EN → ES) con Seq2Seq](#desafío-4-traducción-automática-en-es-con-seq2seq)


## Desafío 1: Clasificación de Documentos y Análisis de Similaridad

**Objetivo:**  
Aplicar técnicas de vectorización para representar documentos en forma numérica y analizarlos desde tres enfoques:

1. Medir similitud entre documentos utilizando TF-IDF.
2. Entrenar clasificadores Naïve Bayes para categorizar los textos.
3. Analizar la similitud semántica entre palabras al transponer la matriz documento-término.

---

### Dataset utilizado

Se utilizó el dataset **20 Newsgroups**, que contiene noticias agrupadas en 20 categorías.  
Se eliminó metadata irrelevante (headers, footers y quotes) antes de procesar el texto.

---

### Metodología

- **Vectorización:** Se aplicó `TfidfVectorizer` para representar los documentos. Se analizaron los 5 documentos más similares a una muestra aleatoria.
- **Clasificación:** Se evaluaron variantes de Naïve Bayes (`MultinomialNB` y `ComplementNB`) con distintas configuraciones de n-gramas y filtrado de términos.
- **Análisis semántico:** Se transpuso la matriz documento-término para observar similitudes entre palabras basadas en el contexto de aparición. Se analizaron 5 palabras seleccionadas manualmente.

---

### Resultados

- **Documentos similares**: los documentos más cercanos en el espacio vectorial compartían clase y palabras clave, evidenciando una buena representación semántica por TF-IDF.
- **Modelado supervisado**: la configuración óptima incluyó n-gramas de hasta 2 palabras y `stop_words='english'`, obteniendo **F1-score > 0.66** con `MultinomialNB`.
- **Palabras similares**: la transposición permitió observar grupos de palabras similares según su coocurrencia, confirmando que TF-IDF conserva estructura semántica útil.

---

### Conclusiones

La representación TF-IDF, aunque simple, permite capturar de forma efectiva la similitud semántica tanto a nivel de documentos como de términos. Además, proporciona una base sólida para tareas supervisadas de clasificación.

---

## Desafío 2: Word2Vec sobre textos clásicos

**Objetivo:** Entrenar modelos Word2Vec sobre textos de *La Biblia*, *La Odisea* y *La Eneida* para analizar relaciones semánticas.

**Técnicas utilizadas:**
- Limpieza y tokenización de corpus.
- WordCloud y análisis de frecuencia.
- Word2Vec con `vector_size=300`, `window=5`, `min_count=5`.
- Pruebas de analogías y visualización de distancias (matriz de similitud coseno).
- Reducción de dimensionalidad con t-SNE.

**Conclusiones:** El modelo logró capturar relaciones semánticas esperables (e.g. *dios* ≈ *jehova*, *justicia* ≈ *equidad*) aunque limitado por tamaño del corpus.

---

## Desafío 3: Modelado de Lenguaje con RNNs, LSTM y GRU

**Objetivo:** Crear un modelo de lenguaje que permita generar oraciones por caracter entrenado sobre el mismo corpus anterior.
Utilizar estrategias Greedy, Sampling, Beam Search y Sthocastic BeamSearch para la selección de próximo caracter y jugar con los valores de temperatura en los casos que aplique.

**Modelos comparados:**
- `SimpleRNN`
- `LSTM`
- `GRU`

**Evaluación:**
- Early Stopping
- Métrica: Perplejidad (PPL)
- Generación de texto con:
  - Greedy Search
  - Sampling (temperatura 0.5 / 0.7 / 1.0)
  - Beam Search determinístico y estocástico

**Conclusiones:** GRU mostró un buen trade-off entre rendimiento y tiempo. Las muestras generadas mostraron loops en greedy y mejor creatividad con sampling y beam search.

---

## Desafío 4: Traducción automática (EN ES) con Seq2Seq

**Objetivo:** Construir un traductor inglés-español con arquitectura Encoder-Decoder basada en LSTM.

**Dataset:** Pares de oraciones en inglés y español (Anki)

**Componentes:**
- Embeddings preentrenados GloVe para inglés.
- Encoder LSTM (2 capas, dropout).
- Decoder LSTM con teacher forcing.
- Padding, tokenización y vocabularios custom.

**Evaluación:**
- Métrica de pérdida: `CrossEntropyLoss`
- Visualización de `Loss Curve`
- Traducción de frases de test y frases nuevas

**Conclusiones:** El modelo aprende estructuras gramaticales básicas. Falla con frases complejas o semánticamente ambiguas. Se proponen mejoras con regularización y corpus extendido.


