# Taller-2

## Preguntas Teoricas

### 1. **¿Qué son los principios SOLID y cómo contribuyen a un buen diseño orientado a objetos?**
Los principios SOLID son un conjunto de cinco principios de diseño que buscan mejorar la calidad, flexibilidad y mantenibilidad del software orientado a objetos:

- **S**: *Single Responsibility Principle (SRP)* - Cada clase debe tener una única responsabilidad o razón para cambiar. Esto facilita la comprensión y mantenimiento de cada clase.
  
- **O**: *Open/Closed Principle (OCP)* - Las clases deben estar abiertas a la extensión pero cerradas a la modificación. Esto se logra mediante la creación de clases base o interfaces que permitan agregar nuevas funcionalidades sin modificar el código existente.
  
- **L**: *Liskov Substitution Principle (LSP)* - Los objetos de una subclase deben ser sustituibles por objetos de la clase base sin alterar el comportamiento del programa. Esto asegura que las clases derivadas respeten el contrato de la clase base.
  
- **I**: *Interface Segregation Principle (ISP)* - Es mejor tener varias interfaces específicas que una sola interfaz general. Esto permite que los clientes dependan solo de los métodos que realmente utilizan.
  
- **D**: *Dependency Inversion Principle (DIP)* - Las clases de alto nivel no deben depender de clases de bajo nivel, sino de abstracciones. Esto reduce la dependencia entre módulos y facilita la creación de sistemas más modulares.

   Los principios SOLID contribuyen a un buen diseño porque promueven un código modular, flexible y mantenible, es decir, un código más limpio, fácil de mantener y con menos acoplamiento; lo que reduce la posibilidad de errores al modificar o extender el sistema. Ademas, facilitan la implementación de nuevas características sin introducir errores y mejoran la reutilización y legibilidad del código.

### 2. **Explica cómo el patrón Singleton asegura que solo haya una instancia de una clase y cuáles son sus posibles usos.**
El **patrón Singleton** restringe la creación de objetos de una clase a una única instancia, asegurando que todas las llamadas a la clase retornen la misma instancia. Esto se logra mediante:
     - Un constructor privado para evitar la creación directa de objetos.
     - Un método estático que controla el acceso a la única instancia, creando la instancia solo la primera vez que se solicita.
   
Al ser único, el patrón Singleton es útil en situaciones donde es necesario un control centralizado, como:
  - **Gestión de recursos compartidos**: Como sistemas de registro (logging) donde solo se necesita un único logger a lo largo de la aplicación, aplicaciones con una conexión a base de datos, o un archivo de configuración que debe ser accesible desde diferentes partes de la aplicación.
   - **Control de acceso global**: Cuando se necesita un punto de acceso común para una clase en todo el sistema.

### 3. **¿Cómo funciona el patrón Observer y en qué situaciones es útil?**
El patrón *Observer* es un patrón de diseño que permite que un objeto (el "sujeto") notifique a otros objetos (los "observadores") sobre cambios en su estado sin que el sujeto necesite saber quiénes son esos observadores. Funciona de la siguiente manera:

1. El sujeto mantiene una lista de observadores interesados en los cambios.
2. Cuando ocurre un cambio en el estado del sujeto, este notifica a todos sus observadores.
3. Cada observador responde de acuerdo a la notificación recibida.

Este patrón es útil en situaciones donde varios objetos necesitan actualizarse en respuesta a cambios en otro objeto, como en:

- *Interfaces gráficas* donde los elementos de la UI necesitan actualizarse cuando el estado de un modelo cambia o  en interfaces donde se necesita una actualización automática de la interfaz en respuesta a cambios en los datos subyacentes (por ejemplo, un gráfico que se actualiza cuando cambian los valores de los datos).
- *Sistemas de eventos* o suscripciones, como en aplicaciones que requieren notificaciones en tiempo real.

### 4. **¿Qué es un antipatrón? explique por medio de dos ejemplos.**
Un *antipatrón* es una solución de diseño o implementación que parece adecuada pero en realidad tiene consecuencias negativas, como crear código difícil de mantener, ineficiente o propenso a errores. Ejemplos:

