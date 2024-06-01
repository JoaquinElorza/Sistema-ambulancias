#Modelo MVC

## Modelo
Incluye clases que representan la estructura de los datos y la lógica de la empresa. Estas clases deben incluyen la lógica para acceder y manipular los datos en la base de datos.
Cada una de las tablas en la base de datos tiene su equivalente en una clase de Java, cada tabla principal cuenta con un CRUD (crear, leer, actualizar y borrar)

**Clases:** `CP` `Conexion` `Direccion` `GeneradorCurp` `Hospital` `Operador` `Personal` `Ambulancia` `Municipio` `Paciente` `Paramedico` `Telefono` `Transcriptor`

---

### Clase de Conexión a la Base de Datos

**Paquete:** `paquete.ambulancias`

**Clase:** `Conexion`

**Propósito:** Esta clase proporciona una única instancia de conexión a la base de datos MySQL para asegurar que todos los accesos a la base de datos se realicen a través de una única conexión compartida, utilizando el patrón Singleton. Esto es útil para evitar múltiples conexiones innecesarias a la base de datos, lo cual puede consumir recursos y complicar la gestión de las conexiones.

#### Atributos de la Clase:

- `private static Conexion instancia;`
  - Mantiene la única instancia de la clase `Conexion`.

- `private Connection conexion;`
  - Almacena la conexión a la base de datos.

- `private String usuario = "root";`
  - Nombre de usuario para la conexión a la base de datos.

- `private String contraseña = "E@e$l@c@d3m1s1";`
  - Contraseña para la conexión a la base de datos.

- `String bd = "tap";`
  - Nombre de la base de datos.

- `String ip = "localhost";`
  - Dirección IP del servidor de la base de datos.

- `String puerto = "3306";`
  - Puerto del servidor de la base de datos.

- `private String url = "jdbc:mysql://"+ip+":"+puerto+"/"+bd;`
  - URL de conexión JDBC construida usando la IP, el puerto y el nombre de la base de datos.

#### Constructores y Métodos:

- **Constructor `private Conexion() throws SQLException`**:
  - Es privado para evitar la creación de instancias fuera de la clase.
  - Carga el driver de MySQL y establece la conexión utilizando `DriverManager.getConnection`.

- **Método `public static Conexion getInstancia() throws SQLException`**:
  - Proporciona el punto de acceso único a la instancia `Conexion`.
  - Si `instancia` es `null` o la conexión está cerrada, crea una nueva instancia de `Conexion`.
  - Devuelve la instancia única de `Conexion`.

- **Método `public Connection getConexion()`**:
  - Devuelve el objeto `Connection` que representa la conexión a la base de datos.

#### Ejemplo de Uso:

Para obtener la conexión a la base de datos en cualquier parte de la aplicación, se puede usar el siguiente código:

```java
try {
    Connection conexion = Conexion.getInstancia().getConexion();
    // Uso de la conexión para operaciones de base de datos
} catch (SQLException e) {
    // Manejo de errores
    JOptionPane.showMessageDialog(null, "Error al conectar con la base de datos: " + e.getMessage());
}
```

#### Manejo de Errores:

- **`Class.forName("com.mysql.cj.jdbc.Driver");`**
  - Carga el driver JDBC de MySQL.
  - Si el driver no se encuentra, lanza una `ClassNotFoundException` que se convierte en `SQLException`.

- **`DriverManager.getConnection(url, usuario, contraseña);`**
  - Establece la conexión a la base de datos utilizando la URL, usuario y contraseña proporcionados.
  - Si la conexión falla, lanza una `SQLException`.

---

## Vista
La **Vista** del patrón MVC en este proyecto está compuesta por los componentes de la interfaz de usuario listados a continuacion. Estos componentes son responsables de la visualización de los datos y la interacción del usuario. Los `JPanel` agrupan diferentes secciones de la interfaz, los `JButton` permiten la ejecución de acciones, los `JTable` muestran datos tabulares, y los `JTextField` y `JPasswordField` permiten la entrada de texto. Los `JRadioButton` y `JComboBox` ofrecen opciones de selección y los `JLabel` proporcionan etiquetas y mensajes descriptivos.

