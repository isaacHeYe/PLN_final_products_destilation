# Generación de descripciones comerciales con destilación

Proyecto académico de PLN que genera descripciones de productos en español. Se ajusta Qwen3-4B con ejemplos de un modelo docente y se comparan soft prompt, LoRA y su combinación.

## Motivación

Un modelo especializado más pequeño puede ejecutar esta tarea sin enviar cada ficha a un modelo grande. Esto ofrece control sobre el despliegue y la posibilidad de evitar pagos por cada llamada a un proveedor externo. También podría reducir el cómputo y la energía por descripción. **No hemos medido los costos, el consumo energético ni las emisiones**, por lo que esas ventajas son hipótesis pendientes de evaluación.

## Pipeline

1. Selección y separación por grupos de fichas con información suficiente; el test permanece reservado.
2. Generación de ejemplos comerciales por un modelo docente y filtrado con apoyo de IA.
3. Ajuste del estudiante mediante soft prompt, LoRA o ambos; la pérdida se calcula sobre la respuesta docente.
4. Limpieza y validación de la ficha, generación y controles orientativos de lenguaje y fidelidad.
5. Evaluación automática y humana sobre fichas exploratorias.

## Datos, pesos y resultados

- [Paquete de datos](paquete_lora_autorizado_20261120.zip): 451 ejemplos docentes de entrenamiento, 34 de validación y 70 fichas de evaluación exploratoria. Los ejemplos se aprobaron globalmente y se revisaron con apoyo de IA, sin revisión humana individual de todos ellos.
- [Resultados y adaptadores](RESULTADOS.md): comparación y descripción de los adaptadores de los docentes anterior y desarrollado, publicados en tres partes por paquete. El modelo base se descarga por separado desde Hugging Face.
- [Evaluación humana](EVALUACION_HUMANA.md), [Excel calificado](evaluacion_calificada.xlsx) y [puntuaciones](puntuaciones.csv).

En las mismas 70 fichas, chrF++ pasó de 11,03 a 16,35 para soft prompt, de 8,43 a 16,23 para LoRA y de 7,98 a 17,49 para la combinación. Las 70 respuestas finalizaron para cada variante. chrF++ mide similitud con una referencia, no fidelidad ni calidad comercial. Cambiaron también el tamaño de los datos y el presupuesto, por lo que la comparación no aísla el efecto del docente.

Una evaluación humana anónima ordenó seis opciones en 10 productos. Los puestos promedio del docente desarrollado fueron 1,9 para LoRA, 1,9 para soft prompt + LoRA y 2,2 para soft prompt; los del docente anterior fueron 5,5, 5,2 y 4,3, respectivamente. El puesto 1 es el mejor. Se trata de una sola evaluación: no permite estimar acuerdo entre evaluadores ni generalizar.

## Requerimientos

Para consultar resultados: Python 3.10 o superior, Jupyter y las dependencias de [requirements.txt](requirements.txt). Para entrenar: Google Colab con GPU NVIDIA T4 de 16 GB o compatible, conexión a internet y espacio disponible. La disponibilidad y el costo de GPU dependen de la cuenta y el entorno.

## Instalación y uso

1. Clona el repositorio y entra a la carpeta:

   git clone https://github.com/isaacHeYe/PLN_final_products_destilation.git
   cd PLN_final_products_destilation

2. Crea un entorno virtual con python3 -m venv .venv y actívalo con source .venv/bin/activate. En Windows usa .venv\Scripts\activate.
3. Instala las dependencias con python -m pip install -r requirements.txt.
4. Ejecuta jupyter notebook y abre [el cuaderno de entrega](notebooks/15_entrega_comparacion_docente.ipynb). Mantén EJECUTAR = False para consultar las métricas históricas sin GPU.

### Entrenamiento en Colab

1. Abre el cuaderno de entrega en Colab y selecciona GPU T4 en Entorno de ejecución → Cambiar tipo de entorno de ejecución.
2. Cambia EJECUTAR a True y ejecuta las celdas en orden. Sube el paquete de datos cuando se solicite.
3. Descarga el ZIP de resultados y verifica resultados/estado.json. Puedes indicar la ruta del ZIP en resultados_nuevos para incorporar las métricas a la comparación.
4. Desconecta el entorno cuando termines: los límites del cuaderno no desconectan automáticamente la GPU.

## Inferencia

La demostración utiliza Qwen3-4B con LoRA y soft prompt del docente desarrollado. Descarga el modelo base desde Hugging Face y los adaptadores desde este repositorio. Requiere una GPU compatible. El cuaderno de inferencia aún debe incorporarse al repositorio para que el equipo pueda reproducir la demostración desde aquí. Sus filtros son orientativos y pueden fallar o generar falsos positivos; revisa las descripciones antes de usarlas.

## Fuentes

El paquete contiene una adaptación de fichas y referencias de [Amazon ESCI](https://github.com/amazon-science/esci-data); las descripciones docentes se generaron para este proyecto. El modelo base es [Qwen3-4B](https://huggingface.co/Qwen/Qwen3-4B). Consulta sus condiciones de uso antes de redistribuir o desplegar los datos y pesos.
