# Protocolo de acceso para técnicos externos

## Principios

- Acceso por tiempo limitado (solo horas laborales).
- Claves de un solo uso, entregadas en sobre cerrado cada día.
- Trazabilidad total: quién, cuándo, desde dónde, qué hizo.

## Pasos

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

## Implementación técnica (generador de claves)

```python
import secrets
from datetime import date

def generar_clave():
    clave = secrets.token_urlsafe(8)
    fecha = date.today().isoformat()
    print(f"Clave: {clave} | Valida hasta: {fecha} 23:59")
    return clave
text

---

## 🧪 3. `docs/shadow_mode.md`

```markdown
# Shadow Mode: Validación continua

## Concepto

El shadow mode ejecuta el código traducido (Python/Go) en paralelo con el COBOL original, sin afectar la producción. Compara las salidas y registra discrepancias.

## Flujo
Transacción real → [COBOL original] → Salida_COBOL
→ [Código generado] → Salida_Python
→ [Comparador] → ¿Coinciden?
↓
Sí: Registrar éxito
No: Registrar error + guardar inputs/outputs

text

## Pseudocódigo (Python)

```python
def shadow_mode(input_data, cobol_func, python_func):
    output_cobol = cobol_func(input_data)
    output_python = python_func(input_data)
    
    if output_cobol == output_python:
        log("OK", input_data, output_cobol)
    else:
        log("ERROR", input_data, output_cobol, output_python)
        alert("Diferencia detectada")
Requisitos
El código traducido debe ser funcionalmente idéntico (no necesariamente sintácticamente igual).

Las comparaciones deben ser tolerantes a formatos (ej. ignorar espacios, normalizar decimales).

Los logs deben almacenarse en una base de datos (PostgreSQL) para auditoría.

text

---

## 🏷️ Badge para llamar a `LEGACYMODERNIZER.md` desde el README

En tu `README.md` (en la sección de documentación o al inicio), agrega:

```markdown
[![Cláusula de Migración Supervisada](https://img.shields.io/badge/Cláusula-Migración%20Supervisada-blue)](./LEGACYMODERNIZER.md)

