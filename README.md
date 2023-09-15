import java.util.Scanner;
import java.util.HashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Map<String, Usuario> usuarios = new HashMap<>();

        while (true) {
            System.out.println("Menú:");
            System.out.println("1. Agregar Usuario");
            System.out.println("2. Mostrar Usuarios");
            System.out.println("3. Realizar Acción");
            System.out.println("4. Salir");
            System.out.print("Seleccione una opción: ");

            int opcion = scanner.nextInt();

            switch (opcion) {
                case 1:
                    agregarUsuario(usuarios);
                    break;
                case 2:
                    mostrarUsuarios(usuarios);
                    break;
                case 3:
                    realizarAccion(usuarios);
                    break;
                case 4:
                    System.out.println("Saliendo del programa...");
                    scanner.close();
                    System.exit(0);
                default:
                    System.out.println("Opción no válida. Intente de nuevo.");
            }
        }
    }

    public static void agregarUsuario(Map<String, Usuario> usuarios) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Ingrese el nombre completo del usuario: ");
        String nombreCompleto = scanner.nextLine();
        System.out.print("Ingrese el correo electrónico del usuario: ");
        String correoElectronico = scanner.nextLine();
        System.out.print("Ingrese el código de perfil asignado (1 para Administrador, 2 para Cajero, 3 para Cliente): ");
        int codigoPerfil = scanner.nextInt();
        System.out.print("Ingrese la clave del usuario: ");
        String clave = scanner.next();

        Usuario usuario = new Usuario(nombreCompleto, correoElectronico, codigoPerfil, clave);
        usuarios.put(usuario.getCorreoElectronico(), usuario);

        System.out.println("Usuario agregado con éxito.");
    }

    public static void mostrarUsuarios(Map<String, Usuario> usuarios) {
        System.out.println("Lista de Usuarios:");
        for (Usuario usuario : usuarios.values()) {
            usuario.mostrarContenido();
            System.out.println("---------------------------");
        }
    }

    public static void realizarAccion(Map<String, Usuario> usuarios) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Ingrese el correo electrónico del usuario que realizará la acción: ");
        String correoElectronico = scanner.nextLine();

        Usuario usuario = usuarios.get(correoElectronico);

        if (usuario == null) {
            System.out.println("Usuario no encontrado.");
            return;
        }

        System.out.print("Ingrese la clave del usuario: ");
        String clave = scanner.next();

        if (usuario.verificarClave(clave)) {
            usuario.mostrarAcciones();
            System.out.print("Seleccione una acción: ");
            int opcionAccion = scanner.nextInt();
            usuario.realizarAccion(opcionAccion);
        } else {
            System.out.println("Clave incorrecta. Acceso denegado.");
        }
    }
}

class Usuario {
    private String nombreCompleto;
    private String correoElectronico;
    private int codigoPerfil;
    private String clave;

    public Usuario(String nombreCompleto, String correoElectronico, int codigoPerfil, String clave) {
        this.nombreCompleto = nombreCompleto;
        this.correoElectronico = correoElectronico;
        this.codigoPerfil = codigoPerfil;
        this.clave = clave;
    }

    public String getNombreCompleto() {
        return nombreCompleto;
    }

    public String getCorreoElectronico() {
        return correoElectronico;
    }

    public int getCodigoPerfil() {
        return codigoPerfil;
    }

    public void mostrarContenido() {
        System.out.println("Nombre: " + nombreCompleto);
        System.out.println("Correo Electrónico: " + correoElectronico);
        System.out.println("Código de Perfil: " + codigoPerfil);
    }

    public void mostrarAcciones() {
        System.out.println("Acciones disponibles:");
        if (codigoPerfil == 1) {
            System.out.println("1. Asignar código a perfil (Administrador)");
        } else if (codigoPerfil == 2) {
            System.out.println("1. Registrar venta (Cajero)");
        } else if (codigoPerfil == 3) {
            System.out.println("1. Consultar productos (Cliente)");
        }
    }

    public boolean verificarClave(String clave) {
        return this.clave.equals(clave);
    }

    public void realizarAccion(int opcionAccion) {
        if (codigoPerfil == 1 && opcionAccion == 1) {
            System.out.println("Acción: Asignar código a perfil (Administrador)");
        } else if (codigoPerfil == 2 && opcionAccion == 1) {
            System.out.println("Acción: Registrar venta (Cajero)");
        } else if (codigoPerfil == 3 && opcionAccion == 1) {
            System.out.println("Acción: Consultar productos (Cliente)");
        } else {
            System.out.println("Acción no válida para este perfil.");
        }
    }
}
