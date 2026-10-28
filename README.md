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

## Cómo correrlo

1. Abrir `notebooks/fase0_mitbih.ipynb` en Google Colab (*Archivo → Abrir cuaderno → GitHub*).
2. *Entorno de ejecución → Ejecutar todas*. Descarga MIT-BIH (~100 MB), procesa los latidos y guarda `mitbih_ds1_ds2.npz`.

## Estructura

```
notebooks/   cuadernos de cada fase
figuras/     figuras para los informes
data/        datos locales (ignorados por git)
```

## Estado

- [x] Fase 0: entorno, lectura de MIT-BIH, resumen de papers
- [x] Fase 1 (adelantada): filtrado, segmentación, características RR, división DS1/DS2
- [ ] Fase 2: Adaline, SOM y RBF
- [ ] Fase 3: CNN 1D
- [ ] Fase 4: poda con OBD

## Referencias

- P. de Chazal, M. O'Dwyer y R. B. Reilly, "Automatic classification of heartbeats using ECG morphology and heartbeat interval features", *IEEE Trans. Biomed. Eng.*, 2004.
- S. Kiranyaz, T. Ince y M. Gabbouj, "Real-time patient-specific ECG classification by 1-D convolutional neural networks", *IEEE Trans. Biomed. Eng.*, vol. 63, n.º 3, pp. 664–675, 2016.
- Y. LeCun, J. S. Denker y S. A. Solla, "Optimal Brain Damage", *Advances in Neural Information Processing Systems 2*, 1990.
- Revisión sistemática de clasificación de arritmias, arXiv:2503.07276, 2025.
