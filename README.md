[![Licencia](https://img.shields.io/badge/Licencia-Copyright%20%28c%29%202026%20Enrique%20Aguayo-red)](LICENSE)
[![Estado](https://img.shields.io/badge/Estado-Concepto%20en%20diseño-yellow)](https://github.com/enriqueherbertag-lgtm/LegacyModernizer)
[![Asistencia IA](https://img.shields.io/badge/Asistencia%20IA-DeepSeek-brightgreen)](https://deepseek.com)
[![Seguridad](https://img.shields.io/badge/Seguridad-Acceso%20y%20Claves-blue)](./docs/seguridad.md)
[![Shadow Mode](https://img.shields.io/badge/Validación-Shadow%20Mode-blue)](./docs/shadow_mode.md)
[![Cláusula de Control Humano](https://img.shields.io/badge/Control%20Humano-No%20Caja%20Negra-red)](./docs/LEGACYMODERNIZER.md)

# LegacyModernizer: Protocolo de modernización segura de sistemas COBOL en banca

**Un protocolo de bajo riesgo para modernizar sistemas legacy COBOL hacia lenguajes modernos (Python/Go), usando IA local, extracción de reglas de negocio y validación en shadow mode — todo dentro de la intranet del banco.**

Este no es un producto comercial listo para usar. Es un **protocolo de trabajo** diseñado para bancos que necesitan migrar sus sistemas críticos sin detener operaciones ni asumir riesgos inaceptables.

**Filosofía:** El banco nunca se detiene. La modernización ocurre en paralelo. El riesgo se controla con shadow mode y validación humana.

---

## El problema que resuelve

| Problema | Consecuencia |
|----------|-------------|
| Más de 220 mil millones de líneas de COBOL siguen en producción mundial | Sistemas críticos dependen de código antiguo y poco documentado |
| El programador promedio de COBOL tiene ~58 años y ~10% se jubila cada año | Pérdida acelerada de conocimiento institucional |
| Las migraciones "big bang" fallan entre 70% y 80% de los casos | Costos millonarios y disrupción en servicios (ej. casos reales en Europa) |
| Herramientas automáticas tradicionales generan código difícil de mantener | Resultado final muchas veces peor que el sistema original |

**Nuestra solución:** Un protocolo incremental que combina análisis con IA local, extracción de reglas de negocio, traducción asistida y validación en shadow mode, permitiendo una conmutación gradual y segura.

---

## Arquitectura general

Mainframe COBOL → Proxy/Sniffer → Extractor de reglas → Traductor asistido (Python/Go) → Shadow Mode → Conmutación gradual



**Todo el proceso se ejecuta exclusivamente dentro de la intranet del banco**, sin conexión a internet.

---

## Componentes principales

### 1. Modelos de IA locales (offline)

| Modelo                        | Función principal                              | Tamaño aproximado      | Notas |
|-------------------------------|------------------------------------------------|------------------------|-------|
| `cobol-analyzer`              | Extracción de reglas de negocio y dependencias | ~110-350M parámetros   | Fine-tuned con código COBOL |
| `cobol2modern`                | Traducción asistida a Python/Go                | ~350M-7B parámetros    | Cuantizado para ejecución en CPU/GPU local |
| Validador de equivalencia     | Comparación de salidas en shadow mode          | Basado en LLMs locales | Enfoque en precisión |

Los modelos se entrenan/preparan fuera del banco con datos públicos o anonimizados y se instalan localmente.

### 2. Pipeline de modernización (fases)

| Fase | Nombre                    | Descripción                                              | Responsable principal |
|------|---------------------------|----------------------------------------------------------|-----------------------|
| 0    | Preparación               | Instalación de modelos y entorno seguro                  | DevOps + Equipo banco |
| 1    | Escaneo e inventario      | Análisis completo del codebase COBOL                     | Automático            |
| 2    | Extracción de reglas      | Generación de grafo de dependencias y lógica de negocio  | Automático + IA       |
| 3    | Validación humana         | Revisión por programadores veteranos                     | Humano (dashboard)    |
| 4    | Traducción asistida       | Generación de código moderno limpio                      | Automático + IA       |
| 5    | Shadow mode               | Ejecución paralela y comparación de resultados           | Automático            |
| 6    | Corrección e iteración    | Ajuste de discrepancias                                  | Humano + IA           |
| 7    | Conmutación gradual       | Reemplazo módulo por módulo                              | Arquitecto del banco  |
| 8    | Mantenimiento continuo    | Monitoreo y actualización de módulos migrados            | Automático + Humano   |

### 3. Shadow Mode (validación en paralelo)

- Transacción real se procesa en el sistema COBOL original.
- La misma transacción se ejecuta simultáneamente en el código moderno generado.
- Se comparan las salidas automáticamente.
- Las discrepancias se registran para revisión humana.
- **No afecta la producción** en ninguna etapa.

### 4. Respaldo en tiempo real
- Copia de transacciones críticas a base de datos moderna (ej. PostgreSQL + Kafka).
- Soporte en caso de fallos del mainframe.
- Modo solo lectura.

### 5. Protocolo de seguridad para personal externo

| Paso | Acción |
|------|--------|
| 1    | Identificación del técnico al llegar |
| 2    | Verificación de autorización |
| 3    | Activación de temporizador visitante |
| 4    | Entrega de credenciales en sobre sellado |
| 5    | Trabajo restringido al horario autorizado |
| 6    | Caducidad automática de accesos al finalizar |

---

## Requisitos técnicos recomendados

| Recurso          | Especificación recomendada                     | Uso |
|------------------|------------------------------------------------|-----|
| Servidores       | 2-4 servidores (16+ núcleos, 64-128 GB RAM)    | Ejecución de modelos y pipeline |
| Almacenamiento   | 1-2 TB SSD                                     | Código fuente, modelos y logs |
| Red              | Intranet aislada del banco                     | Todo el flujo |
| GPU (opcional)   | 1x NVIDIA T4 o equivalente                     | Acelerar validaciones |

---

## Licencia

Copyright © 2026 Enrique Aguayo. Todos los derechos reservados.

**Permitido:** Uso no comercial con fines educativos, de investigación o pruebas piloto internas.

**Prohibido sin autorización expresa por escrito:** Uso comercial, implementación en producción, modificación para entornos productivos o distribución de versiones derivadas.

Para licencias comerciales, soporte técnico o pilotos en bancos:  
**Contacto:** eaguayo@migst.cl

---

## Autor

**Enrique Aguayo H.**  
Mackiber Labs  
Contacto: eaguayo@migst.cl

*Documentación asistida por DeepSeek (IA).*
