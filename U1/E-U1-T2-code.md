# Ejercicio Práctico: Gestión de Cuentas Bancarias en C#

## Objetivos
- Configurar un proyecto de Console App en .NET usando VS Code.
- Crear clases, objetos y aplicar conceptos básicos de Programación Orientada a Objetos (POO) en C#.
- Implementar métodos para depositar, retirar y consultar saldo.
- Explorar la herencia y el polimorfismo mediante la creación de una clase derivada.

## Instrucciones

### 1. Crear el Proyecto
1. Abre **Visual Studio Code**.
2. Abre la Paleta de Comandos con `Ctrl+Shift+P` (o `Cmd+Shift+P` en macOS).
3. Escribe y selecciona **.NET: New Project**.
4. Selecciona **Console App** como tipo de proyecto.
5. Nombra el proyecto como `GestionCuentas`.
6. Elige una ubicación para guardar el proyecto.

### 2. Reemplazar el Código Base
Abre el archivo `Program.cs` que se creó y reemplaza su contenido por el siguiente código:

```csharp
using GestionCuentas;
// Crear una cuenta bancaria básica
CuentaBancaria cuenta1 = new CuentaBancaria("001", "Juan Pérez", 1000m);
cuenta1.Depositar(500m);
cuenta1.Retirar(300m);
cuenta1.MostrarInformacion();

// Crear una cuenta bancaria premium
CuentaBancaria cuentaPremium = new CuentaBancariaPremium("002", "María López", 2000m);
cuentaPremium.Depositar(800m);
cuentaPremium.Retirar(2500m);
cuentaPremium.MostrarInformacion();

// Reto Adicional:
// Crea al menos otra instancia de CuentaBancaria o CuentaBancariaPremium,
// realiza algunas operaciones y muestra su información.
CuentaBancaria cuenta2 = new CuentaBancaria("003", "Carlos Martínez", 1500m);
cuenta2.Depositar(200m);
cuenta2.Retirar(1000m);
cuenta2.MostrarInformacion();
    
```
Encontrarás diferentes alertas en el código que acabas de pegar, para solucionarlas debes crear dos clases: `CuentaBancaria` y `CuentaBancariaPremium` que heredará de `CuentaBancaria`. Ambos archivos deben tener la extensión `.cs`, debes tener el siguiente código:

```csharp
using System;

namespace GestionCuentas
{
    // Clase base: CuentaBancaria
    public class CuentaBancaria
    {
        // Propiedades públicas de solo lectura
        public string NumeroCuenta { get; private set; }
        public string Titular { get; private set; }

        // Campo privado para almacenar el saldo
        private decimal saldo;

        // Constructor: inicializa los valores de la cuenta
        public CuentaBancaria(string numeroCuenta, string titular, decimal saldoInicial)
        {
            NumeroCuenta = numeroCuenta;
            Titular = titular;
            saldo = saldoInicial;
        }

        // Método para depositar dinero
        public void Depositar(decimal cantidad)
        {
            if (cantidad > 0)
            {
                saldo += cantidad;
                Console.WriteLine($"Se depositaron {cantidad:C}. Saldo actual: {saldo:C}");
            }
            else
            {
                Console.WriteLine("Cantidad inválida para depositar.");
            }
        }

        // Método para retirar dinero
        public virtual bool Retirar(decimal cantidad)
        {
            if (cantidad > 0 && saldo >= cantidad)
            {
                saldo -= cantidad;
                Console.WriteLine($"Se retiraron {cantidad:C}. Saldo actual: {saldo:C}");
                return true;
            }
            else
            {
                Console.WriteLine("No se pudo retirar la cantidad solicitada.");
                return false;
            }
        }

        // Método para consultar el saldo actual
        public decimal ConsultarSaldo()
        {
            return saldo;
        }

        // Reto: Agrega aquí un método para mostrar la información completa de la cuenta
        public void MostrarInformacion()
        {
            Console.WriteLine("----- Información de la Cuenta -----");
            Console.WriteLine($"Número de Cuenta: {NumeroCuenta}");
            Console.WriteLine($"Titular: {Titular}");
            Console.WriteLine($"Saldo: {saldo:C}");
            Console.WriteLine("------------------------------------\n");
        }
    }

}
```

Y para `CuentaBancariaPremium`:

```csharp
using System;

namespace GestionCuentas
{
    // Clase derivada: CuentaBancariaPremium (hereda de CuentaBancaria)
    // Permite retirar más dinero (simula un sobregiro del 10%)
    public class CuentaBancariaPremium : CuentaBancaria
    {
        public CuentaBancariaPremium(string numeroCuenta, string titular, decimal saldoInicial)
            : base(numeroCuenta, titular, saldoInicial)
        {
        }

        // Sobrescribe el método Retirar para permitir un sobregiro del 10%
        public override bool Retirar(decimal cantidad)
        {
            decimal saldoActual = ConsultarSaldo();
            // Se permite retirar hasta el 110% del saldo actual
            if (cantidad > 0 && saldoActual * 1.1m >= cantidad)
            {
                Console.WriteLine($"Retiro de {cantidad:C} autorizado en cuenta premium.");
                // Nota: Para efectos del ejercicio, se simula el retiro.
                // En una implementación real, se actualizaría el saldo permitiendo valores negativos.
                return true;
            }
            else
            {
                Console.WriteLine("No se pudo retirar la cantidad solicitada en cuenta premium.");
                return false;
            }
        }
    }
}

```

### 3. Ejecutar y Evaluar el Código

- Guarda los archivos.

- Abre la terminal integrada en VS Code.

- Ejecuta el proyecto usando el comando:

        dotnet run

- Observa la salida en la consola para verificar que las operaciones (depósito, retiro y consulta de saldo) se ejecutan correctamente en cada tipo de cuenta.

### Reto Adicional

* Validación Adicional: Modifica el método Retirar en la clase CuentaBancaria para que no se permita retirar una cantidad negativa.

* Nuevos Métodos: Agrega otros métodos que consideres útiles para gestionar las cuentas, como un método para transferir dinero entre cuentas.

* Pruebas: Crea más instancias y realiza diferentes operaciones para comprobar que el comportamiento de cada clase es el esperado.
