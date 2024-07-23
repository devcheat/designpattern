# SOLID Principals


*SOLID* is a set of five principals intended to make software design more flexible and maintainable. 
It helps building software with more cohesive code and less coupling. Each letter of the acronym corresponds to a prinicple.

## S = Single Responsibility Principal
> A Class should have only a single responsibility, only one reason to change. (High-Cohesion, Low-Coupling)

```cs
using System.Diagnositics;

public class InfoClass
{
	public void GetInfo()
    {
        EventLog.WriteEntry("message title","get some info from DB");
	}
	
    public void SaveInfo(InfoType info)
    {
        EventLog.WriteEntry("message title","save info to DB");
    }
}

public static class LogClass
{
	public static void Log(string title,string message)
	{
		EventLog.WriteEntry(title,message);
	}
}
```

## O = Open/Closed Principal
> Entities should be open for extension, but close for modification.


## L = Liskov Subsitution Principal
> Objects should be replaceable with instance of their subtypes without altering the correctness of the program, otherwise you may be using the abstraction.


## I = Interface Segregation Principal 
> Avoid one general-purpose interface by creating more specific interfaces.


## D = Dependency Inversion Principal
> Depend upon abstractions, not concretions (reduce coupling)





