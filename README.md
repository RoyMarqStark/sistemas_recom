# 🎬 Sistemas de Recomendación de Películas (Movie Recommendation Systems)

Este repositorio contiene la implementación paso a paso de tres modelos de sistemas de recomendación para películas, explorando desde enfoques heurísticos/estadísticos simples hasta modelos basados en contenido con **Procesamiento de Lenguaje Natural (NLP)** y **Metadatos Enriquecidos**.

---

## 📌 Contenido del Proyecto

El desarrollo está organizado en 3 cuadernos Jupyter secuenciales:

| Cuaderno | Enfoque | Técnicas / Modelos | Objetivo |
| :--- | :--- | :--- | :--- |
| **`01_recomendador_simple_imdb.ipynb`** | Recomendador Simple | Media Ponderada de IMDB (Percentil 90) | Clon simplificado del Top 250 de IMDB. |
| **`02_recomendador_contenido_sinopsis.ipynb`** | Basado en Contenido (NLP) | TF-IDF Vectorizer + `linear_kernel` | Recomienda películas similares analizando la trama (`overview`). |
| **`03_recomendador_contenido_metadatos.ipynb`** | Basado en Metadatos ("Metadata Soup") | `CountVectorizer` + `cosine_similarity` | Recomienda según director, elenco principal, palabras clave y géneros. |

---

## 🚀 Estructura de los Modelos

### 1. Recomendador Simple (Fórmula IMDB)
Utiliza la métrica de calificación ponderada (Weighted Rating) para evitar el sesgo de películas con calificaciones altas pero muy pocos votos:

$$\text{Media ponderada} = \left(\frac{v}{v + m} \cdot R\right) + \left(\frac{m}{v + m} \cdot C\right)$$

* $v$: Número de votos de la película (`vote_count`).
* $m$: Umbral de corte del percentil 90 de votos requeridos ($m = 160$).
* $R$: Calificación promedio de la película (`vote_average`).
* $C$: Calificación media de todas las películas en el dataset ($C \approx 5.618$).

### 2. Recomendador Basado en Sinopsis (TF-IDF)
* Extrae las descripciones de la columna `overview`.
* Elimina *stop words* en inglés y construye una matriz TF-IDF de dimensión $(45466, 75827)$.
* Calcula la similitud coseno por pares mediante `linear_kernel()`.
* **Hallazgo:** Recomienda películas con sinopsis parecidas (ej. si buscas *The Dark Knight Rises*, sugiere solo otras películas de *Batman*), pero pasa por alto el estilo de dirección o la química de los actores.

### 3. Recomendador Basado en Metadatos Enriquecidos ("Metadata Soup")
* Fusiona `movies_metadata.csv` con `credits.csv` y `keywords.csv`.
* Parsea datos serializados JSON con `ast.literal_eval`.
* Extrae:
  * El **Director** de la película.
  * Los **3 actores principales** (`cast`).
  * Las **palabras clave temáticas** (`keywords`).
  * Los **géneros** (`genres`).
* Normaliza y concatena en una cadena unificada (`soup`), eliminando espacios para evitar ambigüedades en nombres de personas (ej. `johnnydepp`).
* Vectoriza con `CountVectorizer` y calcula similitud con `cosine_similarity`.
* **Resultado:** Si buscas *The Dark Knight Rises*, ahora sugiere obras afines de **Christopher Nolan** (*The Prestige*) junto a las mejores películas de su universo.

---

## 🛠️ Instalación y Requisitos

### 1. Clonar el repositorio
```bash
git clone https://github.com/TU-USUARIO/sistemas_recom.git
cd sistemas_recom
```

### 2. Crear y activar entorno virtual
```bash
# Windows
python -m venv venv
.\venv\Scripts\activate

# Linux / Mac
python3 -m venv venv
source venv/bin/activate
```

### 3. Instalar dependencias
```bash
pip install -r requirements.txt
```

---

## 📂 Configuración del Conjunto de Datos

Los datos provienen del [The Movies Dataset en Kaggle](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset). Para ejecutar los cuadernos, asegúrate de colocar los siguientes archivos dentro de la carpeta `data/`:

* `movies_metadata.csv`
* `keywords.csv`
* `credits.csv`

*(Nota: La carpeta `data/` con los archivos CSV está excluida del repositorio debido al límite de tamaño de archivo de GitHub).*

---

## 💻 Ejecución

Inicia el entorno de Jupyter Notebook en tu navegador o abre los cuadernos directamente en tu editor (VS Code, Cursor, JupyterLab):

```bash
jupyter notebook
```

Ejecuta los cuadernos en orden secuencial:
1. `01_recomendador_simple_imdb.ipynb`
2. `02_recomendador_contenido_sinopsis.ipynb`
3. `03_recomendador_contenido_metadatos.ipynb`
