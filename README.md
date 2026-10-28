# Inka-Sec

Aplicación web de capacitación en ciberseguridad para empresas del Perú.

## Ejecutar el flujo de administrador en local

El frontend React está en `frontend/`, la API FastAPI en `mi-api-fastapi/` y la base de datos de desarrollo es SQLite. La API crea sus tablas al arrancar. Necesitas Node.js y Python 3.11 o superior.

1. En `mi-api-fastapi/`, crea un entorno virtual e instala las dependencias:

   ```bash
   python -m venv .venv
   # Activa el entorno: source .venv/bin/activate en bash.
   # En PowerShell: .\.venv\Scripts\Activate.ps1
   python -m pip install -r requirements.txt
   ```

2. Copia `.env.example` a `.env`. Sustituye la clave JWT, el correo y la contraseña del administrador y el secreto TOTP. Puedes generar los dos secretos con Python:

   ```bash
   python -c "import secrets; print(secrets.token_urlsafe(48))"
   python -c "import base64, secrets; print(base64.b32encode(secrets.token_bytes(20)).decode().rstrip('='))"
   ```

   Guarda el secreto TOTP en tu aplicación de autenticación para obtener los códigos de seis dígitos. No subas `.env` al repositorio.

3. Desde `mi-api-fastapi/`, crea el primer administrador y arranca la API:

   ```bash
   python -m app.bootstrap_admin
   python -m uvicorn app.main:app --reload --port 8000
   ```

4. En otra terminal, desde `frontend/`, instala y arranca React:

   ```bash
   npm ci
   npm run dev
   ```

   En Windows PowerShell, usa `npm.cmd ci` y `npm.cmd run dev` si PowerShell bloquea `npm.ps1`.

5. Abre `http://localhost:5173/`. Inicia sesión con el correo y contraseña configurados; luego escribe el código de tu aplicación de autenticación. El panel se obtiene de `http://localhost:8000/api/v1/admin/dashboard`. Para cambiar el puerto de la API, configura `VITE_API_URL` en `frontend/.env`.

La ruta `http://localhost:5173/?preview=admin` muestra únicamente datos de ejemplo para revisar el diseño.
