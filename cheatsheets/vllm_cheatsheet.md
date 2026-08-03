# vLLM Cheatsheet

Guía rápida de vLLM organizada por secciones, pensada para despliegue, serving, benchmarking y uso diario en entornos de inferencia local o servidores. Incluye CLI, ejemplos de serving OpenAI-compatible, batch, benchmark y varios flags prácticos.

## Índice

- [1. Qué es vLLM](#1-qué-es-vllm)
- [2. Instalación rápida](#2-instalación-rápida)
- [3. Comandos principales CLI](#3-comandos-principales-cli)
- [4. Serving básico](#4-serving-básico)
- [5. Ayuda avanzada de flags](#5-ayuda-avanzada-de-flags)
- [6. Flags útiles de serving](#6-flags-útiles-de-serving)
- [7. Multi-GPU](#7-multi-gpu)
- [8. Chat y completions desde CLI](#8-chat-y-completions-desde-cli)
- [9. API OpenAI-compatible](#9-api-openai-compatible)
- [10. Cliente OpenAI en Python](#10-cliente-openai-en-python)
- [11. Parámetros extra no OpenAI](#11-parámetros-extra-no-openai)
- [12. Request IDs](#12-request-ids)
- [13. Benchmarks](#13-benchmarks)
- [14. Inferencia batch](#14-inferencia-batch)
- [15. Offline inference en Python](#15-offline-inference-en-python)
- [16. Chat template y generación](#16-chat-template-y-generación)
- [17. Variables y ajustes útiles](#17-variables-y-ajustes-útiles)
- [18. Backends de atención](#18-backends-de-atención)
- [19. Enteros legibles](#19-enteros-legibles)
- [20. Comandos que conviene memorizar primero](#20-comandos-que-conviene-memorizar-primero)
- [21. Buenas prácticas rápidas](#21-buenas-prácticas-rápidas)

## 1. Qué es vLLM

vLLM es un motor de inferencia y serving para LLMs con API compatible con OpenAI, CLI propia y opciones para serving online, inferencia batch y benchmarking.

[↑ ir al índice](#índice)

## 2. Instalación rápida

### Requisitos base

- Linux como sistema operativo principal.
- Python 3.10 a 3.13 en la guía actual de quickstart.

### Instalación con `uv`

```bash
uv venv --python 3.12 --seed
source .venv/bin/activate
uv pip install vllm --torch-backend=auto
```

Este método permite que `uv` seleccione automáticamente el backend de PyTorch según el driver CUDA detectado.

### Ejecución puntual sin entorno persistente

```bash
uv run --with vllm vllm --help
```

También se puede invocar vLLM sin crear un entorno permanente usando `uv run --with vllm`.

[↑ ir al índice](#índice)

## 3. Comandos principales CLI

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `vllm --help` | Muestra ayuda general | `vllm --help` |
| `vllm serve` | Arranca servidor OpenAI-compatible | `vllm serve Qwen/Qwen2.5-1.5B-Instruct` |
| `vllm chat` | Chat contra servidor ya levantado | `vllm chat --url http://localhost:8000/v1` |
| `vllm complete` | Completions contra servidor | `vllm complete --url http://localhost:8000/v1` |
| `vllm bench latency` | Benchmark de latencia | `vllm bench latency --model meta-llama/Llama-3.2-1B-Instruct --input-len 32 --output-len 1 --load-format dummy` |
| `vllm bench serve` | Benchmark de throughput online | `vllm bench serve --model meta-llama/Llama-3.2-1B-Instruct --host localhost --port 8000 --random-input-len 32 --random-output-len 4 --num-prompts 5` |
| `vllm bench throughput` | Benchmark de inferencia offline | `vllm bench throughput --model meta-llama/Llama-3.2-1B-Instruct --input-len 32 --output-len 1 --load-format dummy` |
| `vllm collect-env` | Recoge info del entorno | `vllm collect-env` |
| `vllm run-batch` | Ejecuta lotes y guarda salida | `vllm run-batch -i input.jsonl -o results.jsonl --model meta-llama/Meta-Llama-3-8B-Instruct` |
| `vllm launch` | Lanza componentes individuales | `vllm launch render meta-llama/Llama-3.2-1B-Instruct` |

La CLI de vLLM expone actualmente los subcomandos `chat`, `complete`, `serve`, `launch`, `bench`, `collect-env` y `run-batch`.

[↑ ir al índice](#índice)

## 4. Serving básico

### Arranque mínimo

```bash
vllm serve Qwen/Qwen2.5-1.5B-Instruct
```

vLLM levanta por defecto el servidor en `http://localhost:8000` y sirve un solo modelo a la vez.

### Cambiar host o puerto

```bash
vllm serve Qwen/Qwen2.5-1.5B-Instruct --host 0.0.0.0 --port 8001
```

Se puede cambiar la dirección de escucha con `--host` y `--port`.

### Socket Unix

```bash
vllm serve meta-llama/Llama-2-7b-hf --uds /tmp/vllm.sock
```

La CLI soporta servir mediante Unix domain socket usando `--uds`.

### API key

```bash
vllm serve NousResearch/Meta-Llama-3-8B-Instruct --dtype auto --api-key token-abc123
```

El servidor puede validar claves API pasadas con `--api-key` o mediante la variable `VLLM_API_KEY`.

[↑ ir al índice](#índice)

## 5. Ayuda avanzada de flags

```bash
vllm serve --help
vllm serve --help=all
vllm serve --help=ModelConfig
vllm serve --help=max-num-seqs
vllm serve --help=max
```

La ayuda del subcomando `serve` permite listar todos los flags, filtrar por grupos de argumentos o buscar una opción concreta.

[↑ ir al índice](#índice)

## 6. Flags útiles de serving

| Flag | Uso | Ejemplo |
|---|---|---|
| `--host` | IP o interfaz de escucha | `--host 0.0.0.0` |
| `--port` | Puerto HTTP | `--port 8000` |
| `--uds` | Socket Unix | `--uds /tmp/vllm.sock` |
| `--api-key` | Protege el endpoint | `--api-key token-abc123` |
| `--generation-config vllm` | Ignora `generation_config.json` del repositorio | `--generation-config vllm` |
| `--attention-backend FLASH_ATTN` | Fuerza backend de atención | `--attention-backend FLASH_ATTN` |
| `--tensor-parallel-size` | Paralelismo tensorial multi-GPU | `--tensor-parallel-size 4` |
| `--enable-request-id-headers` | Habilita cabecera `X-Request-Id` | `--enable-request-id-headers` |

vLLM aplica por defecto el `generation_config.json` del repositorio del modelo si existe, y ese comportamiento puede desactivarse con `--generation-config vllm`.

[↑ ir al índice](#índice)

## 7. Multi-GPU

```bash
vllm serve meta-llama/Llama-3.1-8B-Instruct --tensor-parallel-size 4
```

Para serving en varias GPU se puede usar `--tensor-parallel-size` al arrancar el servidor.

[↑ ir al índice](#índice)

## 8. Chat y completions desde CLI

### Chat rápido

```bash
vllm chat --quick "hola"
vllm chat --url http://localhost:8000/v1 --stats
```

`vllm chat` puede conectarse al servidor local sin parámetros o a una URL específica, y también mostrar métricas como TTFT y throughput con `--stats`.

### Completion rápida

```bash
vllm complete --quick "The future of AI is"
vllm complete --url http://localhost:8000/v1 --stats
```

`vllm complete` sirve para probar completions de forma rápida contra un servidor vLLM ya arrancado.

[↑ ir al índice](#índice)

## 9. API OpenAI-compatible

### Listar modelos

```bash
curl http://localhost:8000/v1/models
```

El servidor implementa endpoints compatibles con OpenAI, incluido `/v1/models`.

### Completion HTTP

```bash
curl http://localhost:8000/v1/completions   -H "Content-Type: application/json"   -d '{
    "model": "Qwen/Qwen2.5-1.5B-Instruct",
    "prompt": "San Francisco is a",
    "max_tokens": 7,
    "temperature": 0
  }'
```

vLLM soporta el endpoint `/v1/completions` para modelos de generación de texto.

### Chat HTTP

```bash
curl http://localhost:8000/v1/chat/completions   -H "Content-Type: application/json"   -d '{
    "model": "Qwen/Qwen2.5-1.5B-Instruct",
    "messages": [
      {"role": "system", "content": "You are a helpful assistant."},
      {"role": "user", "content": "Tell me a joke."}
    ]
  }'
```

También soporta `/v1/chat/completions` para modelos con chat template.

[↑ ir al índice](#índice)

## 10. Cliente OpenAI en Python

```python
from openai import OpenAI

client = OpenAI(
    api_key="EMPTY",
    base_url="http://localhost:8000/v1",
)

resp = client.chat.completions.create(
    model="Qwen/Qwen2.5-1.5B-Instruct",
    messages=[{"role": "user", "content": "Hola"}],
)

print(resp.choices[0].message)
```

Como el servidor es compatible con OpenAI, puede usarse como reemplazo directo del cliente oficial cambiando `base_url` y `api_key`.

[↑ ir al índice](#índice)

## 11. Parámetros extra no OpenAI

```python
resp = client.chat.completions.create(
    model="NousResearch/Meta-Llama-3-8B-Instruct",
    messages=[{"role": "user", "content": "Classify this sentiment: vLLM is wonderful!"}],
    extra_body={"top_k": 50}
)
```

vLLM admite parámetros adicionales no estándar de OpenAI, como `top_k`, usando `extra_body` en el cliente o añadiéndolos directamente al JSON HTTP.

[↑ ir al índice](#índice)

## 12. Request IDs

```bash
vllm serve Qwen/Qwen2.5-1.5B-Instruct --enable-request-id-headers
```

Si se habilita `--enable-request-id-headers`, el servidor acepta la cabecera `X-Request-Id`.

[↑ ir al índice](#índice)

## 13. Benchmarks

### Dependencia extra

```bash
pip install vllm[bench]
```

Los subcomandos de benchmark requieren instalar dependencias extra mediante `vllm[bench]`.

### Latencia

```bash
vllm bench latency   --model meta-llama/Llama-3.2-1B-Instruct   --input-len 32   --output-len 1   --enforce-eager   --load-format dummy
```

`vllm bench latency` mide latencia de un batch individual de peticiones.

### Throughput online

```bash
vllm bench serve   --model meta-llama/Llama-3.2-1B-Instruct   --host localhost   --port 8000   --random-input-len 32   --random-output-len 4   --num-prompts 5
```

`vllm bench serve` sirve para medir throughput del serving online.

### Throughput offline

```bash
vllm bench throughput   --model meta-llama/Llama-3.2-1B-Instruct   --input-len 32   --output-len 1   --enforce-eager   --load-format dummy
```

`vllm bench throughput` mide el rendimiento de inferencia offline.

[↑ ir al índice](#índice)

## 14. Inferencia batch

```bash
vllm run-batch   -i input.jsonl   -o results.jsonl   --model meta-llama/Meta-Llama-3-8B-Instruct
```

`run-batch` permite ejecutar prompts por lotes desde un fichero local o remoto y escribir los resultados a un archivo.

[↑ ir al índice](#índice)

## 15. Offline inference en Python

```python
from vllm import LLM, SamplingParams

prompts = [
    "Hello, my name is",
    "The future of AI is",
]

sampling_params = SamplingParams(temperature=0.8, top_p=0.95)
llm = LLM(model="facebook/opt-125m")
outputs = llm.generate(prompts, sampling_params)
```

Para inferencia offline, la documentación usa `LLM` como clase principal y `SamplingParams` para los parámetros de generación.

[↑ ir al índice](#índice)

## 16. Chat template y generación

La guía indica que `llm.generate` no aplica automáticamente el chat template del modelo, por lo que en modelos instruct o chat conviene aplicar el template manualmente o usar `llm.chat`.

[↑ ir al índice](#índice)

## 17. Variables y ajustes útiles

| Variable / ajuste | Qué hace | Ejemplo |
|---|---|---|
| `VLLM_API_KEY` | Define claves API aceptadas por el servidor | `export VLLM_API_KEY=token-abc123` |
| `VLLM_USE_MODELSCOPE=True` | Usa ModelScope en lugar de Hugging Face para descargar modelos | `export VLLM_USE_MODELSCOPE=True` |
| `--generation-config vllm` | Fuerza sampling por defecto de vLLM | `vllm serve modelo --generation-config vllm` |

La documentación indica que vLLM descarga modelos desde Hugging Face por defecto y puede cambiarse a ModelScope con `VLLM_USE_MODELSCOPE=True`.

[↑ ir al índice](#índice)

## 18. Backends de atención

```bash
vllm serve Qwen/Qwen2.5-1.5B-Instruct --attention-backend FLASH_ATTN
```

vLLM soporta múltiples backends de atención y permite fijarlos manualmente con `--attention-backend`.

[↑ ir al índice](#índice)

## 19. Enteros legibles

Algunos flags aceptan sufijos legibles como `1k`, `1K`, `1m`, `1M`, `1g` o `1G`, por ejemplo en `--max-model-len`, `--max-num-batched-tokens` o `--kv-cache-memory-bytes`.

[↑ ir al índice](#índice)

## 20. Comandos que conviene memorizar primero

```bash
vllm --help
vllm serve Qwen/Qwen2.5-1.5B-Instruct
vllm serve Qwen/Qwen2.5-1.5B-Instruct --host 0.0.0.0 --port 8000
vllm serve Qwen/Qwen2.5-1.5B-Instruct --api-key token-abc123
vllm serve Qwen/Qwen2.5-1.5B-Instruct --generation-config vllm
vllm chat --quick "hola"
vllm complete --quick "The future of AI is"
curl http://localhost:8000/v1/models
vllm bench latency --model meta-llama/Llama-3.2-1B-Instruct --input-len 32 --output-len 1 --load-format dummy
vllm bench serve --model meta-llama/Llama-3.2-1B-Instruct --host localhost --port 8000 --random-input-len 32 --random-output-len 4 --num-prompts 5
vllm collect-env
vllm run-batch -i input.jsonl -o results.jsonl --model meta-llama/Meta-Llama-3-8B-Instruct
```

[↑ ir al índice](#índice)

## 21. Buenas prácticas rápidas

- Usa `vllm serve --help=all` antes de tocar flags menos comunes.
- Si quieres resultados coherentes con defaults propios de vLLM, desactiva el `generation_config.json` del repositorio con `--generation-config vllm`.
- Para benchmark, instala `vllm[bench]` y separa latencia, serving y throughput offline según el caso.
- Para clientes existentes con SDK OpenAI, normalmente basta con cambiar `base_url` y apuntarlo al endpoint de vLLM.
