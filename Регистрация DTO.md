```C#
[Serializable]
[DataContract]
public class IntegrationParameters
{
    public Uri Url
    {
        get
        {
            string baseUrl = $"{ServerAddress.TrimEnd('/')}/{DatabaseName}/{AreaNumber}/hs/{ServiceName}/{MethodName}";
            if (Uri.TryCreate(baseUrl, UriKind.Absolute, out var uri))
            {
                return uri;
            }
            throw new UriFormatException("Invalid URL format.");
        }
    }

    [DataMember(Name = "serviceName")]
    public string ServiceName { get; set; }
}
```