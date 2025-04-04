# Taller 2

## Preguntas Teoricas

### 1. **¿Qué son los principios SOLID y cómo contribuyen a un buen diseño orientado a objetos?**
Los principios SOLID son un conjunto de cinco directrices que ayudan a diseñar software orientado a objetos de manera más clara, flexible y fácil de mantener:

S - Principio de Responsabilidad Única (SRP): Cada clase debe encargarse de una sola tarea o tener un único motivo para cambiar. Esto hace que el código sea más comprensible y sencillo de modificar.

O - Principio de Abierto/Cerrado (OCP): Las clases deben poder extenderse sin necesidad de alterar su código original. Esto se logra mediante herencia o interfaces, permitiendo añadir nuevas funcionalidades sin romper lo existente.

L - Principio de Sustitución de Liskov (LSP): Las subclases deben poder usarse como si fueran la clase base sin afectar el correcto funcionamiento del programa. Esto garantiza coherencia y compatibilidad en la jerarquía de clases.

I - Principio de Segregación de Interfaces (ISP): Es preferible contar con varias interfaces pequeñas y específicas, en lugar de una sola con muchas responsabilidades. Así, los componentes solo dependen de lo que realmente usan.

D - Principio de Inversión de Dependencias (DIP): Las clases de nivel alto no deben depender directamente de las de nivel bajo, sino de abstracciones (interfaces). Esto favorece un diseño más modular y desacoplado.

En conjunto, estos principios fomentan un diseño de software más organizado y sólido, facilitando la lectura del código, su mantenimiento, reutilización y ampliación sin que se generen fallos o dependencias innecesarias.

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

### **1. Logger (Singleton)**

```csharp
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
public interface IObserver
{
    void OnNotify(string message);
}

```

- Cualquier clase que implemente esta interfaz debe tener un método `OnNotify`.

### **Clase `Subject`**:

```csharp
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
## logger singleton unity

```
using UnityEngine;
using System.IO;
using System;

public class LoggerSingleton : MonoBehaviour
{
    private static LoggerSingleton _instance;
    private string _logFilePath;

    void Awake()
    {
        // Configuración del Singleton
        if (_instance != null && _instance != this)
        {
            Destroy(gameObject);
            return;
        }

        _instance = this;
        DontDestroyOnLoad(gameObject);

        // Inicializar sistema de logs
        _logFilePath = Path.Combine(Application.persistentDataPath, "game_log.txt");
        Log("Logger inicializado. Ruta: " + _logFilePath);
    }

    public static void Log(string message)
    {
        if (_instance == null) Initialize();

        string formattedMessage = $"{DateTime.Now:HH:mm:ss} - {message}";
        
        // Mostrar en consola de Unity
        Debug.Log(formattedMessage);
        
        // Guardar en archivo
        try
        {
            File.AppendAllText(_instance._logFilePath, formattedMessage + Environment.NewLine);
        }
        catch (Exception e)
        {
            Debug.LogError($"Error al escribir en log: {e.Message}");
        }
    }

    private static void Initialize()
    {
        if (_instance != null) return;
        
        GameObject loggerObj = new GameObject("LoggerSystem");
        _instance = loggerObj.AddComponent<LoggerSingleton>();
    }
}

```

```
using UnityEngine;

public class PruebaLogger : MonoBehaviour
{
    void Start()
    {
       
        LoggerSingleton.Log("¡El juego ha comenzado!");
        
      
        int vidas = 3;
        LoggerSingleton.Log($"Jugador iniciado con {vidas} vidas");
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            LoggerSingleton.Log("Tecla SPACE presionada");
        }
    }
}
```

![image](https://github.com/user-attachments/assets/ebc39a46-cf3e-457c-b12d-38fbf2bb28ed)

## Observer

### Subject
```
using UnityEngine;
using System;

public class Subject : MonoBehaviour
{
    // Evento que notifica cuando el estado cambia
    public event Action<string> OnStateChanged;

    private string _currentState;

