---
title: Выбор кодировщика сообщений
ms.date: 03/30/2017
ms.assetid: 2204d82d-d962-4922-a79e-c9a231604f19
ms.openlocfilehash: a306896af7a73d43956638981908c12d86126a9f
ms.sourcegitcommit: 59e36e65ac81cdd094a5a84617625b2a0ff3506e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2020
ms.locfileid: "80345258"
---
# <a name="choosing-a-message-encoder"></a>Выбор кодировщика сообщений
На этой теме обсуждаются критерии выбора среди кодеров сообщений, включенных в Фонд коммуникаций Windows (WCF): двоичный, текстовый и механизм оптимизации передачи сообщений (MTOM).  
  
 В WCF указано, как передавать данные по сети между конечными точками с помощью *связывания,* которая состоит из последовательности *связывающих элементов.* Кодировщик сообщений - это элемент привязки кодирования сообщений в стеке привязки. В привязку входят необязательные элементы привязки протокола, такие как элемент привязки безопасности или элемент привязки надежного обмена сообщениями, обязательный элемент привязки кодирования сообщений, и обязательный элемент привязки транспорта.  
  
 Элемент привязки кодирования сообщений находится под необязательными элементами привязки протокола и над обязательным элементом привязки транспорта. На стороне отправки кодировщик сообщений сериализует исходящий объект <xref:System.ServiceModel.Channels.Message> и передает его транспорту. На стороне приема кодировщик сообщений получает сериализованную форму объекта <xref:System.ServiceModel.Channels.Message> от транспорта и передает его на более высокий уровень протоколов, если он имеется, или приложению в противном случае.  
  
 При соединении с существующим клиентом или сервером возможно, что выбирать кодировщик не придется: кодировать сообщения необходимо способом, которого ожидает другая сторона. Однако, если вы пишете службу WCF, вы можете разоблачить вашу службу через несколько конечных точек, каждая из которых использует различные кодирования сообщений. Это позволяет клиентам выбирать оптимальный способ кодирования для взаимодействия со службой через оптимальную для них конечную точку, т. е. клиентам предоставляется возможность выбирать способ кодирования, лучше всего им подходящий. Использование нескольких конечных точек также позволяет сочетать преимущества разных способов кодирования сообщений с другими элементами привязки.  
  
## <a name="system-provided-encoders"></a>Кодировщики, предоставляемые системой  
 WCF включает в себя три кодера сообщений, которые представлены следующими тремя классами:  
  
- <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> - текстовый кодировщик сообщений - поддерживает и кодирование в простой XML, и кодирование SOAP. Режим кодирования в простой XML текстового кодировщика называется "plain old XML" (POX), что позволяет отличить его от текстового кодирования SOAP. Чтобы включить POX, присвойте свойству <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement.MessageVersion%2A> значение <xref:System.ServiceModel.Channels.MessageVersion.None%2A>. Используйте кодер текстового сообщения для работы с конечными точками, не входящих в WCF.  
  
- <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>, бинарный кодеер сообщения, использует компактный двоичный формат и оптимизирован для WCF к WCF связи, и, следовательно, не совместим. Это также самый эффективный кодер из всех кодеров WCF предоставляет.  
  
- <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> - элемент привязки - задает способ кодирования и управления версиями для сообщений, использующих кодирование MTOM. MTOM - это эффективный способ передачи двоичных данных в сообщениях WCF. Кодировщик MTOM пытается сохранить баланс между эффективностью и совместимостью. Кодирование MTOM передает большую часть XML-данных в текстовой форме, но оптимизирует большие блоки двоичных данных путем передачи их в исходном виде, без преобразования в текст. С точки зрения эффективности, среди кодеров WCF обеспечивает, MTOM находится между текстом (самый медленный) и двоичный (самый быстрый).  
  
## <a name="how-to-choose-a-message-encoder"></a>Выбор кодировщика сообщений  
 В следующей таблице приведены факторы, чаще всего влияющие на выбор кодировщика сообщений. Расположите эти факторы в соответствии с их приоритетом для данного приложения и выберите оптимальные кодировщики сообщений. Не забудьте принять во внимание все дополнительные факторы, не приведенные в этой таблице, а также пользовательские кодировщики сообщений, которые могут понадобиться в приложении.  
  
