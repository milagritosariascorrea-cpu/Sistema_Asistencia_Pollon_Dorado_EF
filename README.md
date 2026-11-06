# Sistema de control de asistencia — SG Aron's EIRL (Pollón Dorado)

Prototipo en **Python 3** para registrar la asistencia de los trabajadores de un restaurante con **2 turnos** (Mañana y Tarde) y calcular la **planilla mensual** (pago por horas trabajadas). Los datos se guardan en archivos CSV y el reporte se exporta a **Excel**.

Proyecto del curso **Fundamentos de Programación (CIIN1205P)** — Universidad Privada del Norte.

## Integrantes

| Integrante | Módulos |
|---|---|
| Arias Correa, Evelin Milagritos | `modulo_validaciones.py`, `pruebas.py` |
| Luicho Mosqueira, Hector Alexander | `modulo_trabajadores.py` |
| Mosqueira Tanta, Saul Alexander | `modulo_asistencia.py` |
| Ocas Perez, Daniel Alexander | `modulo_persistencia.py`, `modulo_reportes.py` |
| Valdez Quiroz, Victor Paul | `modulo_planilla.py`, `main.py` |

## Requisitos

- Python 3.8 o superior
- La librería `openpyxl` (solo para exportar a Excel):

```
pip install -r requirements.txt
```

## Cómo ejecutar

Abrir una terminal dentro de esta carpeta y ejecutar:

```
python main.py        # programa principal (menú)
python pruebas.py     # 35 casos de prueba automáticos
```

Al terminar `pruebas.py` debe mostrar `RESUMEN: 35/35 casos correctos`.

## Estructura del proyecto

```
codigos/
├── main.py                  Menú principal que integra todo
├── modulo_validaciones.py   Reglas de validación (código, DNI, turno, fecha, hora, mes)
├── modulo_trabajadores.py   Registro, búsqueda (lineal y binaria recursiva) y orden burbuja
├── modulo_asistencia.py     Entrada/salida, horas, tardanza por turno, orden por selección
├── modulo_planilla.py       Horas del mes por turno y pago mensual
├── modulo_persistencia.py   Lectura y escritura de archivos CSV
├── modulo_reportes.py       Exportación a Excel (.xlsx)
├── pruebas.py               Casos de prueba automáticos
├── data/                    Aquí se guardan los CSV y el Excel (se crea sola)
├── requirements.txt
└── README.md
```

## Menú del programa

| Opción | Acción |
|---|---|
| 1 | Registrar trabajador (código, nombres, DNI, cargo, turno habitual y tarifa por hora) |
| 2 | Listar trabajadores |
| 3 | Buscar trabajador por código (búsqueda binaria recursiva) |
| 4 | Marcar entrada (el turno de la jornada puede ser distinto al habitual) |
| 5 | Marcar salida y calcular las horas trabajadas |
| 6 | Ver las asistencias de un trabajador |
| 7 | Ver todas las asistencias ordenadas por fecha (ordenamiento por selección) |
| 8 | Contar tardanzas (recursivo) |
| 9 | Generar la planilla mensual (horas por turno × tarifa por hora) |
| 10 | Exportar el reporte completo a Excel (hojas Empleados, Asistencias y Planilla mensual) |
| 0 | Salir (guarda todos los datos) |

## Validación de los datos al escribirlos

Cada dato se pide en un bucle: si es inválido, el programa explica el error y **repite la pregunta**; no avanza hasta recibir un dato correcto. Para volver al menú sin guardar, se escribe `cancelar`.

| Dato | Regla |
|---|---|
| Código | La letra E y 3 dígitos (`E001`); al registrar no puede repetirse y al marcar asistencia debe existir |
| Nombres y apellidos | Solo letras (con tildes y ñ), espacios, guion o apóstrofe; no se aceptan números ni símbolos como `@` |
| DNI | Exactamente 8 dígitos |
| Cargo y turno | Un cargo de la lista y turno `M` o `T` |
| Tarifa por hora | Número mayor que 0 |
| Fecha, hora y mes | `AAAA-MM-DD` (fecha real), `HH:MM` y `AAAA-MM` |

## Reglas de negocio

- La empresa trabaja con **2 turnos**: `M` (Mañana) y `T` (Tarde).
- Hora tope de entrada por turno: **Mañana 08:00** y **Tarde 14:00**. Son valores de ejemplo: deben confirmarse con la gerencia y se cambian en `HORA_TOPE_POR_TURNO` (ver «Cómo cambiar las horas tope»).
- Una entrada posterior al tope de **su turno** se registra como tardanza.
- El pago es **mensual**: horas trabajadas en el mes × tarifa por hora del trabajador.
- Formatos: código `E###` (ejemplo `E001`), DNI de 8 dígitos, fecha `AAAA-MM-DD`, hora `HH:MM`, mes `AAAA-MM`.
- Cargos válidos: Parrillero, Mesero, Caja, Cocina y Administracion.

## Cómo cambiar las horas tope de cada turno

Las horas tope están en `HORA_TOPE_POR_TURNO` (`modulo_asistencia.py`, líneas 19 a 22). Para usar las horas reales de la gerencia, cambien solo esos dos valores en formato `HH:MM`:

```python
HORA_TOPE_POR_TURNO = {
    "Mañana": "08:00",
    "Tarde": "14:00",
}
```

Las pruebas fijan sus propios topes, así que `pruebas.py` sigue dando 35/35 aunque cambien estos valores.

## Si la planilla sale en 0

La planilla solo suma jornadas **cerradas** (con hora de salida) del **mes** que se pide. El programa muestra un aviso y la causa suele ser una de estas:

| Aviso o síntoma | Qué hacer |
|---|---|
| «N jornada(s) sin hora de salida» | Marcar la salida con la opción 5 (misma fecha que la entrada) |
| «no tiene asistencias en AAAA-MM» y se muestra la lista de meses con datos | Pedir un mes que aparezca en esa lista |
| «asistencias de una version anterior no tenian turno» al iniciar | No hace falta hacer nada: se asigna el turno habitual del trabajador |

Para revisar las jornadas de un trabajador use la opción 6.

## Datos y privacidad

Los archivos de la carpeta `data/` contienen datos personales (DNI, horarios y tarifas), protegidos por la Ley N.° 29733 (Protección de Datos Personales). Por eso el `.gitignore` **no sube** los `.csv` ni los `.xlsx` a GitHub. Si el Excel está abierto, el programa avisa y no se cierra: hay que cerrarlo y volver a exportar.

## Temas del curso aplicados

Estructuras de control (menú con `while` e `if/elif`), funciones y módulos, arreglos (listas de diccionarios), cadenas de caracteres, archivos CSV y Excel, recursividad (búsqueda binaria y conteo de tardanzas por mitades), ordenamiento (burbuja y selección) y manejo de excepciones (`try/except/finally`).

## Trabajo en equipo con Git

Cada integrante trabaja en su propia rama (`feature/validaciones`, `feature/empleados`, `feature/asistencia`, `feature/persistencia-reportes`, `feature/main-integracion`) y la une a `main` con un Pull Request.

## Limitaciones conocidas

Interfaz de consola sin usuarios ni contraseñas; la hora la digita una persona; no se contemplan turnos que cruzan la medianoche, horas extra, feriados ni la sucursal de cada trabajador.
