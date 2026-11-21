# Pre-Entrega-4-API
Extracción de datos desde una API pública con Python, configuración mediante variables de entorno, manejo de errores y almacenamiento. 

## Objetivo

Consultar la API de FRED para obtener el índice mensual de precios de los seguros vinculados al transporte en España durante 2024. La fuente de la serie es Eurostat y su identificador en FRED es `CP1254ESM086NEST`.

## Configuración y ejecución en Google Colab

1. Abrir `Pre_Entrega_4_API.ipynb` desde este repositorio
2. Crear en la computadora un archivo de texto llamado `.env`. Escribir `FRED_API_KEY=tu_clave_personal` y reemplazar el valor de ejemplo por una clave propia de FRED.
3. En el panel **Archivos** de Colab, subir el `.env` a la carpeta de trabajo de la sesión. 
4. Seleccionar **Entorno de ejecución → Ejecutar todas**. El notebook utiliza `requests` y `pandas`, disponibles en Colab, e instala `python-dotenv` en la primera celda.

## Resultado esperado

El notebook genera `data_extracted.csv` con 12 filas, una por mes. La columna `mes` identifica el período y `indice_precio_seguros` contiene el valor numérico del índice. El cuaderno también muestra el recuento de valores nulos y meses duplicados.
