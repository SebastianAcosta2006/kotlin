# kotlin
// Clase Producto
class Producto(private var id: Int, private var nombre: String, private var cantidad: Int, private var precio: Double) {

    // Métodos getter y setter
    fun getId(): Int = id
    fun getNombre(): String = nombre
    fun getCantidad(): Int = cantidad
    fun getPrecio(): Double = precio

    fun setId(id: Int) {
        this.id = id
    }

    fun setNombre(nombre: String) {
        this.nombre = nombre
    }

    fun setCantidad(cantidad: Int) {
        this.cantidad = cantidad
    }

    fun setPrecio(precio: Double) {
        this.precio = precio
    }

    // Calcular el precio total (precio * cantidad)
    fun calcularPrecioTotal(): Double {
        return precio * cantidad
    }
}

// Clase Inventario
class Inventario {
    private val productos: MutableList<Producto> = mutableListOf()

    // Agregar un producto al inventario
    fun agregarProducto(producto: Producto) {
        productos.add(producto)
    }

    // Consultar todos los productos
    fun consultarProductos() {
        if (productos.isEmpty()) {
            println("El inventario está vacío.")
        } else {
            productos.forEach {
                println("ID: ${it.getId()} | Nombre: ${it.getNombre()} | Cantidad: ${it.getCantidad()} | Precio: ${it.getPrecio()}")
            }
        }
    }

    // Buscar un producto por ID
    fun buscarProductoPorId(id: Int): Producto? {
        return productos.find { it.getId() == id }
    }

    // Modificar un producto existente
    fun modificarProducto(id: Int, nombre: String?, cantidad: Int?, precio: Double?) {
        val producto = buscarProductoPorId(id)
        if (producto != null) {
            if (nombre != null) producto.setNombre(nombre)
            if (cantidad != null) producto.setCantidad(cantidad)
            if (precio != null) producto.setPrecio(precio)
            println("Producto modificado exitosamente.")
        } else {
            println("Producto no encontrado.")
        }
    }

    // Eliminar un producto
    fun eliminarProducto(id: Int) {
        val producto = buscarProductoPorId(id)
        if (producto != null) {
            productos.remove(producto)
            println("Producto eliminado exitosamente.")
        } else {
            println("Producto no encontrado.")
        }
    }

    // Calcular el IVA de una venta (porcentaje configurable, por defecto 19%)
    fun calcularIVA(total: Double, porcentaje: Double = 19.0): Double {
        return total * porcentaje / 100
    }
}

// Menú interactivo
fun main() {
    val inventario = Inventario()
    var continuar = true

    while (continuar) {
        println("\n--- Menú de Inventario ---")
        println("1. Agregar Producto")
        println("2. Consultar Productos")
        println("3. Modificar Producto")
        println("4. Eliminar Producto")
        println("5. Calcular IVA")
        println("6. Salir")
        println("Elige una opción: ")

        when (readLine()?.toInt()) {
            1 -> {
                println("Introduce el ID del producto:")
                val id = readLine()!!.toInt()
                println("Introduce el nombre del producto:")
                val nombre = readLine()!!.toString()
                println("Introduce la cantidad del producto:")
                val cantidad = readLine()!!.toInt()
                println("Introduce el precio del producto:")
                val precio = readLine()!!.toDouble()

                val producto = Producto(id, nombre, cantidad, precio)
                inventario.agregarProducto(producto)
                println("Producto agregado exitosamente.")
            }
            2 -> {
                println("Consultando productos...")
                inventario.consultarProductos()
            }
            3 -> {
                println("Introduce el ID del producto a modificar:")
                val id = readLine()!!.toInt()

                println("Introduce el nuevo nombre (deja vacío para no cambiarlo):")
                val nombre = readLine()!!.takeIf { it.isNotEmpty() }
                println("Introduce la nueva cantidad (deja vacío para no cambiarlo):")
                val cantidad = readLine()!!.takeIf { it.isNotEmpty() }?.toInt()
                println("Introduce el nuevo precio (deja vacío para no cambiarlo):")
                val precio = readLine()!!.takeIf { it.isNotEmpty() }?.toDouble()

                inventario.modificarProducto(id, nombre, cantidad, precio)
            }
            4 -> {
                println("Introduce el ID del producto a eliminar:")
                val id = readLine()!!.toInt()
                inventario.eliminarProducto(id)
            }
            5 -> {
                println("Introduce el precio total de los productos:")
                val total = readLine()!!.toDouble()
                val iva = inventario.calcularIVA(total)
                println("El IVA es: $iva")
            }
            6 -> {
                println("¡Hasta luego!")
                continuar = false
            }
            else -> println("Opción no válida, por favor elige una opción válida.")
        }
    }
}
