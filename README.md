[![Licencia](https://img.shields.io/badge/Licencia-Copyright%20%28c%29%202026%20Enrique%20Aguayo-red)](LICENSE)
[![Estado](https://img.shields.io/badge/Estado-Concepto%20en%20diseño-yellow)](https://github.com/enriqueherbertag-lgtm/LegacyModernizer)
[![Asistencia IA](https://img.shields.io/badge/Asistencia%20IA-DeepSeek-brightgreen)](https://deepseek.com)

# LegacyModernizer: Protocolo de modernización de COBOL en bancos

**Un sistema que aprende COBOL en tiempo real, traduce a lenguajes modernos (Python/Go), y valida en shadow mode, todo dentro de la intranet del banco.**

Este proyecto no es un software comercial. Es un **protocolo de trabajo** que combina:
- Modelos de lenguaje pequeños (SLMs) entrenados específicamente para COBOL.
- Un pipeline de extracción, traducción, validación y conmutación.
- Un sistema de seguridad basado en claves de un solo uso y temporizador visitante.
- Una metodología de modernización en caliente (sin apagar el mainframe).

**Filosofía:** El banco no para. El código se traduce en paralelo. El riesgo se mitiga con shadow mode.

---

##  El problema que resuelve

| Problema | Consecuencia |
|:---|:---|
| El código COBOL tiene décadas, sin documentación, sin dueño. | Nadie sabe cómo funciona realmente. |
| Los programadores veteranos se jubilan. | El conocimiento se pierde. |
| Migrar a lenguajes modernos es caro y arriesgado. | Los bancos posponen la migración hasta que es tarde. |
| Las herramientas comerciales traducen mal (código espagueti). | El resultado es peor que el original. |

**Nuestra solución:** Un protocolo que aprende del COBOL mientras opera, traduce a código limpio, y valida cada transacción antes de conmutar.

---

##  Arquitectura general
[Mainframe COBOL] → [Proxy/Sniffer] → [Extractor de reglas] → [Traductor a Python/Go] → [Shadow Mode] → [Conmutación gradual]
↓ ↓ ↓ ↓
[Respaldo en tiempo real] [Reglas validadas] [Código generado] [Módulo migrado]

text

**Todos los componentes corren dentro de la intranet del banco, sin acceso a internet.**

---

##  Componentes del sistema

### 1. Modelos de IA locales (sin internet)

| Modelo | Función | Tamaño | Entrenamiento |
|:---|:---|:---|:---|
| `cobol-bert` | Extraer reglas de negocio, dependencias, flujos de datos. | 110M params | Fine-tuned desde CodeBERT con código COBOL etiquetado. |
| `cobol2py` | Traducir sentencias COBOL a Python/Go. | 350M params | Fine-tuned desde CodeT5 con pares (COBOL, Python). |
| `code-llama-7b` (cuantizado) | Validar equivalencia de salidas en shadow mode. | 7B params (4 bits) | Cuantizado para correr en CPU. |

**Los modelos se entrenan fuera del banco (con datos públicos) y se instalan localmente en sus servidores.**

### 2. Pipeline de modernización (8 fases)

| Fase | Nombre | Qué hace | Automatización |
|:---|:---|:---|:---|
| 0 | Preparación | Instalar modelos y entorno en servidores del banco. | DevOps + Tú |
| 1 | Escaneo inicial | Analizar todo el código COBOL del banco. | Automático |
| 2 | Extracción de reglas | Generar grafo de dependencias y reglas de negocio. | Automático |
| 3 | Validación humana | Programador veterano revisa y corrige reglas extraídas. | Humano (dashboard) |
| 4 | Traducción automática | Generar código Python/Go desde reglas validadas. | Automático |
| 5 | Shadow mode | Ejecutar código nuevo en paralelo, comparar salidas. | Automático |
| 6 | Corrección humana | Revisar discrepancias, corregir código o modelo. | Humano + Automático |
| 7 | Conmutación | Reemplazar COBOL por código nuevo (gradual). | Arquitecto del banco |
| 8 | Mantenimiento continuo | Repetir ciclo para nuevos módulos o cambios en COBOL. | Automático + Humano |

### 3. Shadow mode (validación continua)
Transacción real → [COBOL original] → Salida_COBOL
→ [Código generado] → Salida_Python
→ [Comparador] → ¿Coinciden?
↓
Sí: Registrar éxito
No: Registrar error + guardar inputs/outputs

text

**El shadow mode corre en paralelo con el sistema real, sin afectar la producción.**

### 4. Respaldo en tiempo real

- Cada transacción que pasa por el mainframe se copia a una base de datos moderna (PostgreSQL, Kafka, etc.).
- Si el mainframe falla, los datos no se pierden.
- El respaldo es **solo lectura** (no se escribe en el mainframe).

### 5. Seguridad: Protocolo de acceso para técnicos externos

| Paso | Acción |
|:---|:---|
| 1 | El técnico llega al banco. |
| 2 | Se identifica (cédula, credencial, código). |
| 3 | Confirma su autorización en la lista de personal externo. |
| 4 | Se activa el temporizador visitante (hora de inicio). |
| 5 | Se entrega un sobre cerrado con las claves de administrador. |
| 6 | El técnico trabaja (solo durante el horario autorizado). |
| 7 | Al terminar, el temporizador se desactiva y las claves caducan. |
| 8 | Al día siguiente, se repite el proceso con un nuevo sobre. |

**El técnico nunca tiene acceso fuera de su horario. El banco nunca pierde el control.**

---

## 🛠️ Implementación técnica (resumen)

### Requisitos del banco (infraestructura)

| Recurso | Especificación | Uso |
|:---|:---|:---|
| Servidores (2-4) | CPU modernas (16+ núcleos), 64-128 GB RAM | Correr modelos y pipeline |
| Almacenamiento | 1-2 TB SSD | Código COBOL, modelos, logs |
| Red | Intranet del banco | Acceso a código y transacciones de prueba |
| Opcional (pero recomendado) | 1 GPU (NVIDIA T4 o similar) | Acelerar shadow mode |

### Instalación (una sola vez)

```bash
# Copiar modelos a los servidores (sin internet)
scp ./cobol-bert-local user@server:/bank/models/
scp ./cobol2py-local user@server:/bank/models/
scp ./codellama-7b.Q4_K_M.gguf user@server:/bank/models/

# Instalar dependencias (sin internet, usando paquetes locales)
pip install --no-index --find-links ./offline-packages transformers torch

# Configurar acceso al repositorio COBOL
./setup.sh --cobol-path /bank/cobol/legacy --output /bank/output
Uso diario (automático)
bash
# Escaneo nocturno (cron job)
0 2 * * * /bank/scripts/scanner.sh --incremental

# Shadow mode continuo (servicio)
systemctl start shadow-mode.service

# Dashboard de validación humana (acceso interno)
https://bank-intranet/legacy-modernizer/dashboard

## Licencia

Copyright © 2026 Enrique Aguayo. Todos los derechos reservados.

Este proyecto está protegido por derechos de autor.

PERMITIDO:
- Uso no comercial con fines educativos o de investigación.
- Distribución sin modificación, siempre que se mantenga esta licencia y se dé crédito al autor.

PROHIBIDO sin autorización expresa por escrito:
- Uso comercial (incluyendo, pero no limitado a: ofrecerlo como servicio, SaaS, suscripción, integración en productos que generen ingresos, o cualquier uso que genere beneficio económico directo o indirecto).
- Modificación para entornos de producción.
- Distribución de versiones modificadas sin autorización.

Para licencias comerciales, soporte técnico, pilotos empresariales o consultas:
Contacto: eaguayo@migst.cl

Cualquier uso fuera de los términos permitidos requiere permiso previo del autor.

Las consultas comerciales son bienvenidas y se responderán en un plazo máximo de 7 días hábiles.

## Autor

Enrique Aguayo H.
Mackiber Labs
Contacto: eaguayo@migst.cl
ORCID: 0009-0004-4615-6825
GitHub: @enriqueherbertag-lgtm

Documentación asistida por Ana (DeepSeek), IA para investigación y optimización técnica.