- **Big Ball of Mud o Spaghetti Code**: Se refiere a un sistema donde no hay una estructura clara y todo el código está fuertemente acoplado. Los módulos o clases no siguen principios de diseño, lo que resulta en un código desorganizado, difícil de mantener y escalar.
  
  *Ejemplo*: Un proyecto de software grande que, con el tiempo, se ha convertido en una mezcla de código con dependencias circulares, sin separación de responsabilidades y sin modularidad. Modificar una parte del sistema puede afectar otras partes inesperadamente.

- **God Object**: Ocurre cuando una clase central asume demasiadas responsabilidades, violando el principio de responsabilidad única. Estas clases tienden a ser difíciles de mantener y de probar, ya que manejan demasiadas tareas en lugar de delegarlas a otras clases.

  *Ejemplo*: Un objeto en una aplicación que gestiona la interfaz de usuario, el acceso a la base de datos, la lógica de negocio y la configuración de la aplicación, todo en una sola clase, lo que hace que cualquier cambio o error sea difícil de rastrear y corregir.
   
## Patrones

### ¿Qué beneficios existen al usar patrones?

Utilizar patrones a la hora de crear codigo resulta beneficio en distintos aspectos, uno de los principales siendo la posibilidad de utilizarlos para resolver problemas comunes que se pueden presentar a la hora de implementar soluciones a dichos problemas, ya que proveen una estructura documentada y funcional. Ademas, muchos patrones tienen como proposito facilitar la escalabilidad de los programas sin necesidad de entrar a refactorizar segmentos gigantes que sean preexistentes en el programa evitando asi que surgan errores por incompatibilidades o situaciones en las cuales sea necesario re-hacer segmentos enteros en una aplicacion de considerable tamano.

### Implementacion Singleton Logger
Para la implementacion del patron Singleton, se creo una aplicacion de consola en C# que permite guardar una serie de mensajes predefinidos (Logs) en un archivo de texto (log.txt) haciendo uso del patron singleton. Los aspectos importantes al momento de hablar de singletons es que estos tienen como tarea absoluta evitar que se creen instancias nuevas de una clase al momento de trabajar en un programa. La clase se crea, mantiene un constructor privado dentro de la misma, y solo permite acceder a los miembros datos necesarios por medio de una instancia, evitando que otras clases creen sus propias instancias.

C#

```
using System;
using System.IO;


class Program
{
    static void Main(string[] args)
    {
        // Obtenemos la instancia del Logger
        Logger logger = Logger.Instance;

        // Escribimos nuestros mensajes
        logger.Log("Este es un mensaje de testeo.");
        logger.Log("Esta es la continuacion del mensaje.");
        logger.Log("Gatos.");

        // Mostramos donde se guardo.
        Console.WriteLine("Mensajes guardados en : " + logger.GetLogFilePath());

    }
}

public sealed class Logger
{
    private static Logger _instance = null;
    private string _filePath;

    // Constructor privado dentro de la misma clase para prevenir instanciamiento adicional.
    private Logger()
    {
        _filePath = "log.txt";

        // Creamos nuevo archivo si no existiese antes.
        if (!File.Exists(_filePath))
        {
            File.Create(_filePath).Dispose();
        }
    }

    
    // Metodo publico para obtener la instancia del Logger.
    public static Logger Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new Logger();
            }
            return _instance;
        }
    }

    
    // Metodo para copiar los mensajes al archivo.
    public void Log(string message)
    {
        using (StreamWriter writer = new StreamWriter(_filePath, true))
        {
            string logEntry = $"{DateTime.Now}: {message}";
            writer.WriteLine(logEntry);
        }
    }

    public string GetLogFilePath()
    {
        return _filePath;
    }
}
```


### Implementacion Decorator Coffee Shop
Para la implementacion del patron Decorator, se realizo una simulacion de una plataforma de pedidos de cafe. La esencia del patron Decorator es que permite expandir la funcionalidad de un objeto sin la necesidad de modificar otros objetos que dependan de el. La manera mas sencilla de pensarlo es literalmente en el nombre "Decorador"; Se tiene una clase base que engendra un objeto base, pero a ese objeto se le puede agregar muchas mas funcionalidades o caracteristicas sin modificar la clase base que lo ha creado, literalmente "decorandolo".

