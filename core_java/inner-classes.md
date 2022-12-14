[На главную](../README.md)

# Inner Classes
+ [Какие типы классов бывают в java??](#Какие-типы-классов-бывают-в-java)

## Какие типы классов бывают в java?
+ Top level class (Обычный класс):
   - Abstract class (Абстрактный класс);
   - Final class (Финализированный класс).
+ Interfaces (Интерфейс).
+ Enum (Перечисление).
+ Nested class (Вложенный класс):
   - Static nested class (Статический вложенный класс);
   - Member inner class (Простой внутренний класс);
   - Local inner class (Локальный класс);
   - Anonymous inner class (Анонимный класс).

## Расскажите про вложенные классы. В каких случаях они применяются?
Класс называется вложенным (_Nested class_), если он определен внутри другого класса. Вложенный класс должен создаваться только для того, чтобы обслуживать обрамляющий его класс. Если вложенный класс оказывается полезен в каком-либо ином контексте, он должен стать классом верхнего уровня. Вложенные классы имеют доступ ко всем (в том числе приватным) полям и методам внешнего класса, но не наоборот. Из-за этого разрешения использование вложенных классов приводит к некоторому нарушению инкапсуляции.

Существуют четыре категории вложенных классов:
+ _Static nested class_ (Статический вложенный класс);
+ _Member inner class_ (Простой внутренний класс);
+ _Local inner class_ (Локальный класс);
+ _Anonymous inner class_ (Анонимный класс).

Такие категории классов, за исключением первого, также называют внутренними (_Inner class_). Внутренние классы ассоциируются не с внешним классом, а с экземпляром внешнего.

Каждая из категорий имеет рекомендации по своему применению. Если вложенный класс должен быть виден за пределами одного метода или он слишком длинный для того, чтобы его можно было удобно разместить в границах одного метода и если каждому экземпляру такого класса необходима ссылка на включающий его экземпляр, то используется нестатический внутренний класс. В случае, если ссылка на обрамляющий класс не требуется - лучше сделать такой класс статическим. Если класс необходим только внутри какого-то метода и требуется создавать экземпляры этого класса только в этом методе, то используется локальный класс. А, если к тому же применение класса сводится к использованию лишь в одном месте и уже существует тип, характеризующий этот класс, то рекомендуется делать его анонимным классом.

## Что такое _«статический класс»_?
Это вложенный класс, объявленный с использованием ключевого слова `static`. К классам верхнего уровня модификатор `static` неприменим.

## Какие существуют особенности использования вложенных классов: статических и внутренних? В чем заключается разница между ними?

+ Вложенные классы могут обращаться ко всем членам обрамляющего класса, в том числе и приватным.
+ Для создания объекта статического вложенного класса объект внешнего класса не требуется.
+ Из объекта статического вложенного класса нельзя обращаться к не статическим членам обрамляющего класса напрямую, а только через ссылку на экземпляр внешнего класса.
+ Обычные вложенные классы не могут содержать статических методов, блоков инициализации и классов. Статические вложенные классы - могут.
+ В объекте обычного вложенного класса хранится ссылка на объект внешнего класса. Внутри статической такой ссылки нет. Доступ к экземпляру обрамляющего класса осуществляется через указание `.this` после его имени. Например: `Outer.this`.

## Что такое _«локальный класс»_? Каковы его особенности?
__Local inner class__ (Локальный класс) - это вложенный класс, который может быть декларирован в любом блоке, в котором разрешается декларировать переменные. Как и простые внутренние классы (_Member inner class_) локальные классы имеют имена и могут использоваться многократно. Как и анонимные классы, они имеют окружающий их экземпляр только тогда, когда применяются в нестатическом контексте.

Локальные классы имеют следующие особенности:

+ Видны только в пределах блока, в котором объявлены;
+ Не могут быть объявлены как `private`/`public`/`protected` или `static`;
+ Не могут иметь внутри себя статических объявлений методов и классов, но могут иметь финальные статические поля, проинициализированные константой;
+ Имеют доступ к полям и методам обрамляющего класса;
+ Могут обращаться к локальным переменным и параметрам метода, если они объявлены с модификатором `final`.

## Что такое _«анонимные классы»_? Где они применяются?
Это вложенный локальный класс без имени, который разрешено декларировать в любом месте обрамляющего класса, разрешающем размещение выражений. Создание экземпляра анонимного класса происходит одновременно с его объявлением. В зависимости от местоположения анонимный класс ведет себя как статический либо как нестатический вложенный класс - в нестатическом контексте появляется окружающий его экземпляр.

Анонимные классы имеют несколько ограничений:

+ Их использование разрешено только в одном месте программы - месте его создания;
+ Применение возможно только в том случае, если после порождения экземпляра нет необходимости на него ссылаться;
+ Реализует лишь методы своего интерфейса или суперкласса, т.е. не может объявлять каких-либо новых методов, так как для доступа к ним нет поименованного типа.

Анонимные классы обычно применяются для:

+ создания объекта функции (_function object_), например, реализация интерфейса `Comparator`;
+ создания объекта процесса (_process object_), такого как экземпляры классов `Thread`, `Runnable` и подобных;
+ в статическом методе генерации;
+ инициализации открытого статического поля `final`, которое соответствует сложному перечислению типов, когда для каждого экземпляра в перечислении требуется отдельный подкласс.

## Каким образом из вложенного класса получить доступ к полю внешнего класса?
Статический вложенный класс имеет прямой доступ только к статическим полям обрамляющего класса.

Простой внутренний класс, может обратиться к любому полю внешнего класса напрямую. В случае, если у вложенного класса уже существует поле с таким же литералом, то обращаться к такому полю следует через ссылку на его экземпляр. Например: `Outer.this.field`.
