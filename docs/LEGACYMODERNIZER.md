## 9. Cláusula de Migración Supervisada (Obligatoria)

Una vez clonado el sistema (traducido y validado en shadow mode), el proceso de conmutación se ejecuta **en paralelo y bajo supervisión humana continua**:

1. **Encargado de sistemas presente**: Durante toda la fase de migración (desde la primera ejecución en paralelo hasta la desactivación del COBOL), un responsable técnico designado por el banco debe estar físicamente presente o conectado en tiempo real.

2. **Migración supervisada**: El sistema nuevo corre en paralelo con el legacy. El encargado revisa los logs, las alertas, y las comparaciones de salida. Solo cuando el encargado lo autoriza, se conmuta un módulo.

3. **Derecho a reversión**: En cualquier momento, el encargado puede detener el proceso y revertir al sistema original sin pérdida de datos.

4. **Fin del acompañamiento**: El proceso se considera terminado cuando el banco, representado por su encargado, firma la conformidad de migración. Hasta ese día, el acompañamiento es obligatorio.

## Requisitos de Infraestructura


### Sistema Operativo (Servidor Local)

El banco puede elegir cualquiera de las siguientes distribuciones Linux, según su política interna, presupuesto y familiaridad técnica:

| Distribución | Perfil | Ventaja para LegacyModernizer |
|:---|:---|:---|
| **Ubuntu Server (LTS)** | Propósito general, gran comunidad | Ideal para prototipos y bancos pequeños. Fácil de instalar y documentar. |
| **Debian** | Estabilidad extrema | Para bancos que priorizan "que no se mueva nada" sobre versiones recientes. |
| **Red Hat Enterprise Linux (RHEL)** | Corporativo, con soporte pagado | La opción segura para bancos grandes. Cumple estándares de cumplimiento (PCI, etc.). |
| **AlmaLinux / Rocky Linux** | Alternativa gratuita a RHEL | Misma compatibilidad que RHEL, sin costo de licencia. Ideal para bancos que quieren estabilidad empresarial sin pagar suscripción. |
| **SUSE Linux Enterprise Server (SLES)** | Entornos críticos con soporte profesional | Similar a RHEL, pero con su propio ecosistema. Opción válida si el banco ya usa SUSE. |

**Recomendación por defecto (si el banco no tiene preferencia):** Ubuntu Server 22.04 o 24.04 LTS. Es la más documentada, fácil de probar, y con la comunidad más amplia.

### Hardware (Servidor Local)

- **CPU**: Intel Xeon o AMD EPYC (mínimo 8 núcleos, recomendado 16+).
- **RAM**: 64 GB (mínimo), 128 GB (recomendado).
- **Almacenamiento**: 1 TB SSD NVMe (para sistema, logs y shadow mode).
- **Red**: Gigabit Ethernet a la intranet del banco.
- **GPU (opcional, pero recomendada)**: NVIDIA T4, L4 o A10 (acelera el procesamiento de modelos).

### Software Base (independiente de la distribución)

- **Contenedores**: Podman o Docker (rootless por seguridad).
- **Python**: 3.10 o superior.
- **Base de datos**: PostgreSQL 14+ (para logs del shadow mode).
- **Librerías de ML**: PyTorch (para CPU o GPU).

**Sin encargado de sistemas presente, no se ejecuta la migración. Esta cláusula es obligatoria y no negociable.**
