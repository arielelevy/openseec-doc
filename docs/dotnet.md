# Analizar proyectos .NET con openSeec

openSeec lee el proyecto para encontrar señales de código, secretos, configuración y dependencias. No necesita compilarlo: no ejecuta `dotnet`, MSBuild, tareas del proyecto ni una restauración de paquetes.

## Primera revisión

1. Elegí la carpeta de la solución o del proyecto. El perfil Fuentes omite artefactos que no aportan a la revisión; la cobertura informa el alcance efectivo.
2. Abrí **Consulta remota · Cambiar** antes de analizar. Los motores locales funcionan sin modelos. Si activás consultas remotas, se envían los conteos y estados agregados que describe ese control.
3. Tocá **Analizar**. El grafo muestra qué se ejecutó, qué falló y qué quedó sin analizar. Terminado no significa seguro.
4. En **Señales para revisar**, buscá una ruta o regla. **Filtros** permite elegir tipo, motor, severidad y verificación. El conteo mantiene visible cuántas señales de la lista estás viendo.
5. Abrí una señal para revisar su motivo, regla y procedencia. Una versión corregida de un paquete corresponde al aviso publicado; no prueba por sí sola compatibilidad ni ausencia de otros avisos.
6. Usá **Informe** para leer el resultado y **Historia** para volver a una corrida o compararla con la anterior. Una señal no observada en otra corrida no se considera automáticamente corregida.

## Qué archivos aportan evidencia

| Archivos | Qué puede aportar openSeec | Límite que se conserva visible |
|---|---|---|
| C# | Reglas propias de seguridad, grafo e inventario de entradas ASP.NET | Las reglas y patrones medidos son un alcance concreto, no un análisis completo del programa |
| Razor `.cshtml` | Señales de `Html.Raw` dentro del alcance de la regla | Cobertura parcial; no equivale a revisar todo Razor ni el C# embebido |
| `appsettings*.json` | Secretos y observaciones de configuración por entorno | La lectura estática no demuestra la configuración efectiva de una aplicación desplegada |
| `Program.cs` | Middleware HTTPS/HSTS observado e información de entradas | Una llamada no visible no prueba ausencia: puede estar configurada en otro lugar |
| `launchSettings.json` | Perfiles declarados y configuración HTTP de producción | No demuestra cómo está desplegada la aplicación |
| `packages.lock.json` | Versiones resueltas y relaciones admitidas por el lector | Un formato o dato no soportado queda explícito |
| `obj/project.assets.json` existente | Dependencias resueltas, transitivas y metadatos admitidos | Se lee el archivo existente; openSeec no lo genera ni restaura paquetes |
| `packages.config` | Paquetes y versiones instaladas declaradas | No demuestra por sí solo la relación directa/transitiva |
| `.csproj` sin resolución | Referencias y propiedades literales dentro del alcance admitido | Las versiones declaradas pueden ser mínimos; no se presentan como resolución completa ni cobertura de transitivas |

El análisis estructural de C# usa tree-sitter. Cuando debe recurrir al lexer, el mapa de amenazas muestra qué archivos usaron ese camino y el motivo. Si no se pudo interpretar un archivo, queda como tal. Para la alcanzabilidad de NuGet, un namespace no se adivina a partir del nombre del paquete: si falta evidencia local suficiente, el resultado queda desconocido.

## Base de avisos

OSV se consulta sobre un snapshot local fijado para cada corrida. Actualizar la base no envía código del proyecto ni modifica un informe anterior. La actualización se hace separada del análisis:

```powershell
./ops/update-osv.ps1 -Run
./ops/update-osv.ps1 -Status
```

La tarea diaria es opcional y requiere registrarla expresamente. La vigencia de una base no programa una descarga. Una base vencida, inválida o sin NuGet no se presenta como cero avisos.

## Leer un resultado incompleto

El aviso de revisión incompleta y la cobertura forman parte del resultado. Revisá qué motor y qué archivos faltaron antes de interpretar la cantidad de señales. En secretos, el valor se mantiene oculto. Una hipótesis de un modelo, una coincidencia de una regla y una corrección verificada son evidencias distintas.

