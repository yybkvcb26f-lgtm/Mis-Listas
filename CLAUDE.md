# Mis Listas

Este repo es mi app de listas (web en GitHub Pages: https://yybkvcb26f-lgtm.github.io/Mis-Listas/).

## Archivos

- `listas.json`: las listas `"compra"` y `"tareas-ideas"`. Es la ÚNICA lista de la compra (no hay copias en otros sitios).
- `supermercados.json`: base de productos con su súper o súpers, en orden de preferencia: `{"Producto": ["Súper 1", "Súper 2"]}`. Lista vacía = súper sin decidir.
- `menu.json`: el menú semanal (pestaña «Menú»). `cenas`: lunes a jueves con `plato` (nombre de un plato de la base), `snack`, `fruta` y `postre` (con cantidades). `base`: la base de platos; cada plato tiene `nombre`, `racion` (texto con cantidades por ración), `ingredientes` (`[{"producto", "cantidad", "unidad"}]`, unidad `g`, `ml`, `ud` o `lata`; `producto` con el mismo nombre que se usa en la compra), `kcal` y `proteina` (estimados), y además listas de `snacks`, `frutas` y `postres` (solo nombres). Desde la app, el botón ↻ de cada día cambia el plato por otro de la base con valores nutricionales parecidos y ajusta sola la compra: resta los `ingredientes` del plato viejo y suma los del nuevo (si un producto queda a 0 se quita; si no estaba, se añade con su súper).
- `index.html`: la app. Desde ella añado, tacho y quito elementos (con un token de GitHub guardado solo en mi navegador).

## Formato de los elementos

- Pendiente: un texto. Tachado (ya comprado / hecho): `{"texto": "...", "hecho": true}`.
- En `"compra"`, cada elemento es `Producto – cantidad · Súper` (p. ej. `Pechuga de pollo – 400 g · Lidl/Aldi`). La cantidad es opcional.
- El súper es el PRIMERO que indica `supermercados.json` para ese producto. Si no está o no tiene súper: `· Sin súper`.
- Nombres de súper: `Lidl/Aldi`, `Mercadona`, `BM`, `Carrefour`.

## Reglas

- Cuando te pida apuntar algo, añádelo a la lista que corresponda (en la compra, con el formato de arriba).
- Cuando te pida quitar algo, bórralo de la lista.
- En cada actualización de la compra, revisa `supermercados.json`: añade los productos nuevos que aparezcan en la lista con su súper. Si un producto no tiene súper en `supermercados.json`, ponlo con `· Sin súper` en la compra y con `[]` en `supermercados.json`, sin preguntarme, y avísalo en el mensaje del commit.
- Después de cada cambio, haz siempre commit con un mensaje corto y push a `main`, sin preguntarme. Antes de editar, haz `git pull` (yo también escribo desde la app).
- No toques nada más del repo salvo que te lo pida.
- Los cambios en la app (`index.html`, diseño, funciones) van siempre en una rama aparte con pull request, sin que te lo pida. Los cambios de las listas sí van directos a `main`.

## Renovación del sábado (menú semanal)

Cada sábado se renueva `menu.json`: 4 platos distintos de `base.platos` (sin repetir los de la semana anterior), snack, fruta y postre de la base con su cantidad, y `semana` con las fechas de lunes a jueves. Si te pido un plato nuevo, añádelo a `base.platos` con `racion`, `ingredientes`, `kcal` y `proteina`.

Con ese menú se reescribe `"compra"` (las cantidades de los platos salen de `ingredientes`, con esos mismos nombres de producto, y las de snack/fruta/postre de su texto):

1. Se borra solo lo tachado (`"hecho": true`).
2. Se mantiene TODO lo pendiente, sin distinguir si era del menú anterior o añadido a mano.
3. Se añaden los ingredientes del menú nuevo con la cantidad total de la semana, en formato `Producto – cantidad · Súper`. Si un ingrediente ya está pendiente, no se duplica: se suman la cantidad que ya había y la que pide el menú. Si las unidades no se pueden sumar, se dejan dos líneas y se avisa en el mensaje del commit.
4. Si un producto no tiene súper en `supermercados.json`: `· Sin súper` en la compra y `[]` en `supermercados.json`, sin preguntarme, y se avisa en el mensaje del commit.
5. Orden: por súper (Lidl/Aldi, Mercadona, BM, Carrefour, Sin súper) y dentro de cada súper como vengan.
6. `"tareas-ideas"` no se toca.
