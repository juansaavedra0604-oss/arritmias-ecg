# Detección de arritmias en ECG con redes neuronales

Proyecto de Inteligencia Computacional 2 · Universidad Distrital Francisco José de Caldas

- Julián Esteban Rodríguez Castillo
- Juan Ángel Saavedra Ávila

## Problema

Clasificar cada latido del electrocardiograma en tres clases del estándar AAMI:

| Clase | Significado |
| --- | --- |
| N | Latido normal |
| S | Latido supraventricular (forma casi normal, pero llega antes) |
| V | Latido ventricular (QRS ancho, sin onda P) |

La evaluación es **inter-paciente**: los pacientes de prueba nunca se usan para entrenar (división DS1/DS2 de de Chazal et al., 2004).

## Plan

| Entrega | Modelos | Aporte |
| --- | --- | --- |
| 1 (semana 5) | Adaline con pseudoinversa (línea base lineal), SOM, red RBF | Mostrar que el problema no es linealmente separable |
| 2 (semana 10) | CNN 1D con weight decay y dropout | Podar la CNN con Optimal Brain Damage sin perder F1 |

## Datos

[MIT-BIH Arrhythmia Database](https://physionet.org/content/mitdb/1.0.0/) (PhysioNet): 48 registros de 30 minutos, 360 Hz, unos 110.000 latidos anotados. Se excluyen los 4 registros con marcapasos (102, 104, 107, 217).

Los datos **no se suben al repositorio**: el cuaderno los descarga solo.

| Conjunto | N | S | V | Total |
| --- | --- | --- | --- | --- |
| DS1 (entrenar) | 45.824 | 943 | 3.788 | 50.555 |
| DS2 (probar) | 44.218 | 1.836 | 3.219 | 49.273 |

Los conteos coinciden con los publicados con menos de 0,1 % de diferencia (se omiten el primer y el último latido de cada registro).

## Resultados de la entrega 1 (DS2, pacientes nunca vistos)

| Modelo | S: Se / PP | V: Se / PP | F1 macro |
| --- | --- | --- | --- |
| Adaline (pseudoinversa) | 76,7 % / 35,5 % | 92,6 % / 63,4 % | 0,729 |
| RBF (50 centros por clase) | 52,6 % / 30,0 % | 84,4 % / 53,4 % | 0,663 ± 0,011 |
| de Chazal 2004 (referencia) | 75,9 % / 38,5 % | 77,7 % / 81,9 % | — |

- El problema no es linealmente separable: en el SOM, el 41 % de los latidos S cae en neuronas dominadas por otra clase.
- La RBF gana en la validación cruzada sobre DS1, pero pierde en DS2: sus neuronas locales memorizan a los pacientes de entrenamiento. Por eso la entrega 2 usa una CNN 1D regularizada.

## Cómo correrlo

1. Abrir `notebooks/fase0_mitbih.ipynb` en Google Colab (*Archivo → Abrir cuaderno → GitHub*) y ejecutar todo. Descarga MIT-BIH (~100 MB), procesa los latidos y guarda `mitbih_ds1_ds2.npz`.
2. Abrir `notebooks/fase2_modelos.ipynb` en la misma sesión y ejecutar todo (~10 min). Si no encuentra el `.npz`, corre la fase 0 por su cuenta.
3. Al final de la fase 2 está la **demostración** (sección 7b): toma un paciente de DS2, detecta los picos R automáticamente (sin usar las anotaciones), clasifica sus latidos y grafica un tramo con la etiqueta real y la predicha. Para el paciente 200: 99,9 % de latidos detectados y 90,9 % bien clasificados.

## Estructura

```
notebooks/   cuadernos de cada fase
figuras/     figuras para los informes
data/        datos locales (ignorados por git)
```

## Estado

- [x] Fase 0: entorno, lectura de MIT-BIH, resumen de papers
- [x] Fase 1 (adelantada): filtrado, segmentación, características RR, división DS1/DS2
- [x] Fase 2: Adaline, SOM y RBF
- [ ] Fase 3: CNN 1D
- [ ] Fase 4: poda con OBD

## Referencias

- P. de Chazal, M. O'Dwyer y R. B. Reilly, "Automatic classification of heartbeats using ECG morphology and heartbeat interval features", *IEEE Trans. Biomed. Eng.*, 2004.
- S. Kiranyaz, T. Ince y M. Gabbouj, "Real-time patient-specific ECG classification by 1-D convolutional neural networks", *IEEE Trans. Biomed. Eng.*, vol. 63, n.º 3, pp. 664–675, 2016.
- Y. LeCun, J. S. Denker y S. A. Solla, "Optimal Brain Damage", *Advances in Neural Information Processing Systems 2*, 1990.
- Revisión sistemática de clasificación de arritmias, arXiv:2503.07276, 2025.
