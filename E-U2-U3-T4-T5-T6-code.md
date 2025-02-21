# **Ejercicio Práctico: Construyendo una Web API Segura y Probada en .NET 8/9**

## **Introducción**
En este ejercicio desarrollarás una API RESTful en **ASP.NET Core** aplicando arquitectura en capas, bases de datos con **Entity Framework Core**, autenticación con **Identity Framework y JWT**, y pruebas unitarias e integración con **xUnit y NSubstitute**.

Cada parte del ejercicio se basa en los temas aprendidos y se construye sobre la anterior.

---

## **Parte 1: Creación de la Web API y Arquitectura Backend**
### **Objetivos:**
- Crear una **Web API** en **.NET 8/9**.
- Aplicar **arquitectura en capas** (Controlador, Servicio y Repositorio).
- Definir controladores y rutas RESTful.

### **Instrucciones:**
1. **Crear el proyecto API**:
    ```sh
    dotnet new webapi -n MiProyectoAPI
    cd MiProyectoAPI
    ```
2. **Definir la entidad principal** (ejemplo: `Producto`):
    ```csharp
    public class Producto {
        public int Id { get; set; }
        public string Nombre { get; set; }
        public decimal Precio { get; set; }
    }
    ```
3. **Crear la capa de servicio** (`IProductoService` y `ProductoService`).
4. **Crear la capa de repositorio** (`IProductoRepository` y `ProductoRepository`).
5. **Configurar el controlador** (`ProductosController`).

**Validar con Postman o Swagger que las rutas funcionan correctamente.**

---

## **Parte 2: Base de Datos con EF Core y LINQ**
### **Objetivos:**
- Integrar **Entity Framework Core**.
- Crear la base de datos y aplicar **migraciones**.
- Implementar consultas **LINQ** para filtrar y modificar datos.
- Configurar **PostgreSQL y SQL Server**.

### **Instrucciones:**
1. **Instalar Entity Framework Core**:
    ```sh
    dotnet add package Microsoft.EntityFrameworkCore.SqlServer
    dotnet add package Microsoft.EntityFrameworkCore.Design
    ```
2. **Configurar el `DbContext` y la conexión a la base de datos en `appsettings.json`**.
3. **Aplicar la primera migración y actualizar la base de datos**:
    ```sh
    dotnet ef migrations add InitialCreate
    dotnet ef database update
    ```
4. **Implementar consultas básicas con LINQ** en la capa de repositorio:
    ```csharp
    public async Task<List<Producto>> ObtenerProductosCaros() {
        return await _context.Productos.Where(p => p.Precio > 100).ToListAsync();
    }
    ```

**Verificar que la API interactúa correctamente con la base de datos.**

---

## **Parte 3: Seguridad con Identity Framework y JWT**
### **Objetivos:**
- Implementar **Identity Framework** para autenticación y autorización.
- Configurar **JWT** para el manejo de sesiones.
- Restringir acceso a ciertos endpoints.

### **Instrucciones:**
1. **Instalar Identity y JWT**:
    ```sh
    dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
    dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
    ```
2. **Configurar Identity en `Program.cs`**.
3. **Configurar JWT en `Program.cs`**:
    ```csharp
    builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options => {
            options.TokenValidationParameters = new TokenValidationParameters {
                ValidateIssuer = true,
                ValidateAudience = true,
                ValidateLifetime = true,
                ValidateIssuerSigningKey = true,
                ValidIssuer = builder.Configuration["Jwt:Issuer"],
                ValidAudience = builder.Configuration["Jwt:Audience"],
                IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Key"]))
            };
        });
    ```
4. **Proteger un controlador con [Authorize]**:
    ```csharp
    [Authorize]
    [HttpGet("protegido")]
    public IActionResult EndpointProtegido() {
        return Ok("Acceso permitido");
    }
    ```

**Verificar que los usuarios autenticados pueden acceder y los no autenticados reciben error.**

---

## **Parte 4: Pruebas Unitarias e Integración**
### **Objetivos:**
- Implementar **pruebas unitarias** con xUnit.
- Usar **NSubstitute** para pruebas con dependencias.
- Crear **pruebas de integración** para la API.

### **Instrucciones:**
1. **Agregar xUnit y NSubstitute**:
    ```sh
    dotnet add package xunit
    dotnet add package NSubstitute
    ```
2. **Crear prueba unitaria para `ProductoService`**:
    ```csharp
    public class ProductoServiceTests {
        private readonly IProductoRepository _repo;
        private readonly ProductoService _service;

        public ProductoServiceTests() {
            _repo = Substitute.For<IProductoRepository>();
            _service = new ProductoService(_repo);
        }

        [Fact]
        public async Task ObtenerProductosCaros_DeberiaRetornarProductosCaros() {
            _repo.ObtenerProductos().Returns(new List<Producto> {
                new Producto { Id = 1, Nombre = "Barato", Precio = 50 },
                new Producto { Id = 2, Nombre = "Caro", Precio = 200 }
            });

            var resultado = await _service.ObtenerProductosCaros();
            Assert.Single(resultado);
            Assert.Equal("Caro", resultado[0].Nombre);
        }
    }
    ```
3. **Ejecutar las pruebas**:
    ```sh
    dotnet test
    ```

---

## **Conclusión**
Este ejercicio te permitió aplicar los conceptos fundamentales de ASP.NET Core, bases de datos con Entity Framework Core, seguridad con Identity y JWT, y pruebas unitarias con xUnit y NSubstitute.

Al completar cada parte, fortaleciste tu comprensión de cómo desarrollar APIs seguras, modulares y bien probadas en **.NET 8/9**. 🚀

