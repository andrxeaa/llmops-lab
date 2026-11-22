# llmops-lab

Proyecto base del laboratorio de LLMOps: un clasificador de tickets con evals, gate y monitoreo.

La estructura ya está creada y los archivos están vacíos. La guía indica qué va en cada uno:
[1. llmops-evals-gate-monitoreo.md](https://github.com/utec-devops-2026/laboratorios/blob/main/laboratorio_llmops/1.%20llmops-evals-gate-monitoreo.md)

## Cómo empezar

No trabajes sobre este repositorio: haz **fork** a tu cuenta (botón **Fork**) y clona tu fork.

```bash
git clone https://github.com/<tu-usuario>/llmops-lab.git
cd llmops-lab
```

## Qué archivo se llena en qué paso

| Paso | Archivo | Estado |
|---|---|---|
| 0 | `requirements.txt`, `.gitignore`, `src/__init__.py` | Listos |
| 1.1 | `src/llm.py` | Vacío |
| 1.2 | `config.json`, `src/tracing.py`, `src/app.py` | Vacíos |
| 1.3 | `data/golden.jsonl` | Vacío |
| 1.4 | `tests/test_app.py` | Vacío |
| 2.1 | `prompts/clasificador/v1.md` | Vacío |
| 2.2 | `src/evaluate.py` | Vacío |
| 2.3 | `gate/thresholds.json`, `gate/baseline.json`, `src/gate.py` | Vacíos |
| 2.4 | `prompts/clasificador/v2.md` | Vacío |
| 3.1 | `.github/workflows/llmops.yml` | Lo creas tú |
| 3.2 | `.github/CODEOWNERS` | Vacío |
| 4.1 | `src/traffic.py`, `src/replay.py` | Vacíos |
| 4.2 | `src/monitor.py` | Vacío |
| 4.5 | `.github/workflows/monitoring.yml` | Lo creas tú |

Los dos workflows no vienen creados a propósito: un archivo de workflow vacío hace que GitHub Actions marque un error en cada push.

`data/prod/` se llena al ejecutar `python -m src.traffic` (paso 4.1).

## Respuestas a las preguntas del laboratorio

### Preguntas mandatorias

**¿Por qué el gate rechazó el prompt v1 y qué cambiaste en v2 para que aprobara?**

El v1 fue rechazado porque el LLM inventaba categorías, escribía prosa sin formato JSON e ignoraba los intentos de inyección, fallando las métricas del gate. En v2 se solucionó limitando estrictamente los valores (enum), forzando el formato JSON puro y agregando una instrucción explícita para priorizar la queja real sobre instrucciones inyectadas.

**¿Por qué el pull request trampa falló en GitHub si en tu máquina el gate aprobaba?**

Porque el pipeline CI en GitHub Actions está configurado para extraer el código del juez (`gate.py`, `thresholds.json`, `golden.jsonl`) directamente de la rama `main` protegida, impidiendo que el PR malicioso modifique su propio examen.

**¿Qué te muestra Langfuse que no ves en el archivo de trazas local?**

Langfuse provee una interfaz visual con dashboards agregados para observar la evolución temporal del uso de tokens (costos), latencia y scores. Además, permite inspeccionar las trazas anidadas y crear widgets analíticos dinámicos sin necesidad de manipular manualmente los archivos JSONL.

### Pregunta opcional

**El monitor respondió REVISAR en la semana `price_hike`. ¿Eso significa que el prompt está fallando? ¿Qué dato falta para saberlo?**

No necesariamente significa que el modelo falló, solo indica que la distribución de temas (Data Drift) cambió abruptamente por la subida de precios. Para saber si el prompt falla realmente, necesitamos etiquetas humanas (Ground Truth) de esa nueva semana para poder medir el *accuracy* de las respuestas.
