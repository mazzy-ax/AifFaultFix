# AifFaultFix

[project]:https://github.com/mazzy-ax/AifFaultFix
[license]:https://github.com/mazzy-ax/AifFaultFix/blob/master/LICENSE
[ax2009]:ax2009

[AifFaultFix][project] &ndash; это код, который исправляет в [Microsoft Dynamics AX 2009][ax2009] ошибку `Type 'Wcf.AifFault' in assembly 'Wcf, version=0.0.0.0...' is not marked as serializable.`.

![AifFault not marked as serializable](https://github.com/mazzy-ax/AifFaultFix/wiki/img/AifFault-not-marked-as-serializable.png)

## Как работает fix

Класс `AifServiceReferenceManager` в Аксапте генерирует исходные тексты клиентской части `WCF` и компилирует dll из этих исходных текстов. К сожалению, разработчики Microsoft не проставили несколько атрибутов в "системных" классах, которые обслуживают исключения в `WCF`.

Проект `AifFaultFix` добавляет метод на X++ в класс [AifServiceReferenceManager](ax2009/Src/Class_AifServiceReferenceManager.xpp). Добавленный код вносит исправления в исходный текст `WcfSoapClient.cs` перед тем, как генератор начнет компилировать dll.

После добавления проекта в вашу ax2009, нужно перегенерировать Web Reference (удалить-создать заново или правой кнопкой мыши и пункт Восстановить). После перегенерации вы будете видеть в вашей ax2009 текст исключения, которое было брошено на сервере.

![Custom error from WCF service](https://github.com/mazzy-ax/AifFaultFix/wiki/img/custom-error.png)

## Исправления в WcfSoapClient.cs

````csharp
[System.Diagnostics.DebuggerStepThroughAttribute()]
[System.CodeDom.Compiler.GeneratedCodeAttribute("System.Runtime.Serialization", "3.0.0.0")]
[System.Runtime.Serialization.DataContractAttribute(Name="AifFault", Namespace="http://schemas.microsoft.com/dynamics/2008/01/documents/Fault")]
[System.SerializableAttribute()]  // <-------- AifFaultFix
public partial class AifFault : object, System.Runtime.Serialization.IExtensibleDataObject
{
    [System.NonSerializedAttribute()]  // <-------- AifFaultFix
    private System.Runtime.Serialization.ExtensionDataObject extensionDataField;
...
}

[System.Diagnostics.DebuggerStepThroughAttribute()]
[System.CodeDom.Compiler.GeneratedCodeAttribute("System.Runtime.Serialization", "3.0.0.0")]
[System.Runtime.Serialization.DataContractAttribute(Name="InfologMessage", Namespace="http://schemas.datacontract.org/2004/07/Microsoft.Dynamics.AX.Framework.Services")]
[System.SerializableAttribute()]  // <-------- AifFaultFix
public partial class InfologMessage : object, System.Runtime.Serialization.IExtensibleDataObject
{
...
}
````

## ChangeLog

* [CHANGELOG.md](CHANGELOG.md)
* <https://github.com/mazzy-ax/AifFaultFix/releases>

## Помощь проекту

Буду признателен за ваши замечания, предложения и советы по проекту как в разделе [Issues](https://github.com/mazzy-ax/AifFaultFix/issues), так и в виде письма на адрес <mazzy@mazzy.ru>

Мазуркин Сергей (mazzy)
