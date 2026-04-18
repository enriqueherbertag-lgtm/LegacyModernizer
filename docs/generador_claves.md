import secrets
from datetime import datetime, date

def generar_clave():
    clave = secrets.token_urlsafe(8)  # ej. "x7kL9mQ2"
    fecha = date.today().isoformat()
    print(f"Clave: {clave} | Válida hasta: {fecha} 23:59")
    return clave

def validar_clave(clave, fecha_entrega):
    if fecha_entrega != date.today():
        return False
    # verificar en BD de claves activas
    return clave in claves_activas
