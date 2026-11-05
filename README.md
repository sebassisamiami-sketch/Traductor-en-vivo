# Traductor en vivo

App web con interfaz estilo iOS que escucha inglés en vivo y muestra cada frase
en **inglés arriba y español abajo**. También puede generar una respuesta en
inglés (con su traducción) usando una clave de OpenAI.

La app web vive en la carpeta `www/`.

## Probar en el navegador

Necesita **HTTPS** para acceder al micrófono, así que no funciona abriendo el
archivo directamente. Dos opciones:

### Opción 1 — Servidor local rápido
```bash
npx serve www
```
Abre la dirección que te muestre (http://localhost:3000) en **Google Chrome**.

### Opción 2 — Publicar en Firebase
```bash
npx firebase-tools deploy --only hosting
```

## Convertirla en app de Android

Sigue **GUIA-ANDROID.md**: empaqueta el proyecto con Capacitor y lo publica en la
Play Store desde tu PC con Windows.

## Notas
- La **traducción** EN→ES funciona sin configurar nada (usa MyMemory, gratis).
- Para **generar respuestas en inglés**, abre *Ajustes* y pega tu clave de OpenAI
  (se guarda solo en tu navegador, no se sube a ningún lado).
- Usa **Google Chrome**: es el que mejor soporta el reconocimiento de voz.
