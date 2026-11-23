# Descripciones comerciales con destilación

Proyecto académico de PLN que genera descripciones de productos en español. Se ajusta Qwen3-4B con ejemplos docentes y se comparan soft prompt, LoRA y su combinación.

## Motivación

Un modelo especializado puede evitar enviar cada ficha a un proveedor externo y podría reducir costos y energía por descripción. No hemos medido costos, energía ni emisiones: estas ventajas son hipótesis pendientes de evaluación.

## Pipeline

1. Selección de fichas y separación por grupos; el test permanece reservado.
2. Generación de ejemplos por un docente y filtrado con apoyo de IA.
3. Ajuste del estudiante con soft prompt, LoRA o ambos.
4. Limpieza, validación y generación de una descripción con controles orientativos.
5. Evaluación automática y humana de las variantes.

## Resultados y archivos

- [Datos](paquete_lora_autorizado_20261120.zip): 451 ejemplos de entrenamiento, 34 de validación y 70 fichas exploratorias. La aprobación docente fue global, sin revisión individual de todos los ejemplos.
- [Resultados y adaptadores](RESULTADOS.md): métricas, instrucciones y pesos entrenados, divididos en tres partes por docente. El modelo base se descarga desde Hugging Face.
- [Evaluación humana](EVALUACION_HUMANA.md), [Excel calificado](evaluacion_calificada.xlsx) y [puntuaciones](puntuaciones.csv).
- [Cuaderno de entrenamiento](notebooks/15_entrega_comparacion_docente.ipynb) y [cuaderno de inferencia](notebooks/16_pipeline_inferencia_productos.ipynb).

En 70 fichas, chrF++ anterior → desarrollado fue 11,03 → 16,35 para soft prompt, 8,43 → 16,23 para LoRA y 7,98 → 17,49 para la combinación. Todas las respuestas finalizaron. La similitud con referencias no demuestra fidelidad ni calidad comercial; cambiaron también los datos y el presupuesto, por lo que no se aísla el efecto del docente.

En una evaluación humana anónima de 10 productos, los puestos medios del docente desarrollado fueron 1,9 para LoRA, 1,9 para la combinación y 2,2 para soft prompt; los del docente anterior fueron 5,5, 5,2 y 4,3, respectivamente. Un puesto menor es mejor. Una sola evaluación no permite medir acuerdo ni generalizar.

## Demostración de inferencia

El cuaderno utiliza Qwen3-4B con LoRA y soft prompt del docente desarrollado. Descarga y verifica los adaptadores publicados y carga el modelo base desde Hugging Face. La interfaz recibe el producto, marca, color y características; genera una propuesta y muestra alertas cuando corresponde. Las comprobaciones son orientativas y pueden fallar o dar falsos positivos; revisa el texto antes de usarlo.

1. Abre el [cuaderno de inferencia en Colab](https://colab.research.google.com/github/isaacHeYe/PLN_final_products_destilation/blob/main/notebooks/16_pipeline_inferencia_productos.ipynb).
2. En Entorno de ejecución → Cambiar tipo de entorno de ejecución, selecciona GPU T4.
3. Ejecuta las celdas en orden; espera la descarga y carga del modelo.
4. Completa la ficha y pulsa **Generar propuesta**. La GPU puede consumir cuota o saldo según tu cuenta.

## Requerimientos e instalación

Para consultar resultados localmente se necesita Python 3.10 o superior y las dependencias de [requirements.txt](requirements.txt). Para entrenar o ejecutar la demo se necesita GPU compatible, conexión a internet y espacio para el modelo.

1. Clona el repositorio: `git clone https://github.com/isaacHeYe/PLN_final_products_destilation.git` y entra con `cd PLN_final_products_destilation`.
2. Crea un entorno con `python3 -m venv .venv` y actívalo con `source .venv/bin/activate`. En Windows usa `.venv\Scripts\activate`.
3. Instala dependencias con `python -m pip install -r requirements.txt`.
4. Abre el cuaderno de entrenamiento en Jupyter o Colab. Mantén `EJECUTAR = False` para consultar las métricas sin entrenar. Para reproducir el entrenamiento, activa GPU, cambia a `True`, ejecuta las celdas en orden y sube el paquete de datos cuando se solicite.
5. Descarga y verifica los resultados; desconecta la GPU cuando termines.

## Fuentes

Los datos se derivan de [Amazon ESCI](https://github.com/amazon-science/esci-data) y el modelo base es [Qwen3-4B](https://huggingface.co/Qwen/Qwen3-4B). Consulta sus condiciones de uso antes de redistribuir o desplegar datos y pesos.
