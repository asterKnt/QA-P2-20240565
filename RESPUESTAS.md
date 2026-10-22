# Respuestas - Parcial 2 (Parte Práctica)

## P2. Fallas del Jenkinsfile

1. **Orden de etapas invertido:** `stage('Build image')` estaba colocado antes de `stage('Regresion')`, lo que provocaba que la imagen de Docker se construyera antes de verificar la calidad del código, impidiendo que las pruebas funcionaran como compuerta de entrega (gate).
2. **Enmascaramiento del código de salida:** El comando de regresión utilizaba `|| true` (`bru run ... || true`), lo que forzaba un código de salida exitoso (0) y causaba que Jenkins marcara la etapa y el build como exitosos aun cuando fallaran aserciones.
3. **Ruta incorrecta del reporte JUnit:** La directiva `junit` en `post { always { ... } }` apuntaba a `bruno/smoke/results.xml` en lugar de `bruno/regresion/results.xml`, por lo que Jenkins no publicaba ni contabilizaba los resultados de la suite de regresión en la pestaña Test Result.

---

## P3. Evidencia de Builds

| Paso | Descripción | Build | SHA | Estado |
|---|---|:---:|---|:---:|
| 1 | Push del `Jenkinsfile` corregido (sin prueba P1) | #4 | `cf329f2a8cf75256c8226158d7182f1290f31b5a` | SUCCESS |
| 2 | Push de la prueba P1 en `bruno/regresion/` | #5 | `aedf8427ebf2ec58fb10c420c4dda1a9b234d86b` | FAILURE |
| 3 | Arreglo de la regla de cobro en `api.py` | #6 | `5a36591891ba655082c1572a8077ba8029574167` | SUCCESS |

---

## Aserción Fallida en el Build Rojo (Build #5)

- **Nombre de la aserción en Test Result:** `res.body.monto eq 60`
- **Request / Archivo:** `14-cobro-con-tope`
- **Detalle del fallo:** `expected 80 to equal 60`
