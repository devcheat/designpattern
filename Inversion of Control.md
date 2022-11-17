**Inversion of Control (IOC)**
===

High-level modules should not depend on low-level modules. Both should depend on abstractions.
Abstractions should not depend upon details. Details should depend upon abstractions.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Plug_in1
{
    /*
     *  Dependency inversion principle:
     *  1.High-level modules should not depend on low-level modules. Both should depend on abstractions.
     *  2.Abstractions should not depend upon details. Details should depend upon abstractions.
     */

    // Interface implementing Ioc DI
    public interface IInversion
    {
        void Invert(string message);
    }

    // class
    class Ioc
    {
        //
        private IInversion inv = null;

        // Property Injection
        public IInversion Action
        {
            get { return inv; }
            set { inv = value; }
        }
        //Default 
        public Ioc()
        {
            /*Default const*/
        }


        // Constructor Injection 
        public Ioc(IInversion inv01)
        {
            this.inv = inv01;
        }

        // Method Injection
        public void Change(IInversion concreteInversion, string message)
        {
            this.inv = concreteInversion;
            if (inv != null)
                inv.Invert(message);

        }

        //method
        public void Change(string message)
        {
            if (inv != null)
                inv.Invert(message);
        }
    }

    // First inverted function
    class InvertedFunc1 : IInversion
    {
        public void Invert(string message)
        {
            Console.WriteLine("message: {0}", message);
        }
    }
```

# The Main
```cs
class main
    {
        public void Run(string[] arg)
        {
            Ioc i = new Ioc(new InvertedFunc1());
            i.Change("Adidas");

        }
    }
```
