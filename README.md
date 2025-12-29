# Consulta Pública de Recibos - GitHub Pages

Guía para publicar la consulta pública en GitHub Pages usando un repositorio nuevo llamado "consultaRecibos".

## 📁 Qué subir
- `index.html`: página pública que carga y muestra los recibos.
- `recibos.json`: archivo de datos (se actualiza mensualmente).
- `assets/` (opcional): imágenes/estilos si los necesitas.
- `README.md`: esta guía.

## 🚀 Crear y conectar el repositorio
1. Crea el repositorio en GitHub:
   - Nombre: `consultaRecibos`
   - Visibilidad: público (recomendado para acceso público). En cuentas gratuitas, GitHub Pages es público; en privadas se requiere plan de pago.
2. Activa GitHub Pages:
   - En GitHub: Settings → Pages → Source: "Deploy from a branch"
   - Branch: `main` → Folder: `/docs`
   - Guarda. La URL será: `https://<tu-usuario>.github.io/consultaRecibos/`

## ⬆️ Primer subida (desde tu PC)
1. Prepara carpeta `docs/` con los archivos mínimos:
   ```
   docs/
   ├── index.html
   ├── recibos.json
   └── README.md
   ```
2. Desde PowerShell en la raíz del proyecto:
   ```powershell
   git init
   git branch -M main
   git remote add origin https://github.com/<tu-usuario>/consultaRecibos.git
   git add docs
   git commit -m "Inicial: consulta pública y recibos"
   git push -u origin main
   ```
3. Espera 1-5 minutos y visita tu URL pública.

## 🔄 Actualizar cada mes
1. Genera el JSON desde el panel de Reportes (botón "📄 Generar JSON Público").
2. Reemplaza `docs/recibos.json` con el nuevo archivo.
3. Sube cambios:
   ```powershell
   git add docs/recibos.json
   git commit -m "Actualizar recibos - <Mes Año>"
   git push
   ```
4. Verifica la actualización en la URL.

## ❓ ¿Debe ser público el repositorio?
- **Público (recomendado)**: cualquiera puede acceder. Es el modo estándar en cuentas gratuitas.
- **Privado**: sólo posible con planes de pago (GitHub Pro/Enterprise) y puede requerir controles adicionales. Si deseas acceso público sin restricciones, usa repositorio público.

## 🔧 Ejemplo básico de `index.html`
Coloca este archivo en `docs/index.html` para una página mínima que carga el JSON:
```html
<!doctype html>
<html lang="es">
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Consulta Pública de Recibos</title>
  <style>body{font-family:Segoe UI,Arial,sans-serif;margin:20px} .item{padding:8px;border-bottom:1px solid #ddd}</style>
  <h1>Consulta Pública de Recibos</h1>
  <input id="busqueda" placeholder="Ej: A-01" />
  <button onclick="buscar()">Buscar</button>
  <div id="resultado"></div>
  <script>
    let data = null;
    const base = location.pathname.endsWith('/') ? location.pathname : location.pathname + '/';
    fetch(base + 'recibos.json')
      .then(r => r.json())
      .then(j => { data = j; });
    function buscar(){
      const q = document.getElementById('busqueda').value.trim().toLowerCase();
      if(!data){ alert('Datos no cargados aún'); return; }
      const recibo = (data.recibos || []).find(r => (r.puesto?.numero_puesto||'').toLowerCase() === q);
      const el = document.getElementById('resultado');
      if(!recibo){ el.textContent = 'No encontrado'; return; }
      el.innerHTML = `<div class="item"><strong>Puesto:</strong> ${recibo.puesto.numero_puesto}<br/>` +
                     `<strong>Propietario:</strong> ${recibo.puesto.propietario}<br/>` +
                     `<strong>Consumo:</strong> ${recibo.ultimaLectura.consumo} kWh</div>`;
    }
  </script>
</html>
```

## 🛠️ Solución de problemas
- Si no carga: revisa que los archivos estén en `/docs` y la configuración de Pages apunte a esa carpeta.
- Limpia caché del navegador (Ctrl+F5) si no ves cambios.
- Valida `recibos.json` en https://jsonlint.com.

---
**Última actualización**: Diciembre 2025
