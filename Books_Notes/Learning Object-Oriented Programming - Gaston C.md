# Learning Object-Oriented Programming -  Gaston C. Hillar

Created: Jul 30, 2020 11:44 PM
Reviewed: No

# Notas del libro - Programacion orientada a objetos en C#

**Data encapsulation**: Pilar de OOP. La data requerida se encapsulara en cada objeto.

**Clase**: En OOP es un blueprint/template a partir del cual se creara un objeto. Define el estado y comportamiento de ese objeto (al cual tambien se lo conoce como instancia = cada objeto es una instancia de su clase)

**Atributos**: variables creadas dentro de una clase.

**Metodos**: funciones creadas dentro de una clase.

**Abstraccion**: en una clase abstracta no se pueden crear instancias de la misma

**Instancias**: son creadas a partir de un metodo constructor

**Destructor**: destruyen instancias de clases que no son utilizadas mediante el mecanismo de la garbage collection

```csharp
class Circle
{
	private double Radius;
	public Circle(doble radius) // ==> Funcion constructora. Lleva el nombre de la clase
		{
			Console.WriteLine("Nueva instancia de Circle");
			this.Radius = radius;
			//this.Radius permite acceder a la instancia creada
		}
}

class Graph01
{
	public static void Main(string [] args)
		{
			var circle01 = new Circle(25); //==> Instancias de Circle
			var circle02 = new Circle(20);
		}
}

~Circle() // ==> desctructor. Lleva el ~ delante del nombre (mismo nombre de la clase)
{
	//code
}
```

A veces restricciones son necesarias = el acceso o modificaciones de atributos.

Las propiedades pueden definir get o set metodos:

- setters: permiten controlar como los valores seran seteados
- getters: permiten controlar como los valores son devueltos(no cambian valores de atributos)

**Mutabilidad/Inmutabilidad**: todas las instancias de una clase, los valores de las variables/propiedades son suceptibles de ser mutables, cambiar su estado. Lo cual puede traer problemas. Se soluciona creando getters para esa propiedad. Entonces si cambio alguna propiedad se hace creando una nueva instancia y no modificando la original (genera overhead performance, pero a veces es necesario)

```csharp
class FoxTerrier
{
	public static string Family = "Terrier"; // ==> static significa que es class field
	public static int Energy = 10;
	
	public string Name;
	public int FavoriteToy;
	public FoxTerrier(string name, int FavoriteToy)
		{
			this.Name = name;
			this.FavoriteToy = favoriteToy;
		}
}
```

**instance field** = variable that is bound to the object itself. Si se quiere usar fuera del objeto, debe usarse a traves de metodos getter/setter.

**class field/static field** = you dont need to have an instance of the containing object to use it.

**Modificadores de acceso**: controlan que codigo tiene acceso a un tipo de miembro especifico:

- public
- protected: solo el codigo en la clase o en subclases derivadas pueden acceder
- private: solo el codigo en la clase puede acceder, no asi sus clases derivadas o externas
- internal: solo el codigo dentro del mismo asembly puede acceder
- protected internal: solo el codigo en el mismo asembly y clases derivadas pueden acceder

```csharp
private protected string _name; // ==> usar underscore para las clases private protected

// Ejemplo anterior
protected string _name;
protected int _favoriteToy;

this._name = name;
this._favoriteToy = favoriteToy;
```

**propiedades read-only**: define metodos getter que devuelven un valor

```csharp
public string Name
{
	get
		{
			return _name;
		}
} 
// Con esto no puedo crear una instancia de la clase y asignarle un nuevo .Name

public string Name {get; set;} // ==> auto-implemented prop

public string FavToy
{
	get
		{
			return this._favToy;
		}
	set
		{
			this._favToy = value; // ==> el valor que recibira y seteara
			// puede incluir condicionales if(valor > 0){}
		}
}

var tom = new FoxTerrier ("Tom", 8, "Boomerang");
tom.method = 9;
```

**Polimorfismo**: cuando usamos un metodo que con el mismo nombre y argumentos, causa distintas cosas de acuerdo en la clase en que invocado (ejemplo method talk())

```csharp
public abstract class Animal
{
	protected virtual int NumberOfLegs {get{return= 0}};//=> virtual para override el valor
	protected virtual int PairsOfEyes {get{return= 0}};
	public int Age {get; set;}
	public Animal(int age)
		{
			this.Age = age;
			Console.WriteLine("Animal created");
		}
	public void PrintLegsAndEyes()
		{
			Console.WriteLine($"I have {this.numberOfLegs} legs");
		}
}

public abstract class Mammal : Animal
{
	protected override int PairsOfEyes{get{return = 1}};//se utiliza override junto virtual
	public bool IsPregnant {get; set;};
	private void Init(bool isPregnant)
		{
			this.IsPregnant = isPregnant;
			Console.WriteLine("Mammal is created");
		}
	public Mammal (int age : base(age)//base pérmite llamar al constructor de la superclass
		{
			this.Init(false)
		}
	public Mammal(int age, bool isPregnant) :base(age) // constructor overload
		{
			this.Init(isPregnant);
		}
}
```

**Interfaces**: una clase abstacta especial. Se lo suele llamar contrato. Define metodos y propiedades que una clase debe implementar para ser considerada miembro del grupo con el mismo nombre de interfaz.

Las interfaces no pueden declarar:

- constructores
- destructores
- constantes
- fields
- especificar modificadores de acceso

```csharp
public interface IComicCharacter
{
	string NickName {get; set;};
	void DrawSpeechBallong(string message); // solo la declaracion de los metodos, porque
//todas las clases que implementen la interfaz IComicCharacter realizaran su propia
//implementacion
	}

public class AngryDog : IComicCharacter
{
	public string NickName {get; set;};
	public AngryDog (string nickName)
	{
		this.NickName = nickName;
	}
	protected void Speack(string message)
	{
		Console.WriteLine($"{this.nickName}, {this.message}")
	}

```

En C# no se puede implementar multiple base clases o superclases (no soporta multiple herencias). Una clase solo puede heredar una clase. Pero si puede heredar varias interfaces.

Si una clase hereda otra clase que tiene una interfaz, tambien esta clase hereda esa interfaz.

**Generics**: Concepto. Flexibilidad/facilidad de hacer funcionalidad con el mismo metodo/clase sobre distintos tipos de datos. Se utiliza en las colecciones

```csharp
List <int> num = new List <int>();

//Flexibilidad para elegir el tipo de dato y reutilizacion de codigo. Un mismo metodo
//se comportara distinto de acuerdo al tipo de dato que reciba.
//Tambien te ayuda con la validacion de tipo de dato
//Buen rendimiento

public class A<T> // puede ser cualquier nombre. Se suele utilizar T por type
{
	public void Algo(T un_parametro)
		{
			//
		}
}

A<int> oA = new A<int>();
oA.Algo(4);
A<string> oA2 = new A<strin>();
oA2.Algo("4");

public class Party<T> where T:IAnimal //==> T tiene que ser un tipo que implemente la 
																			// interfax IAnimal

```