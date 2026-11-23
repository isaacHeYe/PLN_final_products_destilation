# Resultados y adaptadores

Los paquetes `docente_anterior.zip` y `docente_desarrollado.zip` se distribuyen en tres partes de hasta 8 MB cada uno. Incluyen, para soft prompt, LoRA y su combinación, el mejor adaptador validado, los registros de entrenamiento, la configuración y las 70 descripciones generadas. No incluyen el modelo base, el conjunto de test ni estados del optimizador.

## Comparación exploratoria

| Variante | chrF++ anterior | chrF++ desarrollado | Palabras anteriores | Palabras desarrolladas | Respuestas finalizadas |
|---|---:|---:|---:|---:|---:|
| Soft prompt | 11,03 | 16,35 | 33,37 | 60,49 | 70/70 en ambos |
| LoRA | 8,43 | 16,23 | 25,43 | 58,91 | 70/70 en ambos |
| Soft prompt + LoRA | 7,98 | 17,49 | 24,13 | 64,23 | 70/70 en ambos |

Las métricas se calcularon en las mismas 70 fichas exploratorias; chrF++ mide similitud con las referencias, no veracidad ni calidad comercial. Cambiaron el docente, el tamaño de entrenamiento y el presupuesto de cómputo, de modo que estas cifras no aíslan el efecto del docente. El mixto desarrollado se reanudó tras un límite de tiempo: su mejor punto validado fue la cuarta época y completó una quinta antes de la evaluación.

## Reconstrucción de paquetes

Descarga las tres partes de cada docente en una misma carpeta. En macOS o Linux, une las partes ordenadas con `cat docente_anterior.zip.parte* > docente_anterior.zip` y `cat docente_desarrollado.zip.parte* > docente_desarrollado.zip`; después descomprime los ZIP. En Windows, PowerShell permite combinar las partes en orden como bytes.

SHA-256 del ZIP del docente anterior: `3e34a7cccc3efcf097a7b128a68d40472eb33e012e704e33f76b2dd8649f3894`

SHA-256 del ZIP del docente desarrollado: `bf022a8cfa015a2745b6945def269c386b4adb5f6985c66a4e1a07d48c72b85e`

## Contenido y uso

Cada ZIP contiene `resultados/configuracion.json`, `metricas.json` y un JSON de predicciones por variante. Bajo `resultados/<variante>/mejor/`, `soft.pt` contiene el prompt suave cuando corresponde y `lora/` contiene el adaptador PEFT cuando corresponde. La combinación requiere ambos componentes: el vector suave se incorpora a los embeddings de entrada antes de la instrucción. Los archivos `entrenamiento.json` y `historial.json` documentan cada ejecución; para el mixto desarrollado, el historial abarca la reanudación.

Se necesita el modelo base [Qwen/Qwen3-4B](https://huggingface.co/Qwen/Qwen3-4B), revisión `1cfa9a7208912126459214e8b04321603b3df60c`, y las versiones indicadas en la configuración. El cuaderno de entrega documenta el formato de la instrucción y la aplicación de los vectores suaves y LoRA. Para usar el mixto no basta con cargar únicamente el adaptador LoRA. Los paquetes no permiten reanudar el optimizador: contienen solo lo necesario para recuperar los adaptadores y consultar los resultados.

Los productos son derivados de [Amazon ESCI](https://github.com/amazon-science/esci-data). La [evaluación humana exploratoria](EVALUACION_HUMANA.md) contiene una evaluación anónima de diez productos; no permite estimar acuerdo entre evaluadores.