C#

```
class Program
{
    static void Main(string[] args)
    {
        // Cafe normal
        ICoffee myCoffee = new SimpleCoffee();
        Console.WriteLine($"{myCoffee.GetDescription()} - ${myCoffee.GetCost()}");

        // Agregamos leche
        myCoffee = new MilkDecorator(myCoffee);
        Console.WriteLine($"{myCoffee.GetDescription()} - ${myCoffee.GetCost()}");

        // Agregamos azucar
        myCoffee = new SugarDecorator(myCoffee);
        Console.WriteLine($"{myCoffee.GetDescription()} - ${myCoffee.GetCost()}");

        // Agregamos crema
        myCoffee = new WhippedCreamDecorator(myCoffee);
        Console.WriteLine($"{myCoffee.GetDescription()} - ${myCoffee.GetCost()}");
    }
}



public interface ICoffee
{
    string GetDescription();
    float GetCost();
}
public class SimpleCoffee : ICoffee
{
    public string GetDescription()
    {
        return "Cafecito normal";
    }

    public float GetCost()
    {
        return 5.00f; // precio de un cafe normal
    }
}
public abstract class CoffeeDecorator : ICoffee
{
    protected ICoffee _coffee;

    public CoffeeDecorator(ICoffee coffee)
    {
        _coffee = coffee;
    }

    public virtual string GetDescription()
    {
        return _coffee.GetDescription();
    }

    public virtual float GetCost()
    {
        return _coffee.GetCost();
    }
}
public class MilkDecorator : CoffeeDecorator
{
    public MilkDecorator(ICoffee coffee) : base(coffee) { }

    public override string GetDescription()
    {
        return _coffee.GetDescription() + ", con leche";
    }

    public override float GetCost()
    {
        return _coffee.GetCost() + 1.50f; // El precio extra de la leche
    }
}
public class SugarDecorator : CoffeeDecorator
{
    public SugarDecorator(ICoffee coffee) : base(coffee) { }

    public override string GetDescription()
    {
        return _coffee.GetDescription() + ", con azucar";
    }

    public override float GetCost()
    {
        return _coffee.GetCost() + 0.50f; // El precio extra de la azucar
    }
}
public class WhippedCreamDecorator : CoffeeDecorator
{
    public WhippedCreamDecorator(ICoffee coffee) : base(coffee) { }

    public override string GetDescription()
    {
        return _coffee.GetDescription() + ", con crema";
    }

    public override float GetCost()
    {
        return _coffee.GetCost() + 2.00f; // El precio extra de la crema
    }
}
```

### Implementacion Observer Store Inventory
Para la implementacion del patron Observer, se realizo una simulacion de una tienda virtual que vende productos (en este caso una computadora) los cuales entran y salen de disponibilidad en cualquier momento, notificando a los clientes interesados de los cambios de disponibilidad. Este es uno de los patrones que puede resultar un tanto dificil de entender, y la manera en como se propuso el ejercicio ayuda un poco mejor a entender el flujo de como funciona el patron. En terminos generales, existe un sujeto el cual es el objeto siendo observado. No necesita saber cuantos ni quienes lo estan observando, solo existe y notifica cambios en su comportamiento, de la misma manera, existen observadores, los cuales estan al tanto del sujeto esperando un cambio y de acuerdo al tipo de cambio experimentado, reaccionan de una manera determinada. En nuestro ejemplo, la tienda tiene un producto, los clientes quieren saber si este producto se encuentra disponible, la tienda les manda actualizaciones de su disponibilidad si asi lo desean los clientes.

