# HoverFlow

**Hover to expand. Leave to restore.**

Una utilidad portátil para Windows que da espacio a la ventana bajo el cursor y la devuelve a su posición original cuando sales. La expansión **nunca ocupa toda la pantalla**: mantiene un perímetro de salida configurable en los cuatro lados del área de trabajo, sin tapar la barra de tareas.

Interfaz en inglés · documentación en español · C# / WPF / Win32 · licencia MIT.

![Interfaz de HoverFlow](assets/screenshot.png)

## Primeros pasos

1. Extrae **todo** el ZIP portátil a una carpeta de tu elección.
2. Abre `HoverFlow.exe`. Requiere Windows 10 22H2 o Windows 11 con .NET Framework 4.8 o posterior. No requiere privilegios de administrador ni conexión a Internet.
3. Coloca el cursor sobre una ventana normal y redimensionable, lejos del borde de la pantalla, durante 180 ms.
4. La ventana se expande dejando 40 DIP de margen. Lleva el cursor al margen y espera 140 ms: recupera su tamaño y posición anteriores.
5. Ajusta **Exit gutter**, **Hover delay** y **Restore delay**, y pulsa **Save preferences**.

El ejecutable es una versión preliminar sin firma digital. Si Windows muestra una advertencia de editor desconocido, verifica el hash SHA-256 o compílalo desde el código. No desactives las protecciones del sistema.

## Controles

| Acción | Atajo | También disponible |
| --- | --- | --- |
| Pausar / reanudar | Ctrl + Alt + H | Panel y bandeja |
| Fijar / liberar la expansión actual | Ctrl + Alt + P | Panel y bandeja |
| Restaurar ahora | Ctrl + Alt + R | Panel y bandeja |
| Cerrar el panel | Botón × | Mantiene la aplicación en la bandeja |
| Salir y restaurar | — | Bandeja → Quit and restore |

**Pin** conserva temporalmente la expansión aunque salgas con el cursor. No convierte la ventana en «siempre visible». Pausar, restaurar o salir prevalece sobre el pin. Si otro programa usa un atajo, HoverFlow informa del conflicto y mantiene los controles de bandeja.

## Qué incluye

- Retardos ajustables, margen mínimo de 16 DIP y cooldown de 50–3000 ms.
- Restauración de coordenadas originales, sin acumular desplazamientos.
- Una expansión simultánea, sin cambiar el foco ni el orden de las ventanas.
- Rearme que exige salir y volver a entrar: un cursor quieto no provoca oscilaciones.
- Exclusiones por nombre de proceso, sin distinguir mayúsculas; acepta `.exe` opcional.
- Opción **Only expand the active window**; por defecto funciona también sobre ventanas inactivas visibles.
- Respeta arrastres, menús, ventanas emergentes propias y teclas modificadoras.
- Si cambias manualmente una ventana, conserva tu cambio y deja de gestionarla hasta un nuevo ciclo.
- Coordenadas físicas, monitores con origen negativo y margen escalado según el DPI del monitor.
- Bandeja, instancia única por sesión/usuario, preferencias locales y registro de recuperación tras un cierre inesperado.
- Sin telemetría, servicios, tareas programadas ni modificaciones de PowerToys.

Se omiten ventanas maximizadas, minimizadas, a pantalla completa, ventanas emergentes con propietario, ventanas no redimensionables, ocultas, cloaked y ventanas que bloquean el acceso. Las ventanas independientes «siempre visibles» conservan ese estado. Si una app impone un tamaño incompatible con el margen, HoverFlow intenta volver inmediatamente al original y la omite.

### Actualizar desde 0.1.0

Cierra la versión anterior con **Quit and restore** y abre el ejecutable del ZIP nuevo. En el primer inicio se ajustan los tiempos antiguos a un máximo de 180 ms / 140 ms / 200 ms, conservando el margen, las exclusiones y los tiempos que ya fueran más rápidos. Después puedes personalizarlos; la migración no se repite. **Use fast timing**, dentro de preferencias, guarda el perfil rápido con un clic.

Se corrigió el rechazo de ventanas de FancyZones/Snap que ocupan toda la altura o anchura: ahora pueden reducirse en una dimensión para dejar el margen, siempre que el área total crezca al menos un 5%. Pasar a otra ventana ya no añade el cooldown de la anterior.

## Compilar y probar

Desde PowerShell, en la raíz del repositorio:

```powershell
.\build.ps1 -Test
.\build.ps1 -Test -NativeTest -Package
.\artifacts\HoverFlow\HoverFlow.exe
```

El script utiliza el compilador incluido en .NET Framework y no necesita NuGet ni un SDK descargado. Si tu política impide ejecutar scripts, revisa el archivo y usa, únicamente para esa ejecución:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\build.ps1 -Test -NativeTest -Package
```

Las pruebas nativas abren una ventana auxiliar propia y la cierran al terminar; no mueven tus ventanas ni el cursor. El ZIP se genera en `artifacts/`. Hay también un proyecto de Visual Studio para .NET Framework 4.8. Consulta [compilación y distribución](docs/BUILD.md).

## PowerToys FancyZones

HoverFlow conserva el rectángulo que tenía la ventana al entrar, incluida una posición creada con FancyZones. No modifica zonas ni intercepta sus atajos. Mientras arrastras o mantienes Shift/Win, no inicia expansiones. Una recolocación de FancyZones se trata como un cambio manual y se respeta.

La pertenencia interna a una zona no se guarda ni reconstruye. No hay una integración con la API de FancyZones y la compatibilidad con todas sus opciones necesita comprobación manual. Más detalles en [la guía de uso](docs/USAGE.es.md).

## Documentación

- [Uso, preferencias, FancyZones y solución de problemas](docs/USAGE.es.md)
- [Compilación, instalación, desinstalación y publicación en GitHub](docs/BUILD.md)
- [Arquitectura y recuperación](docs/ARCHITECTURE.md)
- [Pruebas y límites de validación](docs/TESTING.md)
- [English quick start](docs/README.en.md)
- [Contribuciones](CONTRIBUTING.md) · [Seguridad](SECURITY.md) · [Cambios](CHANGELOG.md)

## Alcance de la versión 0.1.1

Versión preliminar para validar el flujo con tus aplicaciones. La interfaz usa inglés; los textos aún no están extraídos a recursos de localización. No incluye instalador, inicio automático, actualizador ni firma de código. La restauración conserva la geometría de ventanas normales; una minimización o maximización explícita tuya se respeta en lugar de revertirse. Las aplicaciones elevadas suelen quedar fuera por las restricciones de Windows. No ejecutes HoverFlow como administrador para evitarlas.

Los resultados de prueba y los escenarios que todavía requieren hardware adicional se detallan en [TESTING.md](docs/TESTING.md).

## Licencia

[MIT](LICENSE). Puedes publicar este directorio como repositorio independiente; no contiene datos personales ni credenciales.
