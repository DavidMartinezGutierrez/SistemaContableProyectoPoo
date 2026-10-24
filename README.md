# Gestión Veterinaria MVC

**Proyecto del Taller de Panel Administrativo**  
Tecnología de Desarrollos de Sistemas Informáticos  
📅 II Semestre 2026  
👨‍🏫 Profesor: Mag. Carlos Adolfo Beltrán Castro  
👨‍💻 Estudiantes:

* Kevin Alexander Moreno Rodríguez — C.C. 1.095.306.177 — *Módulo 1: Clientes*
* Andrés Felipe Mendoza — C.C. 1.095.920.061 — *Módulo 2: Mascotas*
* Juan Felipe Martínez Sánchez — C.C. 1.121.202.374 — *Módulo 3: Veterinarios*
* Juan David Martínez Gutiérrez — C.C. 1.025.539.526 — *Módulo 4: Medicamentos*

![Menú principal](docs/menu_principal.png)  
*Imagen de pantalla inicial con el menú del proyecto*

---

## 🚀 Descripción del Proyecto

Este proyecto simula un sistema de **gestión para una clínica veterinaria**, desarrollado con **Java SE** y **Swing**. Incluye un menú principal desde el cual se navega a cuatro módulos, cada uno con su propio **CRUD** (Crear, Leer, Actualizar y Eliminar): **Clientes**, **Mascotas**, **Veterinarios** y **Medicamentos**.

La aplicación sigue el patrón de arquitectura **MVC (Modelo – Vista – Controlador)** y guarda la información en una base de datos **SQLite** a través de **JPA (EclipseLink)**, sin escribir sentencias SQL en el código.

**Funcionalidades principales**

* Registro, consulta, edición y eliminación de datos en cada módulo.
* Tabla de listado por módulo; al hacer clic en una fila, sus datos pasan al formulario para editarlos.
* Validación de campos obligatorios y de datos numéricos (por ejemplo, la edad de la mascota debe estar entre 0 y 40 años; el precio del medicamento no puede ser negativo).
* Mensajes informativos, de advertencia y de confirmación antes de eliminar un registro.
* Identificador (`id`) autoincremental, calculado por la base de datos.

## 📂 Estructura del Proyecto

### Menú principal

**Lista de Menú de Opciones** con título e ítems de navegación:

* **1. Clientes**
* **2. Mascotas**
* **3. Veterinarios**
* **4. Medicamentos**
* **Salir**

### Vistas

Cada módulo abre una ventana con tres partes: un **formulario** de datos, una **botonera CRUD** (*Guardar, Actualizar, Eliminar, Limpiar/Nuevo*) y una **tabla** con los registros.

| Módulo | Ventana | Campos |
|---|---|---|
| Clientes | `FrmCliente` | nombres, documento, teléfono, correo |
| Mascotas | `FrmMascota` | nombre, especie (lista desplegable), raza, edad |
| Veterinarios | `FrmVeterinario` | nombres, especialidad, teléfono, correo |
| Medicamentos | `FrmMedicamento` | nombre, laboratorio, precio, stock |

**Capturas de los módulos**

| | |
|---|---|
| ![Clientes](docs/modulo_clientes.png)<br>*Módulo 1 – Clientes* | ![Mascotas](docs/modulo_mascotas.png)<br>*Módulo 2 – Mascotas* |
| ![Veterinarios](docs/modulo_veterinarios.png)<br>*Módulo 3 – Veterinarios* | ![Medicamentos](docs/modulo_medicamentos.png)<br>*Módulo 4 – Medicamentos* |

**Salir** cierra la conexión con la base de datos (`EntityManagerFactory`) y termina la aplicación.

### Organización de carpetas (MVC)

```
GestionVeterinariaMVC/
├── bd_veterinaria.db          # Base de datos SQLite
├── bd_veterinaria.sql         # Script de creación de tablas y datos de prueba
├── lib/                       # Librerías (sqlite-jdbc, slf4j-api)
└── src/
    ├── META-INF/persistence.xml   # Configuración de JPA
    ├── principal/                 # Main (punto de entrada)
    ├── modelo/                    # Entidades: Cliente, Mascota, Veterinario, Medicamento
    ├── vista/                     # Formularios Swing (FrmMenuPrincipal, FrmCliente, ...)
    ├── controlador/               # Controladores MVC (ControladorMenu, ControladorCliente, ...)
    └── persistencia/              # JpaController por entidad + SQLitePlatform + excepciones
```