C#
```
using System;
using System.Collections.Generic;

// Observer Interface - Customer
public interface ICustomer
{
    void Update(string message);
    bool WasNotified { get; set; }
    string GetName();
}

// Subject Interface - Product
public interface IProduct
{
    void Attach(ICustomer customer);
    void Detach(ICustomer customer);
    void Notify(string message);
}

// Concrete Product (Subject)
public class Product : IProduct
{
    private List<ICustomer> _customers = new List<ICustomer>();
    private bool _inStock;

    public bool InStock
    {
        get { return _inStock; }
        set
        {
            _inStock = value;
            if (_inStock)
            {
                Notify($"El producto ha vuelto a la tienda!");
            }
        }
    }

    private string _productName;

    public Product(string productName, bool inStock)
    {
        _productName = productName;
        _inStock = inStock;
    }

    public void Attach(ICustomer customer)
    {
        if (!_customers.Contains(customer))
        {
            _customers.Add(customer);
            Console.WriteLine($"{customer.GetName()} se ha suscrito a las noticias de {_productName}.");
        }
    }

    public void Detach(ICustomer customer)
    {
        if (_customers.Contains(customer))
        {
            _customers.Remove(customer);
            Console.WriteLine($"{customer.GetName()} se ha desuscrito a las noticias de {_productName}.");
        }
    }

    // Notify all customers and set their "WasNotified" property
    public void Notify(string message)
    {
        foreach (var customer in _customers)
        {
            customer.Update(message);
        }
    }

    public string GetProductName()
    {
        return _productName;
    }

    // Manually send a notification and show notification status
    public void SendNotificationToAllCustomers(string customMessage)
    {
        Notify(customMessage);
        Console.WriteLine("Estado de las notificaciones de los clientes:");
        foreach (var customer in _customers)
        {
            Console.WriteLine($"{customer.GetName()}: {(customer.WasNotified ? "Notificado" : "Not Notificado")}");
        }
    }
}

// Concrete Customer (Observer)
public class Customer : ICustomer
{
    private string _name;
    public bool WasNotified { get; set; }

    public Customer(string name)
    {
        _name = name;
        WasNotified = false;
    }

    public void Update(string message)
    {
        WasNotified = true;
        Console.WriteLine($"Notificacion para {_name}: {message}");
    }

    public string GetName()
    {
        return _name;
    }
}

// Main Program with a Menu
class Program
{
    static void Main(string[] args)
    {
        Product laptop = new Product("PC Gamer", false);
        List<Customer> customerList = new List<Customer>();

        bool exit = false;

        while (!exit)
        {
            Console.Clear();
            Console.WriteLine("=== Tiendita Virtual ===");
            Console.WriteLine("1. Agregar cliente");
            Console.WriteLine("2. Quitar cliente");
            Console.WriteLine("3. Poner producto como disponible");
            Console.WriteLine("4. Quitar producto de disponibilidad");
            Console.WriteLine("5. Ver estado de clientes");
            Console.WriteLine("6. Enviar notificacion a los clientes");
            Console.WriteLine("7. Salir");
            Console.Write("Elige una opcion: ");

            switch (Console.ReadLine())
            {
                case "1":
                    Console.Write("Ingresa nombre de cliente: ");
                    string customerName = Console.ReadLine();
                    Customer newCustomer = new Customer(customerName);
                    customerList.Add(newCustomer);
                    laptop.Attach(newCustomer);
                    Console.WriteLine($"Cliente '{customerName}' agregado.");
                    break;

                case "2":
                    Console.Write("Ingresa el nombre del cliente a quitar: ");
                    customerName = Console.ReadLine();
                    Customer customerToRemove = customerList.Find(c => c.GetName() == customerName);
                    if (customerToRemove != null)
                    {
                        laptop.Detach(customerToRemove);
                        customerList.Remove(customerToRemove);
                        Console.WriteLine($"Cliente '{customerName}' ha sido removido.");
                    }
                    else
                    {
                        Console.WriteLine($"Cliente '{customerName}' no encontrado.");
                    }
                    break;

                case "3":
                    laptop.InStock = true;
                    Console.WriteLine($"{laptop.GetProductName()} ha vuelto!");
                    break;

                case "4":
                    laptop.InStock = false;
                    Console.WriteLine($"{laptop.GetProductName()} ya no esta disponible!");
                    break;

                case "5":
                    Console.WriteLine("Clientes suscritos:");
                    if (customerList.Count == 0)
                    {
                        Console.WriteLine("No hay clientes suscritos.");
                    }
                    else
                    {
                        foreach (var customer in customerList)
                        {
                            Console.WriteLine($"- {customer.GetName()}");
                        }
                    }
                    break;

                case "6":
                    Console.Write("Ingresa la notificacion a enviar: ");
                    string customMessage = Console.ReadLine();
                    laptop.SendNotificationToAllCustomers(customMessage);
                    break;

                case "7":
                    exit = true;
                    break;

                default:
                    Console.WriteLine("Opcion invalida.");
                    break;
            }

            Console.WriteLine("\nPresiona cualquier tecla para volver al menu...");
            Console.ReadKey();

            // Reset notification status for all customers for the next round
            foreach (var customer in customerList)
            {
                customer.WasNotified = false;
            }
        }

        Console.WriteLine("Chao!");
    }
}
```
### Implementacion Adapter Payment APIs
Para la implementacion del patron Adapter, se utilizo la simulacion de una plataforma de pagos online que acepta un metodo de pago por default (Paypal) pero tambien necesita aceptar pagos por otro medio de pago (Stripe, en este ejemplo). En nuestro ejemplo, nuestra plataforma de pago acepta Paypal como metodo de pago por defecto, sin embargo, supongamos que Stripe nos ofrece beneficios si lo incluimos como parte de nuestra pasarela de pagos. Esto presenta un inconveniente, ya que la implementacion de Stripe es distinta a la implementacion ya existente de la de Paypal que ya tenemos, por lo debemos "adaptar" la funcionalidad de Stripe a la que utiliza Paypal, sin necesidad de re-escribir nuestra pasarela de pago para incluir de manera estatica a Stripe u otros medios de pago que puedan ser implementados despues. La esencia del patron Adapter es que permite que distintas interfaces (que poseen sus propios metodos) trabajen en conjunto sin necesidad de re-escribir todo el codigo para acomodarlas de manera estatica. En nuestro ejemplo, si nuestra pasarela de pagos espera recibir ordenes de la interfaz IPaymentProcessor (la cual Paypal implementa por defecto) podemos hacer que otros medios de pago utilicen esta interfaz compartida, en vez de re-estructurar o crear multiples interfaces para acomodar cada tipo de pago.

