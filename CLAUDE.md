# Mis Listas

Este repo es mi app de listas (web en GitHub Pages: https://yybkvcb26f-lgtm.github.io/Mis-Listas/).

## Archivos

- `listas.json`: las listas `"compra"` y `"tareas-ideas"`. Es la ÚNICA lista de la compra (no hay copias en otros sitios).
- `supermercados.json`: base de productos con su súper o súpers, en orden de preferencia: `{"Producto": ["Súper 1", "Súper 2"]}`. Lista vacía = súper sin decidir.
- `menu.json`: el menú semanal (pestaña «Menú»). `cenas`: lunes a jueves con `plato` (nombre de un plato de la base), `snack`, `fruta` y `postre` (con cantidades). `base`: la base de platos; cada plato tiene `nombre`, `racion` (cantidades por ración), `kcal` y `proteina` (estimados), y además listas de `snacks`, `frutas` y `postres` (solo nombres). Desde la app, el botón ↻ de cada día cambia el plato por otro de la base con valores nutricionales parecidos.
- `index.html`: la app. Desde ella añado, tacho y quito elementos (con un token de GitHub guardado solo en mi navegador).

## Formato de los elementos

- Pendiente: un texto. Tachado (ya comprado / hecho): `{"texto": "...", "hecho": true}`.
- En `"compra"`, cada elemento es `Producto – cantidad · Súper` (p. ej. `Pechuga de pollo – 400 g · Lidl/Aldi`). La cantidad es opcional.
- El súper es el PRIMERO que indica `supermercados.json` para ese producto. Si no está o no tiene súper: `· Sin súper`.
- Nombres de súper: `Lidl/Aldi`, `Mercadona`, `BM`, `Carrefour`.

## Reglas

- Cuando te pida apuntar algo, añádelo a la lista que corresponda (en la compra, con el formato de arriba).
- Cuando te pida quitar algo, bórralo de la lista.
- En cada actualización de la compra, revisa `supermercados.json`: añade los productos nuevos que aparezcan en la lista con su súper (si no lo sé, pon `[]` y pregúntame).
- Después de cada cambio, haz siempre commit con un mensaje corto y push a `main`, sin preguntarme. Antes de editar, haz `git pull` (yo también escribo desde la app).
- No toques nada más del repo salvo que te lo pida.
- Los cambios en la app (`index.html`, diseño, funciones) van siempre en una rama aparte con pull request, sin que te lo pida. Los cambios de las listas sí van directos a `main`.

## Renovación del sábado (menú semanal)

Cada sábado se renueva `menu.json`: 4 platos distintos de `base.platos` (sin repetir los de la semana anterior), snack, fruta y postre de la base con su cantidad, y `semana` con las fechas de lunes a jueves. Si te pido un plato nuevo, añádelo a `base.platos` con `racion`, `kcal` y `proteina`.

Con ese menú se reescribe `"compra"` (las cantidades salen de `racion` y de snack/fruta/postre):

1. Se borra todo lo tachado (`"hecho": true`).
2. Lo pendiente que NO sea del menú anterior (añadido a mano) se mantiene.
3. Se añaden los ingredientes del menú nuevo con la cantidad total de la semana, en formato `Producto – cantidad · Súper`. Si un ingrediente ya está pendiente, no se duplica: se deja uno solo con la cantidad nueva.
4. Orden: por súper (Lidl/Aldi, Mercadona, BM, Carrefour, Sin súper) y dentro de cada súper como vengan.
5. `"tareas-ideas"` no se toca.