    // Propiedad para modificar el estado
    public string CurrentState
    {
        get => _currentState;
        set
        {
            if (_currentState != value)
            {
                _currentState = value;
                OnStateChanged?.Invoke(_currentState); // Notifica a los observadores
                Debug.Log($"Subject: Estado cambiado a '{_currentState}'");
            }
        }
    }

    // Métodos para probar desde el Inspector
    [ContextMenu("Cambiar a Estado 1")]
    private void SetState1() => CurrentState = "Estado 1";

    [ContextMenu("Cambiar a Estado 2")]
    private void SetState2() => CurrentState = "Estado 2";
}
```

### Observer
```
using UnityEngine;

public class Observer : MonoBehaviour
{
    [Tooltip("Nombre identificador del observador")]
    public string observerName = "Observador";

    [Tooltip("Arrastra el GameObject que tiene el componente Subject")]
    public Subject subjectToObserve;

    private void Start()
    {
        if (subjectToObserve != null)
        {
            // Suscribirse al evento
            subjectToObserve.OnStateChanged += HandleStateChange;
            Debug.Log($"{observerName} listo para recibir notificaciones.");
        }
        else
        {
            Debug.LogError($"{observerName} no tiene un Subject asignado!", gameObject);
        }
    }

    private void OnDestroy()
    {
        // Desuscribirse al destruirse 
        if (subjectToObserve != null)
            subjectToObserve.OnStateChanged -= HandleStateChange;
    }

    // Método que se ejecuta cuando el Subject notifica un cambio
    private void HandleStateChange(string newState)
    {
        Debug.Log($"{observerName}: ¡El estado ahora es '{newState}'!");
    }
}
```
### ObserverTester
```
using UnityEngine;

public class ObserverTester : MonoBehaviour
{
    [SerializeField] private Subject subject;
    [SerializeField] private float delayBetweenChanges = 2f;

    private void Start()
    {
        if (subject == null)
            subject = FindObjectOfType<Subject>();

        InvokeRepeating(nameof(ChangeState), 1f, delayBetweenChanges);
    }

    private void ChangeState()
    {
        string randomState = "Estado " + Random.Range(1, 4);
        subject.CurrentState = randomState;
    }
}
```
![image](https://github.com/user-attachments/assets/65209fb9-ac33-49d5-95c8-e16a8eba927e)
![image](https://github.com/user-attachments/assets/2fcb1dd8-5083-431d-8982-ecb45e20366a)
![image](https://github.com/user-attachments/assets/0fcaf2f5-4058-4304-8b66-19cf4d926d88)

## Observer + Decorador

### Player.cs
```


using System;
using UnityEngine;

public class Player : MonoBehaviour
{
    public delegate void PowerChangedHandler(float newPower);
    public event PowerChangedHandler PowerChanged;

    private float power = 50f;

    public float Power
    {
        get { return power; }
        set
        {
            if (power != value)
            {
                power = value;
                OnPowerChanged(power);
            }
        }
    }

    protected virtual void OnPowerChanged(float newPower)
    {
        PowerChanged?.Invoke(newPower);
    }
}
```
### PowerObserver.cs
```

using UnityEngine;

public class PowerObserver : MonoBehaviour
{
    private Player player;

    private void OnEnable()
    {
        player = FindObjectOfType<Player>();
        if (player != null)
        {
            player.PowerChanged += OnPowerChanged;
        }
    }

    private void OnDisable()
    {
        if (player != null)
        {
            player.PowerChanged -= OnPowerChanged;
        }
    }

    private void OnPowerChanged(float newPower)
    {
        Debug.Log($"El poder del jugador ha cambiado a: {newPower}");
    }
}
```
### IPlayer.cs
```

public interface IPlayer
{
    float GetPower();
}
```
### PlayerBase.cs
```

using UnityEngine;

public class PlayerBase : MonoBehaviour, IPlayer
{
    private float power = 50f;

    public float GetPower()
    {
        return power;
    }

    public void SetPower(float newPower)
    {
        power = newPower;
    }
}
```
### PlayerDecorator.cs
```

