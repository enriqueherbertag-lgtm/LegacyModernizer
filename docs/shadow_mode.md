# Shadow Mode: Validación Continua

## Concepto

El **Shadow Mode** es el mecanismo de validación más importante de LegacyModernizer.  
Permite ejecutar el código moderno (Python/Go) **en paralelo** con el sistema COBOL original **sin afectar la producción**, comparando las salidas en tiempo real y detectando discrepancias antes de hacer la conmutación.

---

## Diagrama del Flujo

```mermaid
flowchart LR
    A[Transacción Real] --> B[COBOL Original]
    A --> C[Código Moderno Python-Go]
    B --> D[Salida COBOL]
    C --> E[Salida Python-Go]
    D & E --> F[Comparador]
    F --> G[¿Coinciden?]
    G -->|Sí| H[Registrar Éxito]
    G -->|No| I[Registrar Error + Guardar Datos]
    I --> J[Alertar al Encargado]


Pasos detallados:Una transacción real llega al sistema.
Se ejecuta simultáneamente en:El código COBOL original (sistema en producción)
El código moderno recién traducido

Se capturan ambas salidas.
El comparador analiza si los resultados son equivalentes.
Si coinciden → se registra como éxito.
Si hay diferencias → se guarda el input completo, ambas salidas y se genera una alerta para revisión humana.

Pseudocódigo (Python)

def shadow_mode(input_data: dict, cobol_function, modern_function):
    """
    Ejecuta shadow mode y compara resultados.
    
    Args:
        input_data: Datos de entrada de la transacción
        cobol_function: Función que llama al sistema COBOL legacy
        modern_function: Función del código moderno (Python/Go)
    """
    try:
        # Ejecución en paralelo
        output_cobol = cobol_function(input_data)
        output_modern = modern_function(input_data)
        
        # Comparación tolerante
        if are_outputs_equal(output_cobol, output_modern):
            log_success(input_data, output_cobol, output_modern)
            return True
        else:
            log_discrepancy(input_data, output_cobol, output_modern)
            alert_engineer("Discrepancia detectada en shadow mode")
            return False
            
    except Exception as e:
        log_error(input_data, str(e))
        alert_engineer(f"Error en shadow mode: {e}")
        return False
end

## Requisitos Técnicos

El código moderno debe ser funcionalmente idéntico (no necesariamente idéntico en sintaxis).
Las comparaciones deben ser tolerantes a formato:Ignorar diferencias de espacios o mayúsculas/mayúsculas
Normalizar números decimales y formatos de fecha
Comparar estructuras de datos complejas (JSON, registros, etc.)

Todos los logs y discrepancias deben guardarse en PostgreSQL para auditoría posterior.
El sistema debe soportar alta concurrencia (miles de transacciones por minuto).

## Ventajas del Shadow

ModePermite validar el nuevo código sin riesgo para la operación del banco.
Acumula evidencia estadística de confiabilidad antes de la conmutación.
Facilita la detección temprana de errores de traducción o lógica.
Cumple con requisitos de auditoría y compliance bancario.

## Notas Importantes

El shadow mode no afecta el rendimiento del sistema principal.
Debe mantenerse activo durante varias semanas o meses según el módulo antes de proceder a la conmutación.
Toda discrepancia debe ser revisada y resuelta por un programador veterano de COBOL + el equipo de modernización.




