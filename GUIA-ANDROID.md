# Guía: convertir el Traductor en una app de Android

Esta guía te lleva de la página web a una app instalable y lista para la Play Store.
Todo se hace **desde tu PC con Windows**. No necesitas Mac para Android.

> Trabaja por pasos. Cuando termines un bloque, pruébalo antes de seguir.

---

## 0. Lo que necesitas instalar (una sola vez)

1. **Node.js** (versión LTS) → https://nodejs.org
2. **Android Studio** → https://developer.android.com/studio
   - Al instalarlo, deja marcado **Android SDK** y **Android Virtual Device**.
3. **Cuenta de desarrollador de Google Play** (para publicar) → pago único de **USD 25**.
   - Esto se hace al final, no ahora.

---

## 1. Preparar el proyecto

Abre una terminal **en la carpeta del proyecto** (donde está `package.json`) y corre:

```bash
npm install
npm install @capacitor/core @capacitor/cli @capacitor/android
npx cap add android
```

Esto crea la carpeta `android/` con el proyecto nativo.

Cada vez que cambies algo del contenido web (la carpeta `www/`), sincroniza:

```bash
npx cap sync
```

Para abrirlo en Android Studio:

```bash
npx cap open android
```

Dentro de Android Studio puedes darle **Run ▶** para probarlo en un emulador o en tu
celular conectado por USB (con la depuración USB activada).

---

## 2. Permiso de micrófono (importante)

La app usa el micrófono. En Android hay que declararlo. Abre:

`android/app/src/main/AndroidManifest.xml`

y dentro de `<manifest>` agrega:

```xml
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.INTERNET" />
```

---

## 3. Reconocimiento de voz nativo (paso clave)

El reconocimiento de voz del navegador **no funciona dentro de la app de Android**.
Hay que usar un plugin nativo:

```bash
npm install @capacitor-community/speech-recognition
npx cap sync
```

Luego hay que conectar ese plugin en el código (reemplaza la parte de
`webkitSpeechRecognition` cuando la app corre como app nativa). **Este cambio lo
hacemos juntos en el siguiente paso**, porque requiere probarlo en tu celular.

La traducción y la lectura en voz alta sí siguen funcionando igual.

---

## 4. Anuncios reales (AdMob)

1. Crea una cuenta en **Google AdMob** → https://admob.google.com (gratis).
2. Registra la app y crea un **bloque de anuncios** (banner e intersticial).
3. Instala el plugin:

```bash
npm install @capacitor-community/admob
npx cap sync
```

4. Conectamos los IDs de tus bloques en el código, en el lugar donde ahora están
   los anuncios de demostración. (Lo hacemos juntos.)

> Mientras pruebas, AdMob tiene **IDs de prueba** para no arriesgar tu cuenta.

---

## 5. El pago de USD 1.99 (quitar anuncios)

El cobro real se hace con la **Facturación de Google Play**. La forma más sencilla
es usar **RevenueCat** (gratis hasta cierto volumen), que simplifica todo:

```bash
npm install @revenuecat/purchases-capacitor
npx cap sync
```

Pasos:
1. En la **Play Console**, crea un producto de compra única llamado, por ejemplo,
   `quitar_anuncios`, con precio USD 1.99.
2. Conecta RevenueCat con tu cuenta de Play.
3. En el código, el botón **"Comprar sin anuncios"** llama a la compra real y,
   al confirmarse, activa el modo sin anuncios. (Lo conectamos juntos.)

> Ojo: para cobrar necesitas completar en la Play Console tus **datos de pago y
> fiscales** (cuenta bancaria, etc.).

---

## 6. Ícono y nombre

- Cambia el nombre visible en `android/app/src/main/res/values/strings.xml`.
- El ícono se genera fácil con **Android Studio → Image Asset**, o con la
  herramienta `@capacitor/assets`. Te ayudo a crear un ícono cuando lleguemos aquí.

---

## 7. Publicar en la Play Store

1. En Android Studio: **Build → Generate Signed Bundle / APK → Android App Bundle (.aab)**.
   - Crea una **clave de firma** y **guárdala muy bien** (sin ella no puedes actualizar la app).
2. En la **Play Console**, crea la ficha de la app: nombre, descripción, capturas, ícono.
3. Sube el archivo `.aab`, completa el cuestionario de contenido y envíalo a revisión.
4. Google revisa en unas horas o días. Cuando aprueben, queda publicada.

---

## Orden recomendado

1. Pasos 0 y 1 → ver la app corriendo en tu celular. **Empieza por aquí.**
2. Paso 3 → que escuche por micrófono dentro de la app.
3. Paso 4 → anuncios.
4. Paso 5 → el pago.
5. Pasos 6 y 7 → ícono y publicación.

Haz el paso 1 y cuéntame cómo te fue; seguimos con el reconocimiento de voz nativo.