public abstract class PlayerDecorator : IPlayer
{
    protected IPlayer player;

    public PlayerDecorator(IPlayer player)
    {
        this.player = player;
    }

    public virtual float GetPower()
    {
        return player.GetPower();
    }
}
```
### PowerUpDecorator.cs
```

public class PowerUpDecorator : PlayerDecorator
{
    private float powerBonus;

    public PowerUpDecorator(IPlayer player, float powerBonus) : base(player)
    {
        this.powerBonus = powerBonus;
    }

    public override float GetPower()
    {
        return player.GetPower() + powerBonus;
    }
}
```
### GameController.cs
```

using UnityEngine;

public class GameController : MonoBehaviour
{
    void Start()
    {
        // --- OBSERVER ---
        Player player = new GameObject("Player").AddComponent<Player>();
        PowerObserver observer = new GameObject("PowerObserver").AddComponent<PowerObserver>();
        player.Power = 75f;  // Cambia el poder para disparar el evento

        // --- DECORATOR ---
        PlayerBase basePlayer = new GameObject("BasePlayer").AddComponent<PlayerBase>();
        PowerUpDecorator powerUp = new PowerUpDecorator(basePlayer, 25f);

        Debug.Log($"Poder original del jugador: {basePlayer.GetPower()}");
        Debug.Log($"Poder del jugador con power-up: {powerUp.GetPower()}");

        // Cambiar el poder para ver cómo reacciona el Observer
        player.Power = 100f;
    }
}
```
![image](https://github.com/user-attachments/assets/06c9b392-5b89-42c4-b1d4-3e8f32d8b6b6)


## Strategy

### Interfaz IAttackStrategy.cs

```
public interface IAttackStrategy 
{
    void Attack();
}
```

### NormalAttack.cs

```
public class NormalAttack : IAttackStrategy 
{
    public void Attack() 
    {
        Debug.Log("Ataque normal: 10 de daño");
    }
}
```

### StrongAttack.cs

```
public class StrongAttack : IAttackStrategy 
{
    public void Attack() 
    {
        Debug.Log("Ataque FUERTE: 25 de daño (gasta 20% de energía)");
    }
}

```

### SpecialAttack.cs

```
public class SpecialAttack : IAttackStrategy 
{
    public void Attack() 
    {
        Debug.Log("Ataque ESPECIAL: 50 de daño (gasta 1 poción)");
    }
}

```


### Player.cs

```

using UnityEngine;

public class Player : MonoBehaviour
{
    private IAttackStrategy _currentStrategy;

    // Cambia la estrategia dinámicamente
    public void SetStrategy(IAttackStrategy strategy) 
    {
        _currentStrategy = strategy;
    }

    // Ejecuta el ataque según la estrategia actual
    public void PerformAttack() 
    {
        if (_currentStrategy != null)
            _currentStrategy.Attack();
        else
            Debug.LogError("¡No hay estrategia de ataque asignada!");
    }
}

```

### StrategyTester.cs

```
using UnityEngine;

public class StrategyTester : MonoBehaviour
{
    private Player _player;

    private void Start() 
    {
        _player = FindObjectOfType<Player>();
        Debug.Log("Prueba de Estrategias: Usa las teclas 1 (Normal), 2 (Fuerte), 3 (Especial)");
    }

    private void Update() 
    {
        if (Input.GetKeyDown(KeyCode.1))
        {
            _player.SetStrategy(new NormalAttack());
            _player.PerformAttack();
        }
        else if (Input.GetKeyDown(KeyCode.2))
        {
            _player.SetStrategy(new StrongAttack());
            _player.PerformAttack();
        }
        else if (Input.GetKeyDown(KeyCode.3))
        {
            _player.SetStrategy(new SpecialAttack());
            _player.PerformAttack();
        }
    }
}
```




## Diapositivas:
- https://www.canva.com/design/DAGjn48JMiA/a7oWOR4UbE6piVyIGOry_Q/edit?utm_content=DAGjn48JMiA&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton
