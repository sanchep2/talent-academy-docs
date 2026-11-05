---
name: investigar-competencia
description: "Lee las reseñas negativas de la competencia en Google Maps (vía Glasser) y saca las quejas que se repiten y la promesa con la que diferenciarse. Úsala cuando pidan investigar a la competencia, sus reseñas o qué prometer a los clientes."
version: 1.0.0
metadata:
  hermes:
    tags: [research, competencia, reseñas, google-maps, glasser, clientes]
---

# Investigar a la competencia por sus reseñas

Entrada: tipo de negocio, ciudad y presupuesto (por defecto 0,05 USD). Requiere la skill `glasser` instalada y autenticada (`glasser balance` con exit 0).

## Reglas
- Dentro del presupuesto NO pidas confirmación de precio: ejecuta y al final informa del coste total y de los enlaces `Run URL` de Glasser.
- Usa SOLO estos dos endpoints de Serper (sync, baratos). No uses Apify ni DataForSEO. Adapta el código de país tanto en búsquedas como en reseñas a la ciudad solicitada.
- Nunca muestres nombres ni fotos de quien escribió una reseña. No copies reseñas enteras: resume la queja con tus palabras o cita como mucho 8 palabras.
- Son testimonios, no hechos: dilo en una línea.
- Trabaja rápido: lanza en paralelo todo lo que se pueda. Para varios alumnos o negocios independientes, delega un caso completo por subagente, conserva sus presupuestos por separado y revisa sus informes y fuentes antes de presentar resultados juntos en pantalla; no sustituyas el contenido pedido por enlaces de descarga.
- Comprueba que la competencia ofrece el servicio concreto investigado; si solo ofrece servicios próximos, etiquétala como competencia adyacente y no atribuyas sus reseñas al servicio específico sin evidencia.
- Cuando el análisis se extienda a captación, distingue testimonios, hipótesis sobre compradores y datos públicos verificados; no supongas el tipo de formación o capacidad por el nombre de una empresa. Las promesas medibles son propuestas condicionadas a confirmar que el negocio puede cumplirlas.
- Adapta la salida a lo pedido: si solo solicitan negocios y quejas, omite propuestas; si piden reseñas de un negocio, resume cada testimonio sin identificar a su autor.
- Al construir una ficha de cliente ideal desde las quejas, separa los testimonios de las suposiciones sobre el cliente objetivo; no atribuyas las quejas de un sector a otro como hechos. Si se piden preguntas, redacta dudas propuestas, identifica su origen en las reseñas y marca que deben validarse hablando con clientes reales. No inventes personas ni sus datos.
- Si se solicitan únicamente negocios y quejas, no añadas una promesa comercial; conserva la advertencia de que las reseñas son testimonios y el coste con las fuentes.

## Pasos
1. Una sola búsqueda (unos 0,003 USD). Ajusta `gl` al país real de la ciudad (`mx`, `ar`, `es`, etc.); no reutilices México para investigaciones internacionales. Primero `glasser inspect -p serper -e /maps` para ver el esquema exacto y después:
   `glasser run -p serper -e /maps -i '{"q":"<tipo de negocio> en <ciudad>","gl":"mx","hl":"es"}' -o maps.json`
   (si el esquema pide coordenadas `ll`, usa las del centro de la ciudad).
2. Del resultado elige 4 negocios con 20 o más reseñas y nota entre 3,5 y 4,6 (si no hay 4, coge los que más se acerquen y dilo). Guarda su `cid`.
3. Inspecciona `/reviews` y su precio vigente antes de gastar; comprueba que el coste máximo previsto cabe en el presupuesto y registra por separado las URL y cargos de cada página. Para cada negocio, en paralelo, pide sus reseñas (habitualmente 0,001 USD por página de 20):
   `glasser run -p serper -e /reviews -i '{"cid":"<cid>","gl":"mx","hl":"es"}' -o r_<n>_1.json`
   y sigue con `nextPageToken` hasta terminar todas las páginas (máximo 6 páginas por negocio).
4. Quédate con las reseñas de 1 a 3 estrellas con texto. Agrupa las quejas que se repiten en reseñas distintas y cuenta cada reseña una sola vez por tema; especifica si quedó paginación sin revisar. Cita las páginas concretas que sustentan las quejas, no solo la primera página. No declares recuentos por tema sin clasificarlos y verificarlos: una misma reseña puede contar una vez en varios temas, pero no dos veces en el mismo.
5. Responde en español:
   - Tabla 1: negocio · nota · reseñas totales · negativas con texto.
   - Tabla 2: queja que se repite · en cuántas reseñas aparece.
   - La promesa concreta, medible y que se pueda cumplir con la que un negocio nuevo se diferenciaría (2-3 líneas, sin prometer resultados económicos).
   - Coste total y enlaces de Glasser.

## Si también piden prospectos y mensajes
1. Elige el sector y la zona solicitados; si faltan, usa una hipótesis didáctica explícita, sin presentar necesidad de compra como hecho. Busca prospectos dentro del presupuesto autorizado y cita los resultados.
2. Aplica literalmente los filtros y la fórmula del usuario; si hay pocos resultados, muestra la insuficiencia y separa cualquier lista exploratoria con criterios relajados. No conviertas la cantidad de reseñas en facturación ni demuestres independencia solo por ausencia de información.
3. Verifica que el canal pertenece al negocio y permite escribir antes de sumar puntos de contacto; un perfil visible, dirección publicada o formulario sin comprobar no demuestra recepción. Si no se verifica, indica «Canal pendiente de confirmar» y asigna cero cuando así lo pida la regla. Aclara que un formulario redactable no garantiza entrega.
4. Selecciona los negocios con la regla indicada y redacta, sin enviar, contacto inicial, seguimiento y respuesta a una duda para cada uno. Personaliza cada primer contacto con un dato público distinto y comprobado; evita nombres de personas, teléfonos y correos cuando el usuario lo pida. Si se exige una promesa previamente elegida y no puede recuperarse, pregunta antes de redactar; si el usuario autoriza escogerla, propón una y condiciona su uso a la capacidad real.
5. En la entrega conjunta muestra para cada alumno competencia y límites, posibles clientes, ángulo, promesa y ejemplos de presentación; conserva fuentes, costes y advertencias sin convertir hipótesis en hechos.
