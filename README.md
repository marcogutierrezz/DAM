# verificacion_entorno

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Learn Flutter](https://docs.flutter.dev/get-started/learn-flutter)
- [Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Flutter learning resources](https://docs.flutter.dev/reference/learning-resources)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

---

## Bitácora técnica — Preparación del entorno Flutter

**Estudiante:** Marco Gutiérrez · **Curso:** Desarrollo de Aplicaciones Móviles — Ciclo 3/2026 · **Docente:** Jaime Mejía
**Fecha:** 27 de agosto de 2026 · **Entorno:** Windows 11 · Flutter 3.47.1 · Android SDK 37.0.0

### Error documentado: estado de licencias de Android SDK no detectable por Flutter

**1. Mensaje de error completo**

```
flutter doctor -v
[!] Android toolchain - develop for Android devices (Android SDK version 37.0.0)
    X Android license status unknown.
      Run `flutter doctor --android-licenses` to accept the SDK licenses.

flutter run  →  "Package ndk not found. Package 28.2.13676358 not found."
Android sdkmanager did not install NDK 28.2.13676358 into ...\Android\sdk.
```

**2. Causa identificada**

El SDK de Android instalado (agosto 2026) reemplazó la herramienta clásica `sdkmanager` por una nueva "Android CLI" (`android.exe`). Al ejecutar `flutter doctor --android-licenses` o cualquier `sdkmanager.bat` (de cualquier versión de las Command-line Tools), la llamada es interceptada por la Android CLI, que solo imprime un aviso de obsolescencia y no ejecuta el flujo de licencias que Flutter espera parsear. Por eso el estado queda en "unknown" aunque las licencias sí estén aceptadas en disco (se verificó que los archivos `android-sdk-license` y `android-sdk-preview-license` en `<sdk>/licenses` ya contenían los hashes oficiales vigentes). Por la misma razón, Gradle tampoco pudo auto-instalar el NDK durante la compilación, porque también depende internamente de `sdkmanager`.

**3. Fuentes consultadas (contraste antes de aplicar la solución)**

- [docs.flutter.dev/install/troubleshoot](https://docs.flutter.dev/install/troubleshoot) — no cubre aún el nuevo comportamiento (confirmado por búsqueda directa en la página).
- [developer.android.com/tools/sdkmanager](https://developer.android.com/tools/sdkmanager) y [developer.android.com/tools/agents/android-cli](https://developer.android.com/tools/agents/android-cli) — tampoco documentan la interacción con Flutter; se confirmó que `sdkmanager` ya no tiene binario propio (no existe jar de `sdkmanager` en `cmdline-tools/*/lib`).

**4. Solución aplicada y resultado**

No se forzó ningún comando con privilegios de administrador ni se modificaron licencias a mano, siguiendo la regla del curso. En su lugar se verificó el entorno funcionalmente: se instaló el NDK faltante con el subcomando que sí funciona en la nueva CLI (`android sdk install "ndk;28.2.13676358"`), y se creó y ejecutó este mismo proyecto de prueba en un emulador Android (Pixel 10, API 37). El proyecto compiló, se instaló y corrió sin errores, y la recarga en caliente (hot reload) y el reinicio en caliente (hot restart) funcionaron correctamente. Esto confirma que el Android toolchain está realmente operativo pese al indicador `[!] unknown` que reporta `flutter doctor`, el cual queda documentado aquí como una discrepancia conocida entre Flutter 3.47.1 y la versión más reciente del Android SDK Command-line Tools.
