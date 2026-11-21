# Generación de descripciones comerciales con destilación

Proyecto académico de procesamiento de lenguaje natural que genera descripciones de productos en español a partir de sus fichas. Se ajusta Qwen3-4B con ejemplos de un modelo docente y se comparan **soft prompt**, **LoRA** y **soft prompt + LoRA**. El cuaderno principal contiene el entrenamiento y la comparación exploratoria entre un docente anterior y uno nuevo, con las mismas 70 fichas de evaluación.

## Motivación

Un modelo especializado más pequeño puede ejecutar esta tarea sin enviar cada producto a un modelo grande como Terra. Esto ofrece mayor control sobre el despliegue y la posibilidad de evitar pagos por cada llamada a un proveedor externo. También podría reducir el cómputo y la energía por descripción; sin embargo, el entrenamiento, el hardware, la demanda y el número de usos influyen en el resultado. **En este proyecto aún no se han medido los costos, el consumo energético ni las emisiones de ambas alternativas**, por lo que la ventaja económica o ambiental es una hipótesis pendiente de evaluación, no un resultado demostrado.

## Pipeline de PLN

1. **Preparación de datos:** se seleccionan fichas de productos con información sustancial y se separan entrenamiento, validación docente y evaluación por grupos de productos. El conjunto de test permanece reservado.
2. **Docente:** un modelo grande redacta ejemplos comerciales condicionados por cada ficha; las propuestas se filtran y revisan con apoyo de IA.
3. **Ajuste del estudiante:** Qwen3-4B aprende a generar las descripciones mediante soft prompt, LoRA o ambos. La pérdida se calcula sobre la respuesta docente.
4. **Generación:** el modelo recibe una instrucción y los atributos disponibles del producto; produce una descripción en español.
5. **Evaluación:** se comparan las tres variantes sobre las mismas 70 fichas con métricas de similitud, longitud y finalización. La revisión factual y la evaluación humana complementan esas medidas.

## Requerimientos

- Para consultar resultados históricos: Python 3.10 o superior, Jupyter y pandas.
- Para entrenar: Google Colab con GPU NVIDIA T4 de 16 GB o una GPU compatible, conexión a Internet y espacio para descargar Qwen3-4B y guardar resultados. La disponibilidad y el costo dependen del entorno de Colab.
- El archivo `paquete_lora_autorizado_20261120.zip`, con las particiones de entrenamiento, validación docente y evaluación exploratoria, se proporciona por separado. El dataset, los pesos y los resultados nuevos no están incluidos en este repositorio.
- Para incorporar los resultados nuevos: el ZIP generado al terminar la ejecución correspondiente.

El cuaderno instala las dependencias de entrenamiento en Colab y registra sus versiones. El entrenamiento está **desactivado por defecto**; consultar la tabla histórica no solicita GPU ni descarga el modelo.

## Instalación y uso

### Opción A: Google Colab

1. Abre [el cuaderno de entrega](notebooks/15_entrega_comparacion_docente.ipynb) en Colab desde GitHub o subiéndolo desde tu equipo.
2. Ejecuta las celdas con `EJECUTAR = False` para consultar los resultados históricos. No se inicia entrenamiento.
3. Para reproducir el entrenamiento, cambia `EJECUTAR` a `True` y elige **Entorno de ejecución → Cambiar tipo de entorno de ejecución → GPU T4**.
4. Ejecuta las celdas en orden y, cuando se solicite, sube `paquete_lora_autorizado_20261120.zip`.
5. Al terminar, descarga el ZIP de resultados y verifica `resultados/estado.json`. En la sección de comparación, indica la ruta de ese ZIP en `resultados_nuevos` y ejecuta la celda para añadir las métricas disponibles.
6. Desconecta el entorno de ejecución una vez verificada la descarga. Los límites del cuaderno interrumpen el trabajo, pero no desconectan automáticamente la GPU.

### Opción B: consulta local

1. Clona el repositorio y entra a su carpeta:

   ```bash
   git clone https://github.com/isaacHeYe/PLN_final_products_destilation.git
   cd PLN_final_products_destilation
   ```

2. Crea y activa un entorno virtual:

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

   En Windows usa `.venv\Scripts\activate`.

3. Instala los requerimientos para consultar el cuaderno:

   ```bash
   python -m pip install -r requirements.txt
   ```

4. Ejecuta `jupyter notebook` y abre `notebooks/15_entrega_comparacion_docente.ipynb`. Mantén `EJECUTAR = False` para consultar las métricas históricas sin GPU.

## Datos y evaluación

El nuevo conjunto docente preparado contiene 451 ejemplos de entrenamiento y 34 de validación; las 70 fichas se usan para evaluación exploratoria. El test permanece reservado. Los ejemplos se aprobaron globalmente y pasaron una revisión asistida por IA, sin que eso implique revisión humana individual.

El cuaderno muestra **chrF++**, longitud media y finalización, y permite comparar las salidas por variante. La similitud con una referencia no demuestra fidelidad ni calidad comercial: la revisión de afirmaciones no respaldadas, cobertura y tono debe hacerse por separado. Los resultados anteriores y nuevos cambian tanto en el docente como en el tamaño de los datos y el presupuesto, de modo que la comparación no aísla el efecto del docente. La ejecución nueva sigue pendiente de incorporar y verificar.

El proyecto usa datos derivados de [Amazon ESCI](https://github.com/amazon-science/esci-data) y el modelo [Qwen3-4B](https://huggingface.co/Qwen/Qwen3-4B); consulta sus condiciones de uso originales antes de redistribuir datos o pesos.
