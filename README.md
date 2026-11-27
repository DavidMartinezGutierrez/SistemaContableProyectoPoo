# Sistema Académico UTS

Sistema de gestión académica y control de acceso basado en roles (RBAC) para la comunidad universitaria de las Unidades Tecnológicas de Santander (UTS), desarrollado en Java bajo el paradigma de Programación Orientada a Objetos (POO) y conectado a PostgreSQL mediante JDBC. Permite la administración integral de estudiantes, docentes, materias del pensum, horarios por franjas y salones, calificaciones por cortes y control de asistencias.

## Información Académica
* **Asignatura:** Programación Orientada a Objetos & Motores de Bases de Datos
* **Docente:** Mag. Carlos Adolfo Beltrán Castro
* **Periodo:** 2026-2
* **Programa:** Tecnología en Desarrollo de Sistemas Informáticos (Pensum TSI2019-2)
* **Institución:** Unidades Tecnológicas de Santander (UTS)

## Tecnologías y Requisitos
* **Lenguaje:** Java JDK 17 o superior
* **IDE Recomendado:** Apache NetBeans 18+ (compatible con IntelliJ IDEA y Eclipse)
* **Gestor de Construcción:** Apache Maven
* **Base de datos:** PostgreSQL 14+ / Gestor pgAdmin 4
* **Interfaz Gráfica:** Java Swing (Look & Feel moderno con paleta institucional UTS en tema oscuro)
* **Conector:** PostgreSQL JDBC Driver (`postgresql-42.7.2.jar`)
* **Seguridad:** jBCrypt (`0.4`) para cifrado y verificación de contraseñas

## Estructura del Proyecto (Arquitectura POO)
```text
src/main/java/uts/academico/
├── model/              # Clases de entidad del dominio con herencia y encapsulamiento
│   ├── Usuario.java    # Clase abstracta base (documento, correo, passwordHash, rol, etc.)
│   ├── Administrador.java # Subclase con privilegios totales y auditoría
│   ├── Docente.java    # Subclase con gestión de notas y asistencias
│   ├── Estudiante.java # Subclase con perfil, créditos, promedio y pensum
│   ├── Asignatura.java # Materias curriculares (código, créditos, semestre)
│   ├── Horario.java    # Franjas semanales y asignación de aulas físicas
│   ├── Calificacion.java # Registro de notas por corte (30%-30%-40%) y definitiva
│   ├── Asistencia.java # Control diario de asistencias (PRESENTE, AUSENTE, JUSTIFICADO)
│   ├── Auditoria.java  # Entidad para registro de auditoría académica
│   └── Rol.java        # Enum con roles del sistema (ADMIN, DOCENTE, ESTUDIANTE)
├── dao/                # Clases de acceso a datos, PreparedStatement y CallableStatement
│   ├── UsuarioDAO.java # Autenticación, carga polimórfica y CRUD de usuarios
│   └── AcademicoDAO.java # Ejecución de SELECT, Funciones (fn_...) y Procedimientos (sp_...)
├── service/            # Capa de lógica de negocio y control de permisos por rol (RBAC)
│   ├── AuthService.java    # Gestión de sesión activa y verificación de permisos
│   └── AcademicoService.java # Reglas de negocio académicas y orquestación DAO
├── ui/                 # Interfaces gráficas de usuario en Java Swing
│   ├── LoginFrame.java     # Autenticación segura con selector de rol visual
│   ├── DashboardPrincipal.java # Panel dinámico adaptado según el rol autenticado
│   ├── UITheme.java        # Constantes de diseño, colores institucionales y fuentes
│   ├── AvatarBannerPanel.java # Componente gráfico del perfil con banner y avatar circular
│   └── BarraProgresoCreditos.java # Barra de progreso visual para créditos aprobados
├── util/               # Conexión JDBC y utilidades del sistema
│   └── ConexionBD.java # Gestión de conexión Singleton y cierre seguro de recursos
└── Main.java           # Punto de entrada de la aplicación
```

## Configuración y Ejecución Local

### 1. Base de Datos (pgAdmin 4)
1. Abrir **pgAdmin 4** y autenticarse con el superusuario `postgres`.
2. Crear una nueva base de datos llamada:
   ```sql
   uts_sistema_academico
   ```
3. Seleccionar la base de datos `uts_sistema_academico` y abrir la herramienta **Query Tool** (ícono de rayo ⚡).
4. Abrir y ejecutar el script completo ubicado en:
   ```text
   database/sistema_academico_uts.sql
   ```
   *(Presionar **F5** para ejecutar. Este script crea automáticamente las tablas normalizadas, funciones de negocio, procedimientos almacenados, triggers de validación/auditoría y datos iniciales de prueba).*

