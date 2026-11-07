# Sistema de control de asistencia - SG Aron's EIRL (Pollón Dorado)

Proyecto del curso Fundamentos de Programación (CIIN1205P) - UPN.
Es un prototipo en Python para el restaurante Pollón Dorado, con menú de consola y también con interfaz web que se abre en Chrome.

### 1. CORE / Problemática

En SG Aron's EIRL el control de asistencia se hace en un cuaderno. Ese es el problema central.

- A veces no anotan la hora exacta de entrada y salida.
- No se controlan las tardanzas porque hay 2 turnos (Mañana y Tarde) y cada uno entra a distinta hora.
- La planilla se saca a mano al final del mes, contando horas con calculadora y se equivocan.
- No hay reportes, si quieren ver faltas tienen que revisar el cuaderno hoja por hoja.

Eso genera que se pague mal, que haya reclamos y desorden en los turnos.

### 2. Objetivo general

Hacer un sistema en Python que registre las entradas y salidas, que detecte solo si es tardanza según el turno, y que calcule al final del mes cuantas horas trabajó cada persona y cuanto se le paga. Todo se exporta a Excel.

### 3. Objetivos específicos

- Registrar trabajadores con código tipo E001, nombres, DNI, cargo, turno y tarifa por hora.
- Marcar entrada y salida por turno.
- Calcular horas trabajadas y si hubo tardanza (Mañana tope 08:00, Tarde tope 14:00).
- Guardar datos en CSV para que no se pierda al cerrar.
- Generar planilla mensual (horas x tarifa).
- Exportar reporte a Excel con 3 hojas: Empleados, Asistencias y Planilla mensual.
- Cumplir con lo del curso: búsqueda binaria recursiva, burbuja, selección y recursividad.
- Ofrecer una interfaz web (Flask + HTML) que se abre sola en Chrome a pantalla completa y usa los mismos módulos.
- Trabajar todo en UTF-8 para aceptar tildes, ñ y otros caracteres especiales.

### 4. Integrantes

- Arias Correa, Evelin Milagritos - modulo_validaciones.py y pruebas.py
- Luicho Mosqueira, Hector Alexander - modulo_trabajadores.py
- Mosqueira Tanta, Saul Alexander - modulo_asistencia.py
- Ocas Perez, Daniel Alexander - modulo_persistencia.py y modulo_reportes.py
- Valdez Quiroz, Victor Paul - modulo_planilla.py y main.py

### 5. Requisitos

Python 3.8 o superior.
Instalar openpyxl (para el Excel) y flask (para la interfaz web):

pip install -r requirements.txt

Si no funciona:

pip install openpyxl flask

### 6. Como ejecutar

Abrir la carpeta codigos en terminal y poner:

python main.py     # abre el menú principal en la consola
python app_web.py  # abre la interfaz web en Chrome (pantalla completa, F11 para salir)
python pruebas.py  # corre las 36 pruebas, tiene que salir 36/36

La interfaz web queda en http://127.0.0.1:5000 y la terminal debe seguir abierta mientras se usa (Ctrl + C la detiene).

### 7. Estructura de carpetas

codigos/
├── main.py                  Menú principal que integra todo (consola)
├── app_web.py               Interfaz web (Flask): conecta la página con los módulos
├── templates/
│   └── index.html           Página web (HTML + CSS): Trabajadores, Asistencia y Planilla mensual
├── modulo_validaciones.py   Reglas de validación (código, DNI, turno, fecha, hora, mes)
├── modulo_trabajadores.py   Registro, búsqueda (lineal y binaria recursiva) y orden burbuja
├── modulo_asistencia.py     Entrada/salida, horas, tardanza por turno, orden por selección
├── modulo_planilla.py       Horas del mes por turno y pago mensual
├── modulo_persistencia.py   Lectura y escritura de archivos CSV (UTF-8)
├── modulo_reportes.py       Exportación a Excel (.xlsx)
├── pruebas.py               Casos de prueba automáticos
├── data/                    Aquí se guardan los CSV y el Excel (se crea sola)
├── requirements.txt
└── README.md

### 8. Menú

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

### 9. Interfaz web

Se ejecuta con `python app_web.py` y tiene 3 pestañas:

| Pestaña | Qué permite |
|---|---|
| Trabajadores | Registrar trabajador, listar y buscar por código (búsqueda binaria recursiva) |
| Asistencia | Marcar entrada y salida, filtrar por código, ordenar por fecha y ver el total de tardanzas (las tardanzas salen en rojo) |
| Planilla mensual | Generar la planilla de un mes (horas por turno × tarifa), ver el total y descargar el Excel |

Usa los mismos módulos y los mismos CSV que el menú de consola, así que ambos se pueden usar con los mismos datos. El HTML y el CSS están en `templates/index.html`; `app_web.py` solo conecta los botones con los módulos.

### 10. Reglas de negocio

- La empresa trabaja con **2 turnos**: `M` (Mañana) y `T` (Tarde).
- Hora tope de entrada por turno: **Mañana 08:00** y **Tarde 14:00**. Son valores de ejemplo configurables en `HORA_TOPE_POR_TURNO` (`modulo_asistencia.py`); deben confirmarse con la gerencia.
- Una entrada posterior al tope de **su turno** se registra como tardanza.
- El pago es **mensual**: horas trabajadas en el mes × tarifa por hora del trabajador.
- Formatos: código `E###` (ejemplo `E001`), DNI de 8 dígitos, fecha `AAAA-MM-DD`, hora `HH:MM`, mes `AAAA-MM`.
- Cargos válidos: Parrillero, Mesero, Caja, Cocina y Administracion.

### 11. Datos y privacidad

Los archivos de la carpeta `data/` contienen datos personales (DNI, horarios y tarifas), protegidos por la Ley N.° 29733 (Protección de Datos Personales). Por eso el `.gitignore` **no sube** los `.csv` ni los `.xlsx` a GitHub. Si el Excel está abierto, el programa avisa y no se cierra: hay que cerrarlo y volver a exportar.

**Codificación UTF-8:** todo el proyecto usa UTF-8 (código con la cabecera `# -*- coding: utf-8 -*-`, CSV y página web), por lo que se aceptan tildes, ñ, ü, ç y cualquier letra en nombres y apellidos. Los CSV se guardan como UTF-8 con BOM (`utf-8-sig`, constante `CODIFICACION` en `modulo_persistencia.py`) para que Excel los abra sin deformar los acentos; al leer se aceptan archivos con o sin BOM.

**Portabilidad:** no hay base de datos; los datos son archivos CSV dentro de `data/` y no hay rutas fijas, por lo que el proyecto corre en otra computadora con solo Python, openpyxl y flask. Como `data/` no se sube a GitHub, para llevar los datos hay que copiar esa carpeta a mano.

### 12. Temas del curso que usamos

if, while, funciones, módulos, listas de diccionarios, manejo de archivos csv y excel, try/except, recursividad, ordenamiento burbuja y selección. Como extra, una interfaz web con Flask y HTML.

### 13. Trabajo con Git

Cada integrante trabaja en su propia rama (`feature/validaciones`, `feature/empleados`, `feature/asistencia`, `feature/persistencia-reportes`, `feature/main-integracion`) y la une a `main` con un Pull Request.

### 14. Limitaciones

Interfaz de consola y web local sin usuarios ni contraseñas; la web corre en una sola computadora (un usuario a la vez, porque los datos están en CSV); la hora la digita una persona; no se contemplan turnos que cruzan la medianoche, horas extra, feriados ni la sucursal de cada trabajador.