# Unity Service Locator

A lightweight and simple **Service Locator** pattern implementation for Unity.  
Designed for small to mid-scale projects to decouple service dependencies cleanly.

## ✨ Features

- Centralized service registration and retrieval
- Type-safe access to services
- Lightweight and MonoBehaviour-based
- Automatically initializes early with `DefaultExecutionOrder`

## 📦 How to Use

### 1. Add the `ServiceLocator` to a GameObject

Create an empty GameObject in your first-loaded scene and attach the `ServiceLocator` script:

```csharp
[DefaultExecutionOrder(-10)]
public class ServiceLocator : MonoBehaviour
{
    public static ServiceLocator Instance;
    private readonly Dictionary<Type, object> _services = new();

    private void Awake()
    {
        Instance = this;
    }
    
    public void RegisterService<T>(T service)
    {
        var type = typeof(T);
        if (!_services.TryAdd(type, service))
        {
            Debug.LogWarning($"Service of type {type} is already registered.");
        }
    }

    public T GetService<T>()
    {
        var type = typeof(T);
        if (_services.TryGetValue(type, out var service))
        {
            return (T)service;
        }

        Debug.LogError($"Service {type} didn't registered.");
        return default;
    }
}