|Фактор|Описание|Кодировщики, поддерживающие этот фактор|  
|------------|-----------------|---------------------------------------|  
|Поддерживаемые кодировки|<xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>и <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> поддерживают только кодирования UTF8 и UTF16 Unicode *(большие* и *малоконечные*коды). Если требуются другие кодировки, такие как UTF7 или ASCII, необходимо использовать пользовательский кодировщик. Для примера пользовательского кодера [см.](https://docs.microsoft.com/dotnet/framework/wcf/samples/custom-message-encoder-custom-text-encoder)|Text|  
|Проверка|Под проверкой понимается возможность анализа сообщений во время передачи. Текстовое кодирование (как с использованием SOAP, так и без) позволяет ряду приложений проверять и анализировать сообщения без использования специализированных инструментов. Обратите внимание, что возможность проверки сообщений зависит от использования механизма безопасности транспорта (на уровне сообщения или на уровне транспорта). Конфиденциальность подразумевает защиту сообщения от анализа, а целостность - от изменения.|Text|  
|Надежность|Надежность - это устойчивость кодировщика к ошибкам передачи. Надежность также может обеспечиваться на уровне сообщения, транспорта или приложения. Все стандартные кодеры WCF предполагают, что еще один слой обеспечивает надежность. Способность кодировщика восстановиться после ошибки передачи весьма ограничена.|None|  
|Простота|Под простотой понимается простота создания кодировщиков и декодеров по спецификации кодирования. В этом отношении особо предпочтительны текстовые способы кодирования, причем текстовое кодирование POX имеет дополнительное преимущество - не требует поддержки обработки SOAP.|Текстовый (POX)|  
|Размер|Способ кодирования определяет объем служебных данных, налагаемых на содержимое. Размер кодированных сообщений напрямую связан с максимальной пропускной способностью операций службы. Двоичные способы обычно обеспечивают более компактное кодирование, чем текстовые. Когда размер сообщений имеет большое значение, следует рассмотреть также сжатие содержимого сообщений при кодировании. При этом следует помнить, что сжатие увеличивает вычислительные затраты как на стороне отправки, так и на стороне приема сообщений.|Двоичные данные|  
|Потоковая передача|Потоковая передача позволяет приложениям начинать обработку сообщения до получения всего сообщения. Эффективное использование потоковой передачи требует, чтобы важные данные содержались в начале сообщения, т. е. получающему приложению не нужно было дожидаться их поступления. Кроме того, приложения, использующие потоковую передачу, должны организовывать данные в сообщениях инкрементно, чтобы в содержимом не было зависимостей от последующих данных. Во многих случаях приходится идти на компромисс между потоковой передачей сообщения и минимальным размером передачи для содержимого.|None|  
|Поддержка сторонних инструментов|Возможная поддержка включает разработку и диагностику. Сторонние разработчики делают большие вложения в библиотеки и наборы инструментов для обработки сообщений, кодированных в формат POX.|Текстовый (POX)|  
|Взаимодействие|Этот фактор относится к способности кодера WCF работать с не-WCF услуг.|Text<br /><br /> MTOM (частично)|  
  
Примечание. При использовании двоичного кодировщика использование параметра IgnoreWhitespace при создании XmlReader не оказывает никакого влияния.  Например, если в операции службы выполняются следующие действия.  

```csharp
public void OperationContract(XElement input)
{
    Console.WriteLine("{0}", input.Value);
    int counter = 0;
    var xreader = input.CreateReader();
    var reader = XmlReader.Create(xreader, new XmlReaderSettings() { IgnoreWhitespace = true });
    while (reader.Read())
    {
        counter++;
    }

    Console.WriteLine("Read {0} lines with reader", counter);
}
```  
  
Параметр IgnoreWhitespace не учитывается.  
  
## <a name="compression-and-the-binary-encoder"></a>Сжатие и двоичный кодировщик

Начиная с версии WCF 4.5, в двоичном кодировщике появилась поддержка сжатия. Это позволяет использовать алгоритм gzip/deflate для передачи сжатых сообщений из WCF-клиента и отправлять сжатые сообщения из резидентной службы WCF. Эта возможность обеспечивает сжатие сообщения для HTTP и TCP-каналов. WCF-службу, размещенную в IIS, можно настроить на повсеместную отправку сжатых ответов, выполнив соответствующую настройку узла IIS. Тип сжатия задается с помощью свойства <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.CompressionFormat%2A?displayProperty=nameWithType>. Свойству присваивается одно из значений перечисления <xref:System.ServiceModel.Channels.CompressionFormat?displayProperty=nameWithType>.

- <xref:System.ServiceModel.Channels.CompressionFormat.Deflate>
- <xref:System.ServiceModel.Channels.CompressionFormat.GZip>
- <xref:System.ServiceModel.Channels.CompressionFormat.None>
  
Так как это свойство разоблачается только на binaryMessageEncodingBindingElement, вам нужно будет создать пользовательский связывание, как следующее, чтобы использовать эту функцию:

 ```xml
 <customBinding>
   <binding name="BinaryCompressionBinding">
     <binaryMessageEncoding compressionFormat ="GZip" />
     <httpTransport />
  </binding>
</customBinding>
 ```

Как клиент, так и служба должны договориться об отправке и получении сжатых сообщений, и поэтому свойство compressionFormat должно быть настроено на элемент binaryMessageEncoding как на клиенте, так и на сервисе. Исключение ProtocolException выдается, если служба или клиент не использует сжатие, но оно используется на другой стороне. Следует внимательно обдумать возможность использования сжатия. Сжатие наиболее полезно в том случае, когда пропускная способность канала является узким местом в системе. В тех случаях, когда узким местом для производительности является ЦП, сжатие снизит пропускную способность системы. Для определения полезности применения сжатия для работы системе следует выполнить соответствующие тесты в симулированной среде выполнения.  
  
## <a name="see-also"></a>См. также

- [Привязки](../../../../docs/framework/wcf/feature-details/bindings.md)
