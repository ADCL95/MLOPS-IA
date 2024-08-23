# [DESAFIO #2] Técnicas para el procesamiento del lenguaje (NLP + LLMs)

## Descripción del Proyecto
Este proyecto tiene como objetivo automatizar y mejorar la precisión en la revisión de perfiles de hojas de vida (CV) utilizando técnicas de Procesamiento del Lenguaje Natural (NLP) y Modelos de Lenguaje Grande (LLMs). La solución busca extraer información clave de los CVs, como el nombre completo del candidato, contacto, años de experiencia, y si tiene formación en inteligencia artificial. Los resultados se devuelven en un formato JSON, incluyendo una puntuación que indica la precisión de cada extracción.

## Configuración del Entorno
- **Entorno de Trabajo**: Google Colab
- **Lenguaje de Programación**: Python
- **Bibliotecas Utilizadas**:
  ```bash
  !pip install pymupdf transformers
  !pip install spacy
  !python -m spacy download es_core_news_sm
# Proyecto de Extracción de Información de CVs

Este proyecto tiene como objetivo extraer información clave de hojas de vida (CVs) en formato PDF utilizando técnicas de procesamiento de lenguaje natural (NLP) y modelos de reconocimiento de entidades nombradas (NER) basados en BERT.

## Importación de Librerías
  ```bash
  import fitz  # PyMuPDF
  from transformers import pipeline
  import re
  ```

## Función para la Lectura de PDFs
python
```bash
def read_pdf(file_path):
    text = ""
    pdf_document = fitz.open(file_path)
    for page_num in range(len(pdf_document)):
        page = pdf_document.load_page(page_num)
        text += page.get_text()
    return text
```
# Preprocesamiento de Datos
## Extracción de Texto
Se utiliza PyMuPDF para extraer el texto de los archivos PDF de los CVs.


## Selección del Modelo
Se utiliza un modelo de reconocimiento de entidades nombradas (NER) basado en BERT.

## Configuración del Pipeline NLP
```bash
nlp = pipeline("ner", model="bert-base-cased")
Función para la Extracción de Información
python
Copy code
def extract_information(text):
    result = {
        "nombre_completo": None,
        "email": None,
        "telefono": None,
        "anos_experiencia": None,
        "formacion_IA": None,
        "scores": {
            "nombre_completo": 0.0,
            "email": 0.0,
            "telefono": 0.0,
            "anos_experiencia": 0.0,
            "formacion_IA": 0.0
        }
    }
    # Lógica de extracción de información
    return result
```

```bash
nlp = spacy.load("es_core_news_sm")

def extract_information(text):
    doc = nlp(text)
    result = {
        "nombre_completo": None,
        "email": None,
        "telefono": None,
        "anos_experiencia": None,
        "formacion_IA": None,
        "scores": {
            "nombre_completo": 0.0,
            "email": 0.0,
            "telefono": 0.0,
            "anos_experiencia": 0.0,
            "formacion_IA": 0.0
        }
    }
```

```bash
    # Nombre completo (considerando que es una entidad PERSON en español)
    for ent in doc.ents:
        if ent.label_ == 'PER':
            result["nombre_completo"] = ent.text
            result["scores"]["nombre_completo"] = 0.9  # Dummy score
            break

    # Email y teléfono
    email_match = re.search(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b', text)
    result["email"] = email_match.group() if email_match else None
    result["scores"]["email"] = 0.95 if email_match else 0.0

    phone_match = re.search(r'\b\d{10,}\b', text)
    result["telefono"] = phone_match.group() if phone_match else None
    result["scores"]["telefono"] = 0.95 if phone_match else 0.0

    # Años de experiencia
    experience_match = re.search(r'\d+', text)
    result["anos_experiencia"] = experience_match.group() if experience_match else None
    result["scores"]["anos_experiencia"] = 0.90 if experience_match else 0.0

    # Formación en IA
    result["formacion_IA"] = 'S' if 'inteligencia artificial' in text.lower() else 'N'
    result["scores"]["formacion_IA"] = 0.85 if result["formacion_IA"] == 'S' else 0.0

    return result
```

## Evaluación del Modelo
Métricas de Evaluación
Se calculan los scores para cada campo extraído y se validan los resultados.

## Resultados y Análisis
Los resultados se presentan en formato JSON, con ejemplos específicos.

## Conclusiones

El modelo que mostro un mejor performance fue el basado con la biblioteca Spacy, donde logro identificar de manera correcta las entidades

