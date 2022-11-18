**Business Delegate**
===

Business Delegate Pattern is used to decouple presentation tier and business tier. It is basically use to reduce communication or remote lookup functionality to business tier code in presentation tier code.
In business tier we've following entities.

## Client - Presentation tier code may be JSP, Asp, servlet or UI code.
```cs

namespace Business_Delegate
{
    class Client
    {
        BusinessDelegate businessService;

        public Client(BusinessDelegate businessService)
        {
            this.businessService = businessService;
        }

        public void doTask()
        {
            businessService.doTask();
        }
    }
}
```
## Business Delegate - A single entry point class for client entities to provide access to Business Service methods.

```cs
namespace Business_Delegate
{
    public class BusinessDelegate
    {
        private BusinessLookUp lookupService = new BusinessLookUp();
        private BusinessService businessService;
        private String serviceType;

        public void setServiceType(String serviceType)
        {
            this.serviceType = serviceType;
        }

        public void doTask()
        {
            businessService = lookupService.getBusinessService(serviceType);
            businessService.doProcessing();
        }
    }
}
```

## LookUp Service - Lookup service object is responsible to get relative business implementation and provide business object access to business delegate object.
```cs

namespace Business_Delegate
{
    class BusinessLookUp
    {
        public BusinessService getBusinessService(String serviceType)
        {
            if (serviceType.Equals("EJB"))
            {
                return new Services.Service1();
            }
            else
            {
                return new Services.Service2();
            }
        }
    }
}
```

## Business Service - Business Service interface. Concrete classes implements this business service to provide actual business implementation logic.
```cs

namespace Business_Delegate
{
    interface BusinessService
    {
        void doProcessing();
    }
}
```
and services as Service1

```cs

namespace Business_Delegate.Services
{
    class Service1:BusinessService
    {
        public void doProcessing()
        {
            Console.WriteLine("Service 1 invoked");
        }
    }
}
```
and Service2
```cs
namespace Business_Delegate.Services
{
    class Service2:BusinessService
    {
        public void doProcessing()
        {
            Console.WriteLine("Service 2 invoked");
        }
    }
}
```

To use these we need a main methods as
## Main Method
```cs

namespace Business_Delegate
{
    class BusinessDelegatePatternDemo
    {
        public static void main(String[] args)
        {

            BusinessDelegate businessDelegate = new BusinessDelegate();
            businessDelegate.setServiceType("WCF");

            Client client = new Client(businessDelegate);
            client.doTask();

            businessDelegate.setServiceType("EJB");
            client.doTask();
        }
    }
}
```
