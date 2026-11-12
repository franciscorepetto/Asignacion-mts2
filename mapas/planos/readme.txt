Cómo agregar un plano nuevo desde GitHub (sin usar el botón "Subir plano" / Supabase)
======================================================================================

1) Subí el archivo .dxf a esta carpeta (mapas/planos/), idealmente dentro de
   una subcarpeta con el código de la sucursal. Ejemplo:

     mapas/planos/MDQ/Planta_Baja.dxf

   Se puede hacer directo desde la web de GitHub: entrá a esta carpeta,
   "Add file" → "Upload files".

2) Agregá una entrada en manifest.json (en esta misma carpeta) con esta forma:

   {
     "MDQ": [
       { "id": "PB", "name": "Planta Baja", "file": "MDQ/Planta_Baja.dxf", "piso": "PB" }
     ]
   }

   - "id"   : identificador único del piso dentro de esa sucursal (sin espacios).
   - "name" : nombre que se muestra en el selector de piso.
   - "file" : ruta del archivo, relativa a esta carpeta (mapas/planos/).
   - "piso" : etiqueta corta que se usa en la ficha de actividades.

   Si la sucursal ya tiene otros pisos cargados, sumá el objeto nuevo al
   array existente en vez de reemplazarlo.

3) Hacé commit y push (o guardá el cambio directo en la web de GitHub).
   Al recargar la app, la sucursal va a aparecer disponible en el selector
   con ese plano, sin necesidad de tocar Supabase.

El código de sucursal ("MDQ" en el ejemplo) tiene que coincidir con el
código IATA usado en el resto de la plataforma (ver el array SUCURSALES
en index.html).

Nota: los .dxf sueltos en esta carpeta (sin subcarpeta) son los planos
viejos que estaban antes de migrar a Supabase; manifest.json los referencia
directamente por nombre de archivo. Algunos quedaron sin mapear en
manifest.json porque el nombre no permitía identificar con certeza a qué
sucursal correspondían (ver aviso del asistente al restaurarlos).

Planta alta dibujada en el mismo CAD que la planta baja (ej.: ROS)
==================================================================
Se generan dos .dxf alineados (mismas coordenadas y mismos $EXTMIN/$EXTMAX)
y se cargan como dos pisos en manifest.json. En el de planta alta, la planta
baja va en la capa "REFERENCIA": la app la dibuja en gris claro, solo como
guía para ubicarse. El campo "piso" del manifest ("PB", "1RO", …) es el valor
que se propone por defecto en la columna Piso de la planilla.

ROS (Rosario): sale de "ROSARIO - LAYOUT OPERATIVO - 2026_11_11", lámina
PROPUESTA 11 NOV 2026, en metros, sin textos, cotas ni mobiliario. Los dibujos
de planta alta (oficinas, entrepiso y oficina junto a Control) quedan donde
están dibujados en el CAD; la garita de Tráfico, que es igual en PB y PA, se
copió a la planta alta.
