# Mis Listas

Este repo es mi app de listas (web en GitHub Pages: https://yybkvcb26f-lgtm.github.io/Mis-Listas/).

## Archivos

- `listas.json`: la lista `"compra"`. Es la ÚNICA lista de la compra (no hay copias en otros sitios).
- **Tareas e ideas: están en el repositorio PRIVADO `yybkvcb26f-lgtm/Tareas-ideas-privado`, archivo `ideas.json` (`{"tareas-ideas": [...]}`, mismo formato de elementos).** Nunca las escribas en este repo (es público): ni en `listas.json` ni en ningún otro archivo, commit o PR. La app las lee y guarda allí con mi token; sin token, la pestaña sale con un candado.
- `supermercados.json`: base de productos con su súper o súpers, en orden de preferencia: `{"Producto": ["Súper 1", "Súper 2"]}`. Lista vacía = súper sin decidir. La app también la actualiza: si cambio el súper de un producto en la compra, ese súper pasa a ser el primero de ese producto (elegir «Sin súper» no cambia la base); si añado o renombro un producto que no está en la base, lo apunta (con el súper que lleve, o `[]`).
- `menu.json`: el menú semanal (pestaña «Menú»). `semana`, `nota` y `dias`: los 7 días (Lunes a Domingo), cada uno `{"dia", "desayuno"?, "comida", "cena"}`.
  - Lunes a viernes, `comida` es `{"trabajo": true}` (como en el trabajo, sin plato). La app enseña la meta de esa comida: objetivo diario − cena.
  - Sábado y domingo: `desayuno` `{"texto", "kcal", "proteina", "grasa"}` y `comida` `{"plato"}`.
  - Todas las cenas: `{"plato", "snack", "fruta", "postre", "extras"}`. `extras` = `{"kcal", "proteina", "grasa"}` estimados de snack + fruta + postre juntos.
  - `base`: la base de platos; cada plato tiene `nombre`, `racion` (texto con cantidades por ración), `kcal`, `proteina` y `grasa` (estimados), `ingredientes` (`[{"producto", "cantidad", "unidad"}]`, unidad `g`, `ml`, `ud` o `lata`; `producto` con el mismo nombre que se usa en la compra) y, si toca, `"postre_ligero": true`. Además, listas de `snacks`, `frutas`, `postres` y `postres_ligeros` (solo nombres).
  - Desde la app, el botón ↻ de cada comida o cena cambia el plato por otro de la base con valores nutricionales parecidos y ajusta sola la compra: resta los `ingredientes` del plato viejo y suma los del nuevo (si un producto queda a 0 se quita; si no estaba, se añade con su súper). En los productos por envase ajusta el «necesitas»: los envases solo suben si dejan de bastar, y si el «necesitas» llega a 0 el producto se quita.
  - Botón ✓ de cada comida o cena con plato («hecha»): descuenta de lo PENDIENTE de la compra lo que lleva (ingredientes del plato + snack/fruta/postre; lo tachado no se toca; en los productos por envase baja el «necesitas» y los envases pueden bajar; si algo llega a 0 se quita). Guarda en esa comida `"hecha": true` y `"descontado": [{"producto", "cantidad", "unidad"}]` con lo que se descontó de verdad. Al desmarcar, devuelve exactamente eso a la compra y borra los dos campos. Mientras está hecha no tiene ↻.
  - La app también entiende el formato antiguo (`cenas`, solo lunes a jueves), pero ya no se usa.
- **Objetivo diario (kcal, proteína, grasa): es privado y NO va nunca en el repo** (el repo es público). Lo tiene la tarea programada del sábado y yo lo guardo en el navegador de mis dispositivos. No lo escribas en ningún archivo, commit ni PR; si no lo tienes, no lo inventes.
- `envases.json`: productos que se compran por envase y cuánto es un envase (ver «Formato de los elementos»).
- `index.html`: la app. Desde ella añado, tacho y quito elementos (con un token de GitHub guardado solo en mi navegador).

## Formato de los elementos

- Pendiente: un texto. Tachado (ya comprado / hecho): `{"texto": "...", "hecho": true}`.
- En `"compra"`, cada elemento es `Producto – cantidad · Súper` (p. ej. `Pollo – 400 g · Aldi`). La cantidad es opcional.
- El súper es el PRIMERO que indica `supermercados.json` para ese producto. Si no está o no tiene súper: `· Sin súper`.
- Nombres de súper: `Aldi`, `Lidl`, `Mercadona`, `BM`, `Carrefour`. Aldi y Lidl son súpers distintos: nunca `Lidl/Aldi`.
- Productos que se compran por envase: están en `envases.json`, cada uno con `{"envase": tamaño de 1 envase, "unidad": "g" | "ml" | "ud", "singular", "plural"}` (p. ej. pasta: paquete de 500 g; arroz: vasito de 100 g; leche: brick de 1000 ml).
  - En la compra van en envases enteros, nunca en gramos: se calcula lo que pide el menú de la semana (platos + snack/fruta/postre) y se redondea HACIA ARRIBA: 600 g de pasta → `Pasta – 2 paquetes de 500 g · BM`; 180 g de arroz → `Arroz – 2 vasitos de 100 g · Aldi`; 40 g de chocolate → `Chocolate 70% – 1 tableta de 100 g · Aldi`; 1,5 l de leche → `Leche – 2 bricks de 1 l · Aldi`. Siempre «N envases de <tamaño>», con el tamaño de 1 envase (en kg o l si llega a 1000). Singular con 1 y plural con más.
  - Detrás, entre paréntesis, lo que pide el menú de esa semana: `Pasta – 1 paquete de 500 g (necesitas 260 g) · BM`; `Leche – 1 brick de 1 l (necesitas 500 ml) · Aldi`; `Huevos – 1 docena de 12 ud (necesitas 4 ud) · Aldi`. La app lo enseña como un segundo globo discreto. En kg o l si llega a 1000 (`necesitas 1,5 l`).
  - Si ya hay uno pendiente y con esos envases basta para la semana, se dejan los envases como están (solo se actualiza el «necesitas» con lo de la semana nueva). Si no basta, se sube al número de envases necesario (no se suman los dos). Al cambiar un plato con ↻, la app suma o resta al «necesitas» y los envases solo suben, nunca bajan.
  - Los productos que no están en `envases.json` (boniato, tomate, aguacate, fruta, entrecot, presa, salmón, atún…) siguen con su cantidad exacta. El atún va siempre en gramos (`Atún – 160 g · Mercadona`), nunca en latas: unas veces lo compro en lata y otras en bote.
  - Yo tacho lo que ya tenga en casa. La app (botón ↻) sigue las mismas reglas.
- Productos unificados (no aparecen nunca sueltos en la compra ni en el menú):
  - Pimiento → se compra como `Verduras para asar` (también el snack «Pimiento en tiras»): sus gramos se suman a las verduras para asar.
  - Yogur griego → solo `Yogur natural`.
  - Mandarinas: no las compro; no se ponen como fruta (mejor kiwis o fresas).
  - Frutos rojos → se llaman `Frambuesas` (en el menú y en la compra).

## Reglas

- Cuando te pida apuntar algo, añádelo a la lista que corresponda (en la compra, con el formato de arriba).
- Las tareas e ideas se apuntan, tachan y quitan en `Tareas-ideas-privado/ideas.json` (git pull antes, commit corto y push a `main` de ese repo, sin preguntarme). Si ese repo no está en la sesión, añádelo (add_repo, acceso push) y clónalo; si no puedes, avísame y NO las apuntes en este repo.
- Cuando te pida quitar algo, bórralo de la lista.
- En cada actualización de la compra, revisa `supermercados.json`: añade los productos nuevos que aparezcan en la lista con su súper. Si un producto no tiene súper en `supermercados.json`, ponlo con `· Sin súper` en la compra y con `[]` en `supermercados.json`, sin preguntarme, y avísalo en el mensaje del commit.
- Después de cada cambio, haz siempre commit con un mensaje corto y push a `main`, sin preguntarme. Antes de editar, haz `git pull` (yo también escribo desde la app).
- No toques nada más del repo salvo que te lo pida.
- Los cambios en la app (`index.html`, diseño, funciones) van siempre en una rama aparte con pull request, sin que te lo pida. Los cambios de las listas sí van directos a `main`.

## Renovación del sábado (menú semanal)

Cada sábado se renueva `menu.json` con la semana siguiente (lunes a domingo):

- `semana`: «Semana del <lunes> al <domingo> de <mes> de <año>». `nota`: «Comidas de lunes a viernes en el comedor del trabajo.»
- 9 platos distintos de `base.platos` (7 cenas + comida del sábado + comida del domingo), sin repetir en la semana y eligiendo primero los que no salieron la semana anterior.
- Lunes a viernes: `comida` = `{"trabajo": true}`.
- Sábado y domingo: `desayuno` = `{"texto": "Leche 200 ml y tostadas con aceite y jamón (o aguacate y jamón, o lomo)", "kcal": 400, "proteina": 20, "grasa": 15}` y `comida` = `{"plato"}`. Desayuno + comida + cena de esos días tienen que acercarse al objetivo diario (aprox. ±10 %): elige la pareja de platos y ajusta snack, fruta y postre para cuadrarlo, y evita juntar dos platos con mucha grasa.
- Cada cena: snack, fruta y postre de la base con su cantidad, y `extras` con sus kcal, proteína y grasa estimados.
- La semana nueva empieza sin comidas hechas (sin `hecha` ni `descontado`).
- Si te pido un plato nuevo, añádelo a `base.platos` con `racion`, `ingredientes`, `kcal`, `proteina` y `grasa`.

Postres: en `menu.json` existe la lista `base.postres_ligeros`, además de `base.postres`. Los platos con `"postre_ligero": true` llevan un postre de `base.postres_ligeros` (con cantidad); el resto, uno de `base.postres` (con cantidad).

Con ese menú se reescribe `"compra"` (las cantidades de los 9 platos salen de `ingredientes`, con esos mismos nombres de producto, y las de snack/fruta/postre de su texto; el desayuno no se añade a la compra):

1. Se borra solo lo tachado (`"hecho": true`).
2. Se mantiene TODO lo pendiente, sin distinguir si era del menú anterior o añadido a mano.
3. Se añaden los ingredientes del menú nuevo con la cantidad total de la semana, en formato `Producto – cantidad · Súper`. Si un ingrediente ya está pendiente, no se duplica: se suman la cantidad que ya había y la que pide el menú. Si las unidades no se pueden sumar, se dejan dos líneas y se avisa en el mensaje del commit. EXCEPCIÓN: los productos de `envases.json` NO se suman así; siguen la regla de «Productos que se compran por envase» (envases enteros + «necesitas» con lo de la semana nueva).
4. Si un producto no tiene súper en `supermercados.json`: `· Sin súper` en la compra y `[]` en `supermercados.json`, sin preguntarme, y se avisa en el mensaje del commit.
5. Orden: por súper (Aldi, Lidl, Mercadona, BM, Carrefour, Sin súper) y dentro de cada súper como vengan.
6. Las tareas e ideas (repo privado) no se tocan.

## Pendientes

Cuando te pregunte qué tenemos pendiente, recuérdame esto:

- Salmón, Merluza, Brócoli y Quinoa no tienen súper todavía (siguen como `· Sin súper`). Cuando tenga súper para alguno, me lo recuerdas y lo apuntamos en `supermercados.json`.
