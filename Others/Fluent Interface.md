**Fluent interface in C#**
===

A fluent interface is normally implemented by using method cascading (concretely method chaining) to relay the instruction context of a subsequent call (but a fluent interface entails more than just method chaining [1]). Generally, the context is
1. defined through the return value of a called method
2. self-referential, where the new context is equivalent to the last context
3. terminated through the return of a void context.

So here it code how you can implement Jquery like fluent interface(method chaning) with C# code.
```cs
class FluentInterface
{
	private StringBuilder _sbl;

	public FluentInterface()
	{
		_sbl = new StringBuilder();
	}

	public FluentInterface Add(string variable)
	{
		_sbl.AppendLine(variable);
		return this;
	}

	public FluentInterface Replace(string oldValue,string newValue)
	{
		_sbl.Replace(oldValue, newValue);
		return this;
	}

	public FluentInterface Print()
	{
		string[] DataArray = new List<string>(Regex.Split(_sbl.ToString(), Environment.NewLine)).ToArray();
		DataArray.ToList().ForEach(x=&gt;Console.WriteLine(x));
		return this;
	}

	public FluentInterface Clear()
	{
		_sbl.Clear();
		return this;
	}
}
```

Here i have chained the method using a class called "FluentInterface", which implements the StringBuilder and provided a set of method, which are chained together. This implements JQuery like accessability in c#. The function can be called as:

```cs
new FluentInterface().Add("line1").Add("line2").Replace("line2","line456").
Add("line3").Add("line4").Add("line5").Print().Clear();
```
