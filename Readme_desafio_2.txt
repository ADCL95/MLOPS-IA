Documentación del Proyecto: [DESAFIO #2] Técnicas para el procesamiento del lenguaje (NLP + LLMs)
1. Descripción del Proyecto
Este proyecto tiene como objetivo automatizar y mejorar la precisión en la revisión de perfiles de hojas de vida (CV) utilizando técnicas de Procesamiento del Lenguaje Natural (NLP) y Modelos de Lenguaje Grande (LLMs). La solución busca extraer información clave de los CVs, como el nombre completo del candidato, contacto, años de experiencia, y si tiene formación en inteligencia artificial, para posteriormente devolver esta información en un formato JSON junto con una puntuación que indique la precisión de cada extracción.

2. Configuración del Entorno
Entorno de Trabajo: Google Colab
Lenguaje de Programación: Python
Bibliotecas Utilizadas:
bash
Copy code
!pip install pymupdf transformers
Importación de Librerías:
python
Copy code
import fitz  # PyMuPDF
from transformers import pipeline
import re
Función para la Lectura de PDFs: Implementación de la función que permite la extracción de texto desde archivos PDF.
python
Copy code
def read_pdf(file_path):
    text = ""
    pdf_document = fitz.open(file_path)
    for page_num in range(len(pdf_document)):
        page = pdf_document.load_page(page_num)
        text += page.get_text()
    return text
3. Preprocesamiento de Datos
Extracción de Texto: Descripción del proceso para extraer el texto desde los archivos PDF de los CVs utilizando PyMuPDF.
Limpieza y Formateo: Cualquier transformación adicional del texto antes de su procesamiento.
4. Modelado
Selección del Modelo: Explicación de por qué se eligió un modelo de reconocimiento de entidades nombradas (NER) basado en BERT para la extracción de información.
Configuración del Pipeline NLP:
python
Copy code
nlp = pipeline("ner", model="bert-base-cased")
Función para la Extracción de Información: Implementación que analiza el texto extraído para identificar el nombre, contacto, años de experiencia y formación en IA.
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
5. Evaluación del Modelo
Métricas de Evaluación: Descripción de cómo se calculan los scores para cada campo extraído y cómo se validan los resultados.
Resultados y Análisis: Presentación de los resultados obtenidos, incluyendo ejemplos de la salida en formato JSON.
6. Conclusiones
Resumen de Resultados: Resumen de los hallazgos y la efectividad del modelo en la tarea.
Limitaciones y Mejoras Futuras: Discusión sobre las limitaciones encontradas y posibles mejoras que podrían implementarse en iteraciones futuras.
7. Publicación del Proyecto
Repositorio de GitHub: Incluir el enlace al repositorio donde se ha publicado el código y la documentación.
Instrucciones para Reproducir: Instrucciones claras para que otros usuarios puedan ejecutar el código y obtener los mismos resultados.
Capturas de Pantalla: Evidencia visual de las pruebas realizadas durante el desarrollo.
8. Errores y Aprendizajes
Deuda Técnica: Registro de errores cometidos durante el proceso de implementación, cómo se resolvieron y qué se aprendió de ellos.