C#
```
namespace Adapterpayments
{
    internal class Program
    {
        static void Main(string[] args)
        {
            // Usando Paypal
            IPaymentProcessor paypal = new PayPal();
            paypal.ProcessPayment(100.00);

            // Usando Stripe adaptado
            StripePayment stripe = new StripePayment();
            IPaymentProcessor stripeAdapter = new StripeAdapter(stripe);
            stripeAdapter.ProcessPayment(150.00);
        }
    }

    public interface IPaymentProcessor
    {
        void ProcessPayment(double amount);
    }
    public class PayPal : IPaymentProcessor
    {
        public void ProcessPayment(double amount)
        {
            Console.WriteLine($"Procesando pago de Paypal de ${amount}");
        }
    }

    // Stripe tiene su propio metodo de hacer pagos.
    public class StripePayment
    {
        public void MakeStripePayment(double dollars)
        {
            Console.WriteLine($"Procesando pago de Stripe de ${dollars}");
        }
    }
    public class StripeAdapter : IPaymentProcessor
    {
        private readonly StripePayment _stripePayment;

        public StripeAdapter(StripePayment stripePayment)
        {
            _stripePayment = stripePayment;
        }

        // Adaptando Stripe a la interfaz existente.
        public void ProcessPayment(double amount)
        {
            _stripePayment.MakeStripePayment(amount);
        }
    }

}
```
# Programa Unity

### **1. Logger (Patrón Singleton)**

```csharp
csharp
Copiar código
public class Logger
{
    private static Logger instance = null;
    private static readonly object padlock = new object();

    private Logger() { }

    public static Logger Instance
    {
        get
        {
            lock (padlock)
            {
                if (instance == null)
                {
                    instance = new Logger();
                }
                return instance;
            }
        }
    }

    public void Log(string message)
    {
        Debug.Log("Log: " + message);
    }
}

```

