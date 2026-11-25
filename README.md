# Preentrega 4 · Extracción de datos de seguros con la API de FRED

## Objetivo

Obtener el índice mensual de precios de los seguros vinculados al transporte en España durante 2024. El proyecto consulta la API de FRED mediante una petición GET, transforma la respuesta JSON con pandas y guarda el resultado en un archivo CSV.

La serie consultada es `CP1254ESM086NEST`, publicada por FRED a partir de datos de Eurostat.

[Consultar la serie en FRED](https://fred.stlouisfed.org/series/CP1254ESM086NEST) · [Documentación de la consulta de observaciones](https://fred.stlouisfed.org/docs/api/fred/series_observations.html)

## Archivos del proyecto

| Archivo | Contenido |
|---|---|
| `Pre_Entrega_4_API.ipynb` | Notebook principal con la consulta a FRED, el manejo de errores, la transformación y el guardado. |
| `data_extracted.csv` | Resultado de la extracción mensual de 2024. |
| `.env.example` | Plantilla con el nombre de la variable requerida para autenticar la consulta. |
| `requirements.txt` | Dependencias para procesar los datos y ejecutar el notebook con JupyterLab. |
| `.gitignore` | Reglas para excluir las credenciales locales, los entornos virtuales y los archivos temporales. |
| `README.md` | Instrucciones para configurar, ejecutar y verificar el proyecto. |

## Instalación y ejecución local

Se necesita Python 3, Git, y una clave personal de FRED. La clave puede obtenerse desde la [página de claves de la API de FRED](https://fredaccount.stlouisfed.org/apikey).

### 1. Clonar el repositorio

Abrir una terminal y ejecutar:

```bash
git clone https://github.com/natyherreraa/Pre-Entrega-4-API.git
cd Pre-Entrega-4-API
```

### 2. Crear el entorno virtual e instalar las dependencias

Desde la carpeta del proyecto, crear el entorno virtual:

```bash
python -m venv .venv
```

Activar el entorno virtual .venv y ejecutar en la misma terminal el siguiente comando para instalar las dependencias del proyecto:

```bash
python -m pip install -r requirements.txt
```

### 3. Configurar la clave de FRED

Duplicar `.env.example` en la carpeta del proyecto y nombrar la copia `.env`. Abrir `.env` en un editor de texto y reemplazar `tu_api_key_aqui` por la clave personal:

```dotenv
FRED_API_KEY=tu_api_key_aqui
```

Guardar `.env` junto al notebook. Al ejecutar las celdas, `python-dotenv` carga el archivo y el código obtiene la clave mediante `os.getenv("FRED_API_KEY")`. La plantilla `.env.example` se comparte en GitHub y `.gitignore` excluye el archivo local `.env`.

### 4. Ejecutar el notebook

Con el entorno virtual activado, ejecutar desde la carpeta del proyecto:

```bash
python -m jupyterlab Pre_Entrega_4_API.ipynb
```

En el notebook que se abre en el navegador, seleccionar **Run → Run All Cells**. Las celdas se ejecutan en orden. Si la extracción finaliza correctamente, se genera o actualiza `data_extracted.csv` en la misma carpeta que el notebook.

## Ejecución alternativa en Google Colab

1. Abrir `Pre_Entrega_4_API.ipynb` en Google Colab.
2. Preparar el archivo `.env` con la clave personal de FRED siguiendo el paso 3 de la configuración local.
3. Desde el panel **Archivos** de Colab, subir `.env` a la carpeta de trabajo de la sesión.
4. Seleccionar **Entorno de ejecución → Ejecutar todas**.
5. Al finalizar, descargar `data_extracted.csv` desde el panel **Archivos** para conservar el resultado.

## Extracción y transformación

El notebook consulta el endpoint `https://api.stlouisfed.org/fred/series/observations` con el identificador de la serie y el período comprendido de 2024. 

La celda de extracción establece un tiempo límite para la petición y comprueba el código de estado HTTP. Cuando la consulta tiene éxito, lee la lista `observations` de la respuesta JSON. Si la petición falla, informa el problema.

A partir de las observaciones recuperadas, el notebook construye una tabla con dos columnas:

| Columna de salida | Transformación |
|---|---|
| `mes` | Se obtiene del campo `date` y se expresa en formato `AAAA-MM`. |
| `indice_precio_seguros` | Se obtiene del campo `value` y se convierte a un valor numérico. |

El notebook muestra el recuento de valores nulos y de meses duplicados. La última etapa exporta la tabla a `data_extracted.csv` mediante `to_csv(..., index=False)`.

## Cómo verificar el resultado

- **Extracción.** La celda de consulta debe mostrar el código HTTP `200` y 12 observaciones recuperadas.
- **Transformación.** La tabla debe contener un registro por mes, desde `2024-01` hasta `2024-12`. Los controles permiten comprobar si hay valores faltantes o meses repetidos.
- **Persistencia.** Para verificar que el notebook genera el archivo desde cero, renombrar el CSV incluido a `data_anterior.csv` en una copia local del proyecto y ejecutar todas las celdas. Al finalizar, debe aparecer un nuevo `data_extracted.csv` con las dos columnas y las 12 filas de datos.