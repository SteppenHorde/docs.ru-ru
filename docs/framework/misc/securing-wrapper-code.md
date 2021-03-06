---
title: Безопасность кода программы-оболочки
ms.date: 03/30/2017
helpviewer_keywords:
- security [.NET Framework], wrapper code
- wrapper code, securing
- secure coding, wrapper code
- code security, wrapper code
ms.assetid: 1df6c516-5bba-48bd-b450-1070e04b7389
ms.openlocfilehash: 3d38a4d4fd33798cf5987f5ce67305725ad9daec
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2020
ms.locfileid: "79400935"
---
# <a name="securing-wrapper-code"></a>Безопасность кода программы-оболочки
[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 Код программы-оболочки, особенно когда она имеет более высокий уровень доверия, чем код, который ее использует, может открыть уникальный набор уязвимостей системы безопасности. Все, что делается от имени вызывающего объекта, когда его ограниченные разрешения не включаются в соответствующую проверку безопасности, является потенциальной уязвимостью и может быть использовано злоумышленником.  
  
 Никогда не позволяйте осуществлять с помощью программы-оболочки действия, которые вызывающий объект не может выполнить самостоятельно. Особую опасность представляют действия, подразумевающие ограниченную проверку безопасности, в отличие от требования полного обхода стека. При использовании одноуровневых проверок внедрение кода программы-оболочки между настоящим вызывающим объектом и элементом API может легко привести к успешному прохождению проверки безопасности, когда это не должно было произойти, что ослабляет защиту.  
  
## <a name="delegates"></a>Делегаты  
 Защита делегатов различается в разных версиях .NET Framework.  В этом разделе описывается различное поведение делегатов и соответствующие вопросы безопасности.  
  
### <a name="in-version-10-and-11-of-the-net-framework"></a>В .NET Framework версий 1.0 и 1.1  
 В .NET Framework версий 1.0 и 1.1 с создателем делегата и вызывающим делегат объектом выполняются следующие действия по обеспечению безопасности.  
  
- При создании делегата требования связывания безопасности в целевом методе делегата выполняются по отношению к набору привилегий создателя делегата.  В результате сбоя при выполнении действия по обеспечению безопасности возникает исключение <xref:System.Security.SecurityException>.  
  
- При вызове делегата выполняются все существующие требования безопасности в вызывающем делегат объекте.  
  
 Каждый раз, когда код принимает <xref:System.Delegate> из менее доверенного кода, который может вызывать его, убедитесь, что менее доверенному коду не разрешено повышать уровень своих разрешений. Если делегат принимается, но используется позже, то код, создавший делегат, не находится в стеке вызовов, и его разрешения не будут проверяться, если код на уровне делегата или ниже попытается выполнить защищенную операцию. Если ваш код и код вызывающего объекта обладают более высокими привилегиями, чем создатель, то создатель может управлять путем вызова, не являясь частью стека вызовов.  
  
### <a name="in-version-20-and-later-versions-of-the-net-framework"></a>В версии 2.0 и более поздних версиях рамочного соглашения .NET  
 В отличие от предыдущих версий, версия 2.0 и более поздние версии рамочного проекта .NET выполняют меры безопасности против создателя делегата при создании и вызове делегата.  
  
- При создании делегата требования связывания безопасности в целевом методе делегата выполняются по отношению к набору привилегий создателя делегата.  В результате сбоя при выполнении действия по обеспечению безопасности возникает исключение <xref:System.Security.SecurityException>.  
  
- Набор прав создателя делегата также собирается при создании делегата и сохраняется с делегатом.  
  
- При вызове делегата сохраненный набор прав создателя делегата сначала оценивается по отношению ко всем требованиям в текущем контексте, если создатель делегата и вызывающий делегат объект принадлежат разным сборкам.  Затем выполняются все существующие требования безопасности в вызывающем делегат объекте.  
  
## <a name="link-demands-and-wrappers"></a>Требования связывания и программы-оболочки  
 В инфраструктуре безопасности особая защита была усилена с помощью требований связывания, но она по-прежнему является источником уязвимости в коде.  
  
 Если полностью доверенный код вызывает свойство, событие или метод, защищенный [LinkDemand,](link-demands.md)вызов удается, если проверка разрешения **LinkDemand** для вызываемого абонента будет выполнена. Кроме того, если полностью доверенный код предоставляет класс, который принимает имя свойства и вызывает его **получить** accessor с помощью отражения, что вызов **получить** accessor удается, даже если пользовательский код не имеет права доступа к этому свойству. Это происходит потому, что **LinkDemand** проверяет только непосредственный абонент, который является полностью доверенным кодом. По существу полностью доверенный код выполняет привилегированный вызов от имени пользовательского кода, не убедившись в том, что пользовательский код имеет права для выполнения такого вызова.  
  
 Чтобы предотвратить такие дыры в безопасности, общее время выполнения языка расширяет проверку в полный спрос на ходячий стек на любой косвенный вызов к методу, конструктору, свойству или событию, защищенному **LinkDemand.** Такая защита влечет за собой некоторое снижение производительности, а также меняет семантику проверки безопасности; требование полного обхода стека может завершиться ошибкой там, где была бы пройдена более быстрая одноуровневая проверка.  
  
## <a name="assembly-loading-wrappers"></a>Программы-оболочки, загружающие сборки  
 Некоторые методы, используемые для загрузки управляемого кода, включая <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>, загружают сборки со свидетельством вызывающего объекта. Если включить в программу-оболочку любой из этих методов, система безопасности может использовать для загрузки сборки предоставленные вашему коду разрешения вместо разрешений объекта, вызывающего вашу программу-оболочку. Не следует разрешать менее доверенному коду загружать код, который имеет более высокий уровень разрешений, чем объект, вызывающий вашу программу-оболочку.  
  
 Таким образом может быть ослаблена безопасность любого кода с полным уровнем доверия или значительно более высоким уровнем доверия, чем у потенциального вызывающего объекта (включая вызывающий объект уровня Интернет-разрешений). Если ваш код имеет общедоступный метод, который принимает массив байт и передает его **на Assembly.Load**, создавая сборку от имени вызывающего абонента, это может привести к взлому безопасности.  
  
 Эта проблема относится к следующим элементам API:  
  
- <xref:System.AppDomain.DefineDynamicAssembly%2A?displayProperty=nameWithType>  
  
- <xref:System.AppDomain.Load%2A?displayProperty=nameWithType>  
  
- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>  
  
- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>  
  
## <a name="demand-vs-linkdemand"></a>Demand и LinkDemand  
 Декларативная безопасность предлагает два вида проверки безопасности, которые похожи, но выполняют совершенно разные проверки. Проанализируйте обе формы, так как неправильный выбор может привести к ослаблению безопасности или потере производительности.  
  
 Декларативная безопасность предлагает следующие проверки безопасности.  
  
- Проверка <xref:System.Security.Permissions.SecurityAction.Demand> указывает обход стека управления доступом для кода. Для прохождения этой проверки все вызывающие объекты в стеке должны иметь указанное разрешение или удостоверение. **Спрос** возникает на каждый вызов, потому что стек может содержать различные абоненты. Если метод вызывается многократно, данная проверка безопасности происходит каждый раз. **Спрос** - это хорошая защита от заманивающих атак; несанкционированный код, пытающийся пройти через него, будет обнаружен.  
  
- [LinkDemand](link-demands.md) происходит в точно в срок (JIT) компиляции времени и проверяет только немедленного абонента. Эта проверка безопасности не проверяет вызывающий объект вызывающего объекта. После того как эта проверка пройдена, никакие дополнительные проверки не выполняются, независимо от того, сколько раз вызывающий объект может выполнять вызов. Однако эта проверка не обеспечивает защиту от атак с заманиванием. С **LinkDemand**любой код, который проходит тест и может ссылаться на ваш код, потенциально может нарушить безопасность, позволив вредоносному коду звонить с помощью авторизованного кода. Поэтому не используйте **LinkDemand,** если все возможные слабости можно полностью избежать.  
  
    > [!NOTE]
    > В рамочном 4 .NET требования к <xref:System.Security.SecurityCriticalAttribute> ссылкам были заменены атрибутом в <xref:System.Security.SecurityRuleSet.Level2> сборках. Это <xref:System.Security.SecurityCriticalAttribute> эквивалентно ссылке спросна на полное доверие; однако это также влияет на правила наследования. Для получения дополнительной информации [Security-Transparent Code, Level 2](security-transparent-code-level-2.md)об этом изменении см.  
  
 Дополнительные меры предосторожности, необходимые при использовании **LinkDemand,** должны быть запрограммированы индивидуально; система безопасности может помочь в обеспечении соблюдения. Любая ошибка приводит к возникновению уязвимости в системе безопасности. Любой авторизованный код, использующий ваш код, должен отвечать за реализацию дополнительных мер защиты, выполняя следующие действия.  
  
- Ограничение доступа вызывающего кода к классу или сборке.  
  
- Размещение тех же проверок безопасности в вызывающем коде, который появляется в вызываемом коде, и принуждение вызывающих его объектов делать это. Например, если вы пишете код, который вызывает метод, защищенный **LinkDemand** <xref:System.Security.Permissions.SecurityPermission> для указанного <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode> флага, ваш метод должен также сделать **LinkDemand** (или **Спрос,** который сильнее) для этого разрешения. Исключением является использование вашего кода методом, защищенным **ссылками,** ограниченным способом, который вы решили сделать безопасным, учитывая другие механизмы защиты безопасности (например, требования) в коде. В этом исключительном случае вызывающий объект несет ответственность за понижение степени защиты в базовом коде.  
  
- Обеспечение, чтобы вызывающие ваш код объекты не могли заставить его вызывать защищенный код от их имени. Другими словами, вызывающие объекты не должны иметь возможность заставить авторизованный код передавать определенные параметры в защищенный код или получить результаты из него.  
  
### <a name="interfaces-and-link-demands"></a>Интерфейсы и требования связывания  
 Если виртуальный метод, свойство или событие с **LinkDemand** переопределяет метод базового класса, метод базового класса должен также иметь тот же **метод LinkDemand** для переовереденного метода, чтобы быть эффективным. Вредоносный код может выполнить обратное приведение к базовому типу и вызвать метод базового класса. Также обратите внимание, что требования связывания могут добавляться неявно в сборки, в которых отсутствует атрибут <xref:System.Security.AllowPartiallyTrustedCallersAttribute> уровня сборки.  
  
 Рекомендуется защищать реализации методов с помощью требований связывания, когда методы интерфейса также имеют требования связывания. Обратите внимание на следующие аспекты использования требований связывания с интерфейсами.  
  
- Если вы разместите **LinkDemand** на общедоступном методе класса, который реализует метод интерфейса, **LinkDemand** не будет применяться, если вы затем бросили в интерфейс и вызов метода. В этом случае, поскольку вы связаны с интерфейсом, только **LinkDemand** на интерфейсе соблюдается.  
  
 Изучите следующие элементы проблем безопасности.  
  
- Явные требования связывания в методах интерфейса. Убедитесь, что эти требования связывания обеспечивают ожидаемую защиту. Определите, может ли вредоносный код использовать приведение, чтобы обойти требования связывания, как описано выше.  
  
- Виртуальные методы с примененными требованиями связывания.  
  
- Типы и интерфейсы, которые они реализуют. Они должны согласованно использовать требования связывания.  
  
## <a name="see-also"></a>См. также раздел

- [Правила написания безопасного кода](../../standard/security/secure-coding-guidelines.md)