- Garantiza que solo haya una instancia de `Logger` en todo el sistema.
- **¿Dónde está el patrón Singleton?**
    - **`instance`**: Es una variable estática que contiene la única instancia de `Logger`.
    - **`Instance`**: Un método que devuelve la única instancia, asegurándose de crearla solo si no existe ya.
    - **`lock(padlock)`**: Asegura que la creación de la instancia sea segura en entornos multihilo.

### **Relación con SOLID:**

- **S (Responsabilidad Única)**: `Logger` solo se encarga de una cosa, registrar mensajes.
- **O (Abierto/Cerrado)**: Si necesitamos extender la funcionalidad del logger, podemos hacerlo sin cambiar su estructura básica.

---

### **2. Subject y Observer (Patrón Observer)**

El patrón **Observer**, que permite notificar a varios objetos cuando el estado de un sujeto cambia.

### **Interfaz `IObserver`**:

```csharp
csharp
Copiar código
public interface IObserver
{
    void OnNotify(string message);
}

```

- Cualquier clase que implemente esta interfaz debe tener un método `OnNotify`.

### **Clase `Subject`**:

```csharp
csharp
Copiar código
public class Subject
{
    private List<IObserver> observers = new List<IObserver>();

    public void Attach(IObserver observer)
    {
        observers.Add(observer);
    }

    public void Detach(IObserver observer)
    {
        observers.Remove(observer);
    }

    public void Notify(string message)
    {
        foreach (IObserver observer in observers)
        {
            observer.OnNotify(message);
        }
    }
}

```

- El `Subject` es el que notifica a los observadores. Aquí tenemos una lista de observadores que se adjuntan y se eliminan cuando sea necesario.
- El método `Notify` envía el mensaje a todos los observadores registrados.

### **Clase `ConcreteObserver`**:

```csharp
csharp
Copiar código
public class ConcreteObserver : MonoBehaviour, IObserver
{
    public void OnNotify(string message)
    {
        Debug.Log("Observer received message: " + message);
    }
}

```

- Al implementar `IObserver`, asegura que el observador tenga el método `OnNotify` para recibir notificaciones del `Subject`.

### **Relación con el patrón Observer:**

- **Subject**: Mantiene una lista de observadores y los notifica cuando ocurre un cambio.
- **Observer**: Las clases que implementan `IObserver` reciben las notificaciones y responden a los cambios.

---

### **3. Integración de Singleton y Observer en Unity**

### **Clase `GameManager` (Ejemplo de uso del Logger y Observer en Unity)**:

```csharp
csharp
Copiar código
public class GameManager : MonoBehaviour
{
    private Subject subject;
    private Logger logger;

    private void Start()
    {
        // Inicializar el Logger (Singleton)
        logger = Logger.Instance;
        logger.Log("Game started");

        subject = new Subject();
        ConcreteObserver observer1 = new GameObject("Observer1").AddComponent<ConcreteObserver>();
        ConcreteObserver observer2 = new GameObject("Observer2").AddComponent<ConcreteObserver>();

        // Se le ponen los observadores al sujeto
        subject.Attach(observer1);
        subject.Attach(observer2);
    }

    private void Update()
    {
        // Si se presiona la barra espaciadora, el Subject notifica a los Observadores
        if (Input.GetKeyDown(KeyCode.Space))
        {
            subject.Notify("Space key pressed");
        }
    }

    private void OnApplicationQuit()
    {
        logger.Log("Game finished");
    }
}

```

### **Explicación paso a paso:**

1. **Inicializamos el Singleton `Logger`**:
    - Usamos `Logger.Instance` para asegurarnos de que solo haya una instancia de `Logger` en el sistema.
    - Registramos un mensaje cuando el juego comienza y cuando termina.
2. **Inicializamos el `Subject` y los `Observers`**:
    - Creamos un nuevo `Subject`.
    - Creamos dos observadores (`ConcreteObserver`) y los adjuntamos al `Subject` utilizando el método `Attach`.
