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

## Bitácora técnica: preparación del entorno Flutter

Estudiante: Marco Gutiérrez. Curso: Desarrollo de Aplicaciones Móviles, Ciclo 3/2026. Fecha: 27 de agosto de 2026.

### Error encontrado

Al correr `flutter doctor -v`, la sección de Android toolchain mostraba una advertencia: "Android license status unknown", pidiendo aceptar las licencias del SDK. Al intentar aceptarlas con `flutter doctor --android-licenses`, el comando no hacía nada, solo mostraba un aviso de que esa opción ya no era necesaria. Ese mismo problema hizo que, al correr la app por primera vez, fallara la compilación porque no se pudo instalar el NDK (una herramienta que Android necesita para compilar).

### Causa

Investigué y encontré que Google reemplazó la herramienta `sdkmanager`, que era la que usaba Flutter para manejar las licencias, por una herramienta nueva llamada "Android CLI". Como es un cambio muy reciente, Flutter todavía no sabe comunicarse bien con esta herramienta nueva, así que no logra confirmar si las licencias están aceptadas, aunque en realidad sí lo están.

### Cómo lo comprobé y resolví

En vez de forzar comandos que no entendía, como advierte la guía del curso, revisé que los archivos de licencias en la carpeta del SDK ya tenían la información correcta. Para el error del NDK, lo instalé manualmente con el comando `android sdk install "ndk;28.2.13676358"`. Después creé este proyecto de prueba y lo corrí en un emulador: compiló, se instaló y funcionó sin problema, incluyendo la recarga en caliente. Con eso confirmé que el entorno sí funciona bien, aunque `flutter doctor` siga mostrando esa advertencia por un tema de compatibilidad entre versiones muy nuevas.

Fuentes revisadas: docs.flutter.dev/install/troubleshoot y developer.android.com/tools/sdkmanager. Ninguna menciona todavía este cambio.