Estos elementos están definidos dentro de la clase que los contiene, y se gestionan mediante controladores que manipulan los datos del modelo para actualizar la vista en función de la lógica de negocio. La implementación asegura que la separación de responsabilidades entre el modelo, la vista y el controlador esté bien definida y clara.
### Componentes de la Vista:

1. **Paneles** (`JPanel`):
2. **Botones** (`JButton`):
3. **Etiquetas** (`JLabel`):
4. **Tablas** (`JTable`):
5. **Campos de texto** (`JTextField`, `JPasswordField`):
6. **Botones de radio** (`JRadioButton`):
7. **Grupos de botones** (`ButtonGroup`):
8. **ComboBox** (`JComboBox`):



### Controlador del Proyecto
El controlador es responsable de manejar la lógica de la aplicación, gestionar la interacción del usuario, actualizar el modelo y seleccionar la vista adecuada para mostrar.

#### Funcionalidades Principales
1. **Autenticación:**
   - Maneja la autenticación del usuario con el botón `btnIniciarSesion`.

2. **Gestión de Hospitales:**
   - **Agregar:** Manejado por `btnAgregarHospital`.
   - **Modificar:** `btnModificarHospitalActionPerformed` realiza la lógica para modificar un hospital existente.
   - **Eliminar:** `btnEliminarHospitalActionPerformed` permite eliminar un hospital.
   - **Selección:** `tbHospitalesMouseClicked` permite seleccionar un hospital de una tabla y cargar sus datos en los campos correspondientes.

3. **Gestión de Ambulancias:**
   - **Agregar:** `btnAgregarAmbulancia`.
   - **Modificar:** `btnModificarAmbulancia`.
   - **Eliminar:** `btnEliminarAmbulancia`.
   - **Visualizar:** Métodos y acciones relacionados con la visualización de ambulancias en tablas.

4. **Gestión de Personal (Paramédicos, Operadores, Capturistas):**
   - **Agregar:** `btnAgregarParamedico`, `btnAgregarOperador`, `btnAgregarCapturistas`.
   - **Modificar:** `btnModificarParamedico`, `btnModificarOperador`.
   - **Eliminar:** `btnEliminarPersonal`.
   - **Visualizar:** Métodos relacionados con la visualización de personal en tablas.

5. **Gestión de Pacientes:**
   - **Agregar:** `btnAgregarPaciente`.
   - **Modificar:** `btnModificarPaciente`.
   - **Eliminar:** `btnEliminarPaciente`.
   - **Visualizar:** Métodos para mostrar la información de los pacientes.

#### Descripción de Métodos Clave
- `btnModificarHospitalActionPerformed`: Este método modifica un hospital. La secuencia de acciones incluye la modificación de la dirección, el CP y el municipio relacionados con el hospital.
  ```java
  private void btnModificarHospitalActionPerformed(java.awt.event.ActionEvent evt) {
      try {
          Hospital h = new Hospital();
          int idDireccion = h.modificarHospital(txtNombreHospital, txtidHospital);

          Direccion d = new Direccion();
          int idCP = d.modificarDireccion(txtNExteriorHospital, txtCalleHospital, idDireccion);

          CP cp = new CP();
          int idMunicipio = cp.modificarCP(txtCpHospital, cboEstadoHospital.getSelectedItem().toString(), idCP);

          municipio m = new municipio();
          m.modificarMunicipio(txtMunicipio, idMunicipio);

          h.mostrarHospitales(tbHospitales);
      } catch (Exception e) {
          JOptionPane.showMessageDialog(null, "Error al modificar hospital (btn): " + e.toString());
      }
  }
  ```

- `btnEliminarHospitalActionPerformed`: Este método elimina un hospital de la base de datos y actualiza la tabla de hospitales.
  ```java
  private void btnEliminarHospitalActionPerformed(java.awt.event.ActionEvent evt) {
      Hospital h = new Hospital();
      h.eliminarHospital(txtidHospital);
      h.mostrarHospitales(tbHospitales);
  }
  ```

#### Manejo de Eventos
- Eventos como `MouseClicked`, `ActionPerformed` están configurados para reaccionar a las interacciones del usuario, como hacer clic en una fila de una tabla o presionar un botón.
