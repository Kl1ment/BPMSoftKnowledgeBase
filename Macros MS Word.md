
Использование макроса в отчете - `ИмяКолонки[#НазваниеМакроса|Параметры#]`

```C#
namespace BPMSoft.Configuration
{
	using System;
	using System.Text;
 
	[ExpressionConverterAttribute("ReplaceEmptyString")]
	public class ReplaceEmptyString : IExpressionConverter 
	{
 
		public string Evaluate(object value, string arguments = "") 
        {
        
		}
	}
}
```

**namespace обязательно должен быть BPMSoft.Configuration**
Так как в классе ExpressionConverterHelper в методе GetValue присутствует строка `string.Format("BPMSoft.Configuration.{0}", type.Name));`

Получить UserConnection можно из сессии
`(UserConnection)HttpContext.Current.Session["UserConnection"];`