3. **Notificaciones del `Subject`**:
    - En el método `Update`, si el jugador presiona la barra espaciadora, el `Subject` notifica a todos los observadores registrados.
    - Los observadores (instancias de `ConcreteObserver`) reciben la notificación e imprimen un mensaje en la consola.

### **Relación con SOLID:**

- **S (Responsabilidad Única)**: Cada clase tiene una responsabilidad clara:
    - `GameManager` maneja la lógica del juego.
    - `Logger` se encarga de los logs.
    - `Subject` se encarga de notificar a los observadores.
    - `ConcreteObserver` maneja las notificaciones que recibe.
- **O (Abierto/Cerrado)**: Si queremos agregar más observadores, solo tenemos que crear nuevas clases que implementen `IObserver`, sin modificar `Subject`.
- **L (Sustitución de Liskov)**: Cualquier clase que implemente `IObserver` puede reemplazar a `ConcreteObserver` sin que el código cambie.
- **I (Segregación de Interfaces)**: `IObserver` solo tiene un método `OnNotify`, lo que mantiene la interfaz simple y enfocada.
- **D (Inversión de Dependencias)**: `Subject` depende de la abstracción `IObserver`, no de implementaciones concretas.

---

## Fotos en Unity

![image](https://github.com/user-attachments/assets/3e93e1db-fa19-4c10-8b2d-059c92263254)

![image](https://github.com/user-attachments/assets/893acdd3-0856-4f07-ae94-8474fb6bb56f)


Diaps: https://www.canva.com/design/DAGRIfryQVY/3wV_4fGc8w1mJwjeQxCH9g/edit?utm_content=DAGRIfryQVY&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

  
## Instrucciones 
(Trate de organizar todo lo que el profesor puso en la pagina)

### Implementacion
1. **Aplicar los principios S.O.L.I.D.**
   - Diseñar el código utilizando los cinco principios SOLID (Responsabilidad única, Abierto/cerrado, Sustitución de Liskov, Segregación de interfaces, Inversión de dependencias).

2. **Patrón Singleton**
   - Implementar el patrón Singleton en una clase para asegurar que solo haya una instancia de esta en el sistema. Ejemplo: `Logger` para registrar mensajes en un archivo.

3. **Patrón Observer**
   - Implementar el patrón Observer para permitir que varios objetos reciban notificaciones cuando un objeto cambie de estado. Ejemplo: una clase `Subject` que notifique a los `Observers` sobre cambios.

4. **Delegados**
   - Usar delegados para crear funcionalidad personalizada. Relaciona esto con la implementación del patrón Observer.

5. **Aplicación en Unity y Consola**
   - Desarrollar dos versiones: una en consola y otra en Unity que demuestren el uso de los patrones y principios mencionados.

### Preguntas Teóricas

1. **Principios SOLID**: Explica qué son y cómo ayudan a mejorar el diseño orientado a objetos.
2. **Patrón Singleton**: Describe cómo este patrón asegura una sola instancia y sus usos comunes.
3. **Patrón Observer**: Explica cómo funciona y cuándo es útil.
4. **Antipatrones**: Definir qué es un antipatrón y proporcionar dos ejemplos con explicaciones.

### Ejercicios Prácticos

1. **Clase `Logger` usando Singleton**: Crear una clase que registre mensajes en un archivo de texto.
2. **Patrón Observer**: Implementar una clase `Subject` que notifique a sus observadores cuando cambie de estado.
3. **Delegados y Observer**: Usar delegados para implementar la funcionalidad del patrón Observer.
4. **Patrón Decorador en C#**: Consultar librería nativa de C# que use el patrón decorador e integrarla.

### Implementación de Patrones de Diseño

1. **Elegir 2 patrones**: Implementar otros dos patrones (elegir entre Decorador, Facade, Strategy, Adapter).
2. **Comparación de Implementaciones**: Explicar las diferencias entre la implementación de estos patrones en consola y en Unity, y los beneficios de usar patrones.

### Notas

1. **Commits Regulares**: Mínimo 10 commits por estudiante. Todos deben subir cambios al repositorio.
2. **Presentacion**: Que tenga evidencias del trabajo realizado (código, capturas de pantalla, diagramas).
