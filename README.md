**Laboratorio 7.1 : Spring API RESTful**

En este laboratorio se creará un servicio web RESTful con Spring Boot, usando Spring MVC, Spring JPA (Hibernate) y MySQL. Vamos a construir una API básica para gestionar "Productos"
**1.Crear el Proyecto**
Puedes usar Spring Initializr con estas dependencias:
- Spring Web
- Spring Data JPA 
- Driver MySql
- Lombok(Opcional)

Crea los paquetes necesarios en el proyecto, el proyecto debe tener la siguiente estructura:



**2.Configuración en application.properties**
Configuramos la conexión con la base de datos para JPA

```
spring.application.name=servicioweb
spring.datasource.url=jdbc:mysql://localhost:3306/negocios2026?useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=mysql
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

```
*Importante!	Para este ejemplo la base de datos será negocios2025, la que debemos tener esta base de datos, iremos a MySql y ejecutar el comando:  ´create database negocios2025;´ *

3.Entidades Producto
Creamos el paquete model y las entidades dentro de ella:
Entidad Producto
package com.ejemplo.servicioweb.model;


import jakarta.persistence.*;
import lombok.Data;
@Data
@Entity
@Table(name = "productos")
public class Producto {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    @Column(nullable = false)
    private String nombre;
    @Column(nullable = false)
    private double precio;
}


Importante!	@Entity: Indica que es una tabla en la BD. Y @Data (Lombok): Genera getters, setters, toString(), etc. automáticamente.

4.Repositorios
Creamos el paquete repository y dentro de lel las clases repositorios:
package com.ejemplo.servicioweb.repository;

import com.ejemplo.servicioweb.model.Producto;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ProductoRepository extends JpaRepository<Producto, Long> {
    
}

   Reordenemos! 	Spring Data JPA provee métodos CRUD automáticamente Ejemplo: save(), findAll(), findById(), deleteById(), etc.

5.Lógica del negocio (Service)
package com.ejemplo.servicioweb.service;

import com.ejemplo.servicioweb.model.Producto;
import com.ejemplo.servicioweb.repository.ProductoRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class ProductoService {
    @Autowired
    private ProductoRepository productoRepository;

    public List<Producto> listarTodos() {
        return productoRepository.findAll();
    }

    public Producto guardar(Producto producto) {
        return productoRepository.save(producto);
    }

    public Producto obtenerPorId(Long id) {
        return productoRepository.findById(id).orElse(null);
    }

    public void eliminar(Long id) {
        productoRepository.deleteById(id);
    }
}

6.Controladores
package com.ejemplo.servicioweb.controller;


import com.ejemplo.servicioweb.model.Producto;
import com.ejemplo.servicioweb.service.ProductoService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/productos")
public class ProductoController {
    @Autowired
    private ProductoService productoService;

    @GetMapping
    public List<Producto> listarProductos() {
        return productoService.listarTodos();
    }

    @PostMapping
    public Producto crearProducto(@RequestBody Producto producto) {
        return productoService.guardar(producto);
    }

    @GetMapping("/{id}")
    public Producto obtenerProducto(@PathVariable Long id) {
        return productoService.obtenerPorId(id);
    }

    @DeleteMapping("/{id}")
    public void eliminarProducto(@PathVariable Long id) {
        productoService.eliminar(id);
    }
}

   Importante! 	@RestController: Combina @Controller + @ResponseBody (respuestas JSON). @RequestMapping: Define la ruta base del API.

7.Ejecución y resultados
Para este caso todo los resultados serán servicios web, por lo que no se requiere de las vistas en el proyecto.
a.Preparando la base de datos. Debemos crear la base de datos negocions2025 e insertar algunos registros para las pruebas:
CREATE DATABASE negocios2026;

-- Ejecuta el programa para que se cree la tabla Productos o en caso contrario crea 
-- la tabla con crete table…

USE negocios2026;
INSERT INTO productos VALUES
(1,'Laptop HP i7 RAM 16Gb, HD 1Tb',3600.00),
(2,'Laptop HP i5 RAM 8Gb, HD 520Gb',1600.00);
8.Prueba Endpoints disponibles:
a)GET /api/productos > Listar todos.
b)GET /api/productos/{id} > Obtener por ID.
c)POST /api/productos > Crear producto (envía JSON en el body).
d)DELETE /api/productos/{id} > Eliminar por ID..
a) GET /api/productos

b) GET /api/productos/{id}


c) POST /api/productos 
Para creara productos debemos enviar por POST un JSON con los datos de producto, para ello utilizaremos POSTMAN . Deben registrarse o con su correo ingresar a una sesión. También puede descargar la app.
 Creamos uno nuevo
Para ingresar los datos de envío ubicarse en Body y raw; los datos a envias debes estar den JSON como se muestra la figura y dar Send.

Mostramos si se inserto:

e) DELETE /api/productos/{id}
Para la eliminación de un registro el Id se envía por la url pero debe ser un verbo del tipo DELETE, por lo tanto también usaremos POSTMAN.