| Capa | Responsabilidad |
|---|---|
| **Modelo** (`modelo`) | Clases que representan las tablas (entidades JPA con anotaciones `@Entity`). |
| **Vista** (`vista`) | Construye las ventanas Swing. No conoce JPA ni la base de datos. |
| **Controlador** (`controlador`) | Escucha los botones, valida los datos y conecta la vista con la persistencia. |
| **Persistencia** (`persistencia`) | Operaciones `create`, `edit`, `destroy` y consultas sobre la base de datos. |

### 🗄️ Modelo de datos

Base de datos `bd_veterinaria` (SQLite) con cuatro tablas, cada una con su llave primaria autoincremental:

| Tabla | Columnas |
|---|---|
| **Cliente** | `id` (PK), nombres, documento, telefono, correo |
| **Mascota** | `id` (PK), nombre, especie, raza, edad |
| **Veterinario** | `id` (PK), nombres, especialidad, telefono, correo |
| **Medicamento** | `id` (PK), nombre, laboratorio, precio, stock |

![Diagrama Entidad-Relación](docs/diagrama_ER_bd_veterinaria.png)  
*Diagrama Entidad-Relación de la base de datos*

> En esta versión las cuatro tablas son independientes (sin llaves foráneas), ya que cada módulo se desarrolla por separado.

## 🧰 Lista de Tecnologías Usadas

| Tecnología | Versión | Uso |
|---|---|---|
| **Java SE (JDK)** | 17 | Lenguaje de programación |
| **Java Swing** | incluido en el JDK | Interfaz gráfica de usuario |
| **Patrón MVC** | — | Modelo, Vista y Controlador, con una capa de persistencia JPA |
| **JPA** (Java Persistence API) | 2.1 | Mapeo objeto-relacional |
| **EclipseLink** | 2.7.12 | Proveedor de persistencia (el que trae NetBeans) |
| **SQLite** | 3.45.1 | Motor de base de datos local en archivo (`bd_veterinaria.db`) |
| **sqlite-jdbc** (Xerial) | 3.45.1.0 | Controlador JDBC para SQLite |
| **slf4j-api** | 1.7.36 | Librería de registro que pide el driver |
| **Java with Ant** | — | Tipo de proyecto de escritorio y sistema de construcción |
| **NetBeans IDE** | 29 | Entorno de desarrollo |

## 🔧 Instalación y ejecución

### Requisitos previos

* **JDK 17** o superior.
* **NetBeans IDE 29** (proyecto *Java with Ant*).
* *(Opcional)* cliente de línea de comandos `sqlite3` para recrear la base de datos.

### Pasos

1. Descargar y descomprimir el proyecto (o clonar el repositorio):

   ```bash
   git clone <URL_DEL_REPOSITORIO>
   ```

2. En NetBeans: **File > Open Project...** y seleccionar la carpeta `GestionVeterinariaMVC`.

3. Si NetBeans marca librerías faltantes (**Resolve Problems**), verificar en **Properties > Libraries** que estén:
   * `lib/sqlite-jdbc-3.45.1.0.jar`
   * `lib/slf4j-api-1.7.36.jar`
   * la librería **EclipseLink (JPA 2.1)**

4. Verificar que el archivo `bd_veterinaria.db` esté en la **raíz del proyecto**, porque la conexión usa la ruta relativa `jdbc:sqlite:bd_veterinaria.db`.

5. Ejecutar con **Run (F6)**. La clase principal es `principal.Main`.

### Recrear la base de datos (opcional)

Desde la carpeta del proyecto:

```bash
sqlite3 bd_veterinaria.db < bd_veterinaria.sql
```

### Solución de problemas

| Error | Causa y solución |
|---|---|
| `NoClassDefFoundError: org/slf4j/LoggerFactory` | Falta `slf4j-api-1.7.36.jar` en las librerías del proyecto. |
| «No se pudo iniciar la unidad de persistencia» | Revisar que `bd_veterinaria.db` esté en la raíz y que la librería EclipseLink esté agregada. |

## 📝 Notas técnicas

* **EclipseLink no trae plataforma para SQLite.** La clase `SQLitePlatform` le enseña a leer el `id` generado con `last_insert_rowid()`, necesario para `@GeneratedValue(strategy = GenerationType.IDENTITY)`.
* Todas las ventanas **comparten una sola `EntityManagerFactory`**, creada en `Main` y cerrada al salir del menú.
* La interfaz usa el *Look and Feel* nativo del sistema operativo.