### 2. Conexión en el Código
Antes de ejecutar la aplicación, verifica o ajusta las credenciales locales de PostgreSQL en el archivo `src/main/java/uts/academico/util/ConexionBD.java`:
```java
private static String host = "localhost";
private static String puerto = "5432";
private static String baseDatos = "uts_sistema_academico";
private static String usuario = "postgres";
private static String password = "admin"; // Modificar por tu contraseña local de PostgreSQL
```
> **Nota:** También puedes configurar las credenciales en tiempo de ejecución desde la pantalla de Login mediante el botón **"⚙ Configurar Conexión PostgreSQL..."** sin necesidad de recompilar.

### 3. Ejecución
1. Clonar el repositorio o descargar el código fuente:
   ```bash
   git clone https://github.com/TU_USUARIO/SistemaAcademicoUTS.git
   ```
2. Abrir el proyecto en **Apache NetBeans** (**File → Open Project...** y seleccionar la carpeta del proyecto con ícono de Maven).
3. Asegurar que las dependencias se descarguen correctamente (`pom.xml` incluye el driver `postgresql-42.7.2` y `jbcrypt-0.4`).
4. Hacer clic derecho sobre el proyecto → **Clean and Build**.
5. Ejecutar la clase principal `Main.java` (o presionar **Shift + F6**).

## Funcionalidades Implementadas
* **Control de Acceso Basado en Roles (RBAC):** Inicio de sesión con autenticación polimórfica para Administrador, Docente y Estudiante; vista dinámica según permisos.
* **Módulo de Perfil del Estudiante:** Visualización de foto/avatar institucional, información académica (Jornada, Situación, Categoría, Pensum), barra de progreso de créditos aprobados y promedio acumulado con recálculo dinámico invocando funciones de PostgreSQL (`fn_calcular_promedio_estudiante` y `fn_porcentaje_creditos`).
* **Módulo de Horario Semanal estilo UTS:** Grilla de Lunes a Sábado con franjas horarias oficiales (`07:30-08:59`, `09:00-10:29`, etc.) y asignación de salones físicos (`312 C`, `SINF 315 A`, `202 C`).
* **Módulo de Calificaciones:** Consulta para estudiantes y formulario de registro/edición para docentes invocando el procedimiento almacenado `sp_registrar_o_actualizar_nota` (ponderación UTS 30% - 30% - 40%).
* **Control de Asistencia:** Registro de asistencias mediante `sp_registrar_asistencia`, consulta de porcentaje con `fn_porcentaje_asistencia` y diálogo de consolidado de asistencia con alerta semafórica de riesgo de pérdida por inasistencia (< 80%).
* **Módulo CRUD y Auditoría:** Gestión de usuarios y matrículas con `sp_matricular_estudiante`, y visualización en tiempo real de registros de auditoría (`auditoria_academica`) generados por triggers en formato JSONB.
* **Validación e Integridad mediante Triggers:** Trigger `trg_validar_rango_nota` para asegurar que las notas estén entre 0.00 y 5.00, y triggers de auditoría automática ante eventos `INSERT`, `UPDATE` o `DELETE`.

## Usuarios de Prueba (Credenciales)
> **Contraseña universal para todos los usuarios:** `Uts2026*`

| Rol | Nombre Completo | Correo Institucional | Contraseña |
|---|---|---|---|
| **Administrador** | Ing. Germán Darío Pinzón Blanco | `gpinzonblanco@uts.edu.co` | `Uts2026*` |
| **Docente** | Ing. Ricardo Morales Sandoval | `rmoraless@uts.edu.co` | `Uts2026*` |
| **Docente** | Ing. Claudia Patricia Peña Suárez | `cpenasuarez@uts.edu.co` | `Uts2026*` |
| **Estudiante** | Andrés Felipe Vargas Rojas | `afvargasrojas@uts.edu.co` | `Uts2026*` |
| **Estudiante** | Camila Andrea Pardo Méndez | `capardomendez@uts.edu.co` | `Uts2026*` |
| **Estudiante** | Santiago Gómez Villamizar | `sgomezvillamizar@uts.edu.co` | `Uts2026*` |

## Autores

| Estudiante | C.C. | Módulo Asignado |
|---|---|---|
| Kevin Alexander Moreno Rodríguez | 1.095.306.177 | Módulo Perfil Académico |
| Andrés Felipe Mendoza | 1.095.920.061 | Módulo Horario - Materia |
| Juan Felipe Martínez Sánchez | 1.121.202.374 | Módulo Calificaciones |
| Juan David Martínez Gutiérrez | 1.025.539.526 | Módulo Asistencias |
