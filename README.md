# Agente inteligente de monitoreo geriátrico remoto
Agente conversacional para el monitoreo clínico remoto de pacientes adultos mayores con hipertensión y diabetes. Combina una arquitectura LangChain (ReAct) con Telegram como interfaz, permitiendo registrar signos vitales, medicación y síntomas, y generar informes clínicos en lenguaje natural.

## Características
- Gestión de pacientes: listar, agregar, seleccionar y eliminar pacientes por instrucciones en lenguaje natural.
- Registro clínico: 9 herramientas (`@tool`) para presión arterial, glucosa, medicamentos, síntomas y perfil clínico.
- Evaluación de riesgo: análisis automatizado del estado de salud y detección de patrones de riesgo.
- Informes automatizados: reportes clínicos completos con gráficos de evolución, pensados para apoyar la teleconsulta.

## Arquitectura
```
Usuario (Telegram)
    │
    ▼
MessageHandler
    │
    ▼
AgentExecutor (LangChain ReAct)
    │  ├── ConversationBufferMemory  (Historial por chat)
    │  └── 9 herramientas @tool      (Acciones clínicas + gestión de pacientes)
    │
    ▼
Respuesta en lenguaje natural → Telegram

Persistencia:
    Usuario/Usuarios/usuario_<user_id>.pkl  ← un archivo por usuario de Telegram
```

## Herramientas del agente

| # | Herramienta | Función |
|---|-------------|---------|
| 1 | `registrar_presion` | Guarda presión arterial con alertas clínicas |
| 2 | `registrar_glucosa` | Guarda glucosa en sangre con clasificación |
| 3 | `registrar_medicamento` | Registra toma de medicamento |
| 4 | `registrar_sintoma` | Registra síntoma con intensidad |
| 5 | `consultar_historial` | Resume presión, glucosa, medicamentos, síntomas |
| 6 | `evaluar_riesgo_clinico` | Evalúa nivel de riesgo global |
| 7 | `generar_informe_medico` | Informe clínico completo + gráfico |
| 8 | `gestionar_pacientes` | Listar, agregar, cambiar, eliminar pacientes |
| 9 | `registrar_perfil` | Registrar perfil del paciente |

## Requisitos
- Python 3.10+
- Un bot de Telegram (token vía [@BotFather](https://t.me/BotFather))
- Una API key compatible con OpenAI (usada a través de [OpenRouter](https://openrouter.ai/))

Instala las dependencias:

```bash
pip install -r requirements.txt
```

## Configuración de credenciales
Este notebook fue desarrollado originalmente en Google Colab, donde las credenciales se leen así:

```python
from google.colab import userdata
TOKEN = userdata.get("TELEGRAM_TOKEN")
OPENROUTER_API_KEY = userdata.get("OPENROUTER_API_KEY")
```

Si vas a ejecutarlo fuera de Colab (Jupyter local, servidor, etc.), reemplaza esa lectura por variables de entorno, por ejemplo:

```python
import os
TOKEN = os.environ.get("TELEGRAM_TOKEN")
OPENROUTER_API_KEY = os.environ.get("OPENROUTER_API_KEY")
```

Y define las variables antes de correr el notebook:

```bash
export TELEGRAM_TOKEN="tu_token_de_telegram"
export OPENROUTER_API_KEY="tu_api_key"
```

⚠️ Nunca subas tus tokens o API keys al repositorio. Usa variables de entorno, un archivo `.env` (agregado a `.gitignore`) o los secretos de Colab.

## Uso
1. Abre `Agente_inteligente_de_monitoreo_geriatrico_remoto.ipynb` en Jupyter o Google Colab.
2. Configura las credenciales como se indica arriba.
3. Ejecuta las celdas en orden.
4. Interactúa con el agente desde Telegram o mediante el bucle conversacional local incluido en el notebook.

## Persistencia de datos
Los datos de cada paciente se almacenan localmente en archivos `.pkl` bajo `Usuario/Usuarios/usuario_<user_id>.pkl`, uno por usuario de Telegram.

## Aviso
Este proyecto es un prototipo educativo/demostrativo y no reemplaza el criterio médico profesional. No debe usarse como única fuente para decisiones clínicas reales.
