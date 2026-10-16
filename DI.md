Создаем интерфейс
```C#
public interface ITest
{
    void Test(string str);
}
```

Создаем класс и реализуем интерфейс
```C#
public class Test : ITest
{
    ILog _logger = LogManager.GetLogger<Test>();

    void ITest.Test(string str)
    {
        _logger.Info(str);
    }
}
```

На старте приложения биндим класс к интерфейсу
```C#
public class TmkAppEventLister : IAppEventListener
{
    public void OnAppEnd(AppEventContext context){}

    public void OnAppStart(AppEventContext context)
    {
        ClassFactory.Bind<ITest, Test>();
    }

    public void OnSessionEnd(AppEventContext context){}

    public void OnSessionStart(AppEventContext context){}
}
```

Получаем экземпляр класс по интерфейсу в любой точке кода
```C#
[OperationContract]
[WebInvoke(Method = "POST", BodyStyle = WebMessageBodyStyle.Wrapped, RequestFormat = WebMessageFormat.Json, ResponseFormat = WebMessageFormat.Json)]
public void Test()
{
    ClassFactory.Get<ITest>().Test("Hello");
}
```