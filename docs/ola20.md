# Ola20: alcance y límites

Actualizado:2026-11-01. Versión del producto:0.24.0. La integración en main no
equivale a una release aceptada ni a cobertura completa.

## Disponible en el código

- La API local admite exclusiones de rutas, hipótesis del usuario y notas de informe.
  El formulario todavía no está conectado a la app. Las hipótesis quedan sin investigar;
  las notas no se consideran verificadas y ninguna acción apaga motores.
  Esta capacidad está deshabilitada en cloud.
- Las reglas C# incorporan fuentes y receptores tipados System.Web para comandos,
  LDAP y redirecciones. El caso público de redirección quedó detectado sin cambiar
  su oráculo. Son controles DEV, no un holdout nuevo. HttpCookie conserva un falso
  negativo condicional porque no se observó su configuración efectiva.
- La integración CI local conserva códigos0/1/2 y no ejecuta el repositorio analizado.

## Evidencia acotada

React:2285 identidades y185 relaciones directas contrastadas con oráculo independiente.
Las otras2100 relaciones permanecen desconocidas, sin transitivas confirmadas en esa
medición. Esto no mide vulnerabilidades. Un lockfile YAML multidocumento sigue siendo
no soportado; no se interpreta como cero dependencias ni cero avisos.

Las fixtures canónicas verificaron34/34 hashes y30/30 pruebas focales. Quedan pendientes
la global y la localidad de ola20. No se aceptaron nuevas referencias de baseline.

## Prototipos, sin eliminación

La retención OSV sólo planifica. Los leases cooperativos se midieron en Windows y
Ubuntu WSL, pero sus consumidores siguen pendientes de integración. No se acreditó
interoperabilidad de un mismo store entre ambos sistemas ni aislamiento de producción.
El inventario de dependencias declaradas no certifica licencias ni cumplimiento
CycloneDX/SPDX y no realiza evaluación de seguridad.

## Imagen de cobertura

El porcentaje sigue en88%:52 de59 celdas que aplican alcanzan «Parcial» o mejor.
No es porcentaje de vulnerabilidades detectadas ni probabilidad de seguridad.
C# conserva evidencia histórica; el alcance agregado en SAST15 figura como parcial
porque los controles nuevos son DEV y no tienen un holdout nuevo.

La imagen clara y oscura del README incluye estos límites. Una señal no confirma
una vulnerabilidad y la ausencia de señales no prueba seguridad.
