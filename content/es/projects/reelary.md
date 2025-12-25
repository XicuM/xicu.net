---
title: Reelary 
date: 2025-12-25T09:05:03+01:00 
hideMeta: true 
ShowBreadCrumbs: true 
tags: [Flutter, Instagram, Gemini API] 
github: https://github.com/XicuM/Reelary 
icon: "/projects/reelary/icon.png" 
cover: 
    image: "/projects/reelary/cover.png"
---

Una elegante aplicación de Flutter para extraer recetas y lugares de los Reels de Instagram utilizando la API de Gemini, con una interfaz de usuario basada en Material Design 3 y organización por carpetas.

> **AVISO: ÚNICAMENTE CON FINES DEMOSTRATIVOS**
> Esta aplicación tiene un propósito estrictamente educativo y demostrativo. **NO** está diseñada para ser ejecutada o distribuida, ya que podría infringir las Condiciones de uso de Instagram. Úsala bajo tu propia responsabilidad.

## ✨ Características

* 📱 **Extraer recetas y lugares**: Convierte Reels de Instagram en recetas estructuradas o recomendaciones de lugares.
* 🗺️ **Mapas interactivos**: Visualiza ubicaciones en Google Maps con marcadores y navegación.
* 🏷️ **Categorización inteligente**: Etiquetado automático mediante IA para lugares (restaurantes, sitios de viaje, actividades, naturaleza).
* 📁 **Organización por carpetas**: Organiza tus recetas y lugares con carpetas personalizables mediante emojis.
* 🎨 **Material Design 3**: Interfaz moderna y cuidada con soporte para modo oscuro.
* ✅ **Cocina interactiva**: Tacha los pasos de la receta a medida que cocinas.
* 📍 **Gestión de ubicaciones**: Múltiples ubicaciones por lugar con coordenadas GPS y direcciones.
* 💾 **Almacenamiento local**: Todos los datos se guardan localmente mediante SQLite.
* 🌙 **Modo oscuro**: Cambio de tema automático basado en las preferencias del sistema.

## Instrucciones de configuración

### Requisitos previos

**Obligatorio:** API de RapidAPI Instagram Downloader (para la descarga de vídeos de Instagram multiplataforma).

### Configuración de la aplicación

1. **Inicializar el proyecto de Flutter**
Dado que es un proyecto nuevo, necesitas generar los archivos específicos de la plataforma (Android/iOS). Ejecuta el siguiente comando en tu terminal:
```bash
flutter create .

```

2. **Configurar el Android Manifest**
La aplicación utiliza el paquete `receive_sharing_intent` para gestionar las URLs compartidas desde Instagram.
Tras ejecutar `flutter create .`, revisa el archivo `android/app/src/main/AndroidManifest.xml`.
Asegúrate de que el siguiente `<intent-filter>` esté presente dentro de la etiqueta `<activity>`:
```xml
<intent-filter>
    <action android:name="android.intent.action.SEND" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:mimeType="text/plain" />
</intent-filter>

```

*Nota: Ya he creado este archivo para ti, pero `flutter create` podría sobrescribirlo o requerir que fusiones los cambios.*
3. **Configuración de las claves de API (API Keys)**
**a) Gemini API Key**
Obtén tu clave de API de Gemini en [Google AI Studio](https://makersuite.google.com/app/apikey).
**b) RapidAPI Key**
1. Regístrate en [RapidAPI](https://rapidapi.com/).
2. Suscríbete a [Instagram Premium API 2023](https://rapidapi.com/sainikhilmaremanda-jgDbPvTvR/api/instagram-premium-api-2023) (tiene modalidad gratuita).
3. Copia tu clave de API desde el panel de control.

**c) Google Maps API Key**
1. Ve a [Google Cloud Console](https://console.cloud.google.com/).
2. Crea un proyecto nuevo o selecciona uno existente.
3. Habilita el SDK de Maps para Android e iOS.
4. Crea las credenciales (API Key).
5. Restringe la clave de API al nombre de paquete de tu aplicación por seguridad.

**Para Android:** Abre `android/app/src/main/AndroidManifest.xml` y sustituye `YOUR_API_KEY_HERE`:
```xml
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="TU_CLAVE_DE_GOOGLE_MAPS" />

```

**Para iOS:** Abre `ios/Runner/AppDelegate.swift` y sustituye `YOUR_API_KEY_HERE`:
```swift
GMSServices.provideAPIKey("TU_CLAVE_DE_GOOGLE_MAPS")

```

**d) Actualizar el archivo .env**
Abre el archivo `.env` en el directorio raíz y añade tus claves:
```
GEMINI_API_KEY=tu_clave_de_gemini
RAPIDAPI_KEY=tu_clave_de_rapidapi
RAPIDAPI_HOST=instagram-premium-api-2023.p.rapidapi.com
INSTAGRAM_POST_INFO_ENDPOINT=/v1/post_info

```

4. **Ejecutar la aplicación**
```bash
flutter run

```

## Cómo usar la aplicación

### Recetas

* **Compartir con la app**: Comparte un Reel de Instagram directamente a Reelary.
* **Pegar URL**: Pega manualmente la URL de un Reel.
* **Extracción con Gemini**: Utiliza Gemini 2.0 Flash para analizar el vídeo y extraer ingredientes y pasos.
* **Almacenamiento de recetas**: Guarda las recetas localmente con un modo de cocina interactivo.

### Lugares

* **Extraer lugares**: La IA identifica ubicaciones, direcciones y categorías a partir de Reels de viajes o comida.
* **Ver en el mapa**: Vista interactiva de Google Maps con marcadores para cada ubicación.
* **Etiquetas inteligentes**: Categorización automática en Restaurante, Sitio de viaje, Actividades o Naturaleza.
* **Navegación**: Enlaces de navegación directa a Google Maps para cada ubicación.
* **Compartir**: Comparte los detalles del lugar incluyendo los enlaces de Google Maps.
