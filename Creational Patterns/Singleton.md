__Singleton__
===

The singleton pattern is what you use when you want to ensure that only one instance of an object is ever created. In classical object-oriented programming languages, the concepts behind creating a singleton was a bit tricky to wrap your mind around because it involved a class that has both static and non-static properties and methods.

> The Singleton design pattern ensures a class has only one instance and provide a global point of access to it.

- Defines an Instance operation that lets clients access its unique instance. Instance is a class operation.
- Responsible for creating and maintaining its own unique instance.

## Structural code in C#
This structural code demonstrates the Singleton pattern which assures only a single instance (the singleton) of the class can be created.

```cs
using System;

namespace Creational.Singleton.Structure
{
    /// <summary>
    /// Singleton Design Pattern
    /// </summary>

    public class Program
    {
        public static void Main(string[] args)
        {
            // Constructor is protected -- cannot use new

            Singleton s1 = Singleton.Instance();
            Singleton s2 = Singleton.Instance();

            // Test for same instance

            if (s1 == s2)
            {
                Console.WriteLine("Objects are the same instance");
            }

            // Wait for user

            Console.ReadKey();
        }
    }

    /// <summary>
    /// The 'Singleton' class
    /// </summary>

    public class Singleton
    {
        static Singleton instance;

        // Constructor is 'protected'

        protected Singleton()
        {
        }

        public static Singleton Instance()
        {
            // Uses lazy initialization.
            // Note: this is not thread safe.
            if (instance == null)
            {
                instance = new Singleton();
            }

            return instance;
        }
    }
}
```

## .NET Optimized code in C#
The .NET optimized code demonstrates the same code as above but uses more modern, built-in .NET features.

Here is an elegant .NET specific solution. The Singleton pattern simply uses a private constructor and a static readonly instance variable that is lazily initialized. Thread safety is guaranteed by the compiler.

```cs
using System;
using System.Collections.Generic;
using static System.Console;

namespace Creational.Singleton.NetOptimized
{
    public class Program
    {
        /// <summary>
        /// Singleton Design Pattern
        /// </summary>

        public static void Main()
        {
            var b1 = LoadBalancer.GetLoadBalancer();
            var b2 = LoadBalancer.GetLoadBalancer();
            var b3 = LoadBalancer.GetLoadBalancer();
            var b4 = LoadBalancer.GetLoadBalancer();

            // Confirm these are the same instance

            if (b1 == b2 && b2 == b3 && b3 == b4)
            {
                WriteLine("Same instance\n");
            }

            // Next, load balance 15 requests for a server

            var balancer = LoadBalancer.GetLoadBalancer();
            for (int i = 0; i < 15; i++)
            {
                string serverName = balancer.NextServer.Name;
                WriteLine("Dispatch request to: " + serverName);
            }

            // Wait for user

            ReadKey();
        }
    }

    /// <summary>
    /// The 'Singleton' class
    /// </summary>

    public sealed class LoadBalancer
    {
        // Static members are 'eagerly initialized', that is, 
        // immediately when class is loaded for the first time.
        // .NET guarantees thread safety for static initialization

        private static readonly LoadBalancer instance = new LoadBalancer();

        private readonly List<Server> servers;
        private readonly Random random = new Random();

        // Note: constructor is 'private'

        private LoadBalancer()
        {
            // Load list of available servers

            servers = new List<Server>
                {
                  new Server{ Name = "ServerI", IP = "120.14.220.18" },
                  new Server{ Name = "ServerII", IP = "120.14.220.19" },
                  new Server{ Name = "ServerIII", IP = "120.14.220.20" },
                  new Server{ Name = "ServerIV", IP = "120.14.220.21" },
                  new Server{ Name = "ServerV", IP = "120.14.220.22" },
                };
        }

        public static LoadBalancer GetLoadBalancer()
        {
            return instance;
        }

        // Simple, but effective load balancer

        public Server NextServer
        {
            get
            {
                int r = random.Next(servers.Count);
                return servers[r];
            }
        }
    }

    /// <summary>
    /// Represents a server machine
    /// </summary>

    public class Server
    {
        public string Name { get; set; }
        public string IP { get; set; }
    }
}
```

