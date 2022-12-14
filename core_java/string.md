[На главную](../README.md)

- Где хранятся стринги

# String
+ [Что такое «пул строк»?](#Что-такое-«пул-строк»)

## Что такое «пул строк»?
Пул строк – это набор строк, хранящийся в Heap.

+ Пул строк возможен благодаря неизменяемости строк в Java и реализации идеи интернирования строк;
+ Пул строк помогает экономить память, но по этой же причине создание строки занимает больше времени;
+ Когда для создания строки используются ", то сначала ищется строка в пуле с таким же значением, если находится, то просто возвращается ссылка, иначе создается новая строка в пуле, а затем возвращается ссылка на неё;
+ При использовании оператора new создаётся новый объект String. Затем при помощи метода intern() эту строку можно поместить в пул или же получить из пула ссылку на другой объект String с таким же значением;
+ Пул строк является примером паттерна «Приспособленец» (Flyweight).

## Что такое литералы?
__Литералы__ — это явно заданные значения в коде программы — константы определенного типа, которые находятся в коде в момент запуска.
```java
class Test {
   int a = 0b1101010110;
   public static void main(String[] args) {
       System.out.println("Hello world!");       
   }
}
```
В этом классе “Hello world!” — литерал.

Переменная `a` - тоже литерал.

Литералы бывают разных типов, которые определяются их назначением и способом написания. 

## Какие есть особенности класса `String`?
+ Это неизменяемый (immutable) и финализированный тип данных;
+ Все объекты класса `String` JVM хранит в пуле строк;
+ Объект класса `String` можно получить, используя двойные кавычки;
+ Можно использовать оператор `+` для конкатенации строк;
+ Начиная с Java 7 строки можно использовать в конструкции `switch`.

## Почему `String` неизменяемый и финализированный класс?
Есть несколько преимуществ в неизменности строк:

+ Пул строк возможен только потому, что строка неизменяемая, таким образом виртуальная машина сохраняет больше свободного места в _Heap_, поскольку разные строковые переменные указывают на одну и ту же переменную в пуле. Если бы строка была изменяемой, то интернирование строк не было бы возможным, потому что изменение значения одной переменной отразилось бы также и на остальных переменных, ссылающихся на эту строку.
+ Если строка будет изменяемой, тогда это станет серьезной угрозой безопасности приложения. Например, имя пользователя базы данных и пароль передаются строкой для получения соединения с базой данных и в программировании сокетов реквизиты хоста и порта передаются строкой. Так как строка неизменяемая, её значение не может быть изменено, в противном случае злоумышленник может изменить значение ссылки и вызвать проблемы в безопасности приложения.
+ Неизменяемость позволяет избежать синхронизации: строки безопасны для многопоточности и один экземпляр строки может быть совместно использован различными потоками.
+ Строки используются _classloader_ и неизменность обеспечивает правильность загрузки класса.
+ Поскольку строка неизменяемая, её `hashCode()` кэшируется в момент создания и нет необходимости рассчитывать его снова. Это делает строку отличным кандидатом для ключа в `HashMap` т.к. его обработка происходит быстрее.

## Почему `char[]` предпочтительнее `String` для хранения пароля?
С момента создания строка остаётся в пуле, до тех пор, пока не будет удалена сборщиком мусора. Поэтому, даже после окончания использования пароля, он некоторое время продолжает оставаться доступным в памяти и способа избежать этого не существует. Это представляет определённый риск для безопасности, поскольку кто-либо, имеющий доступ к памяти сможет найти пароль в виде текста.
В случае использования массива символов для хранения пароля имеется возможность очистить его сразу по окончанию работы с паролем, позволяя избежать риска безопасности, свойственного строке.

## Почему строка является популярным ключом в `HashMap` в Java?
Поскольку строки неизменяемы, их хэш код вычисляется и кэшируется в момент создания, не требуя повторного пересчета при дальнейшем использовании. Поэтому в качестве ключа `HashMap` они будут обрабатываться быстрее.

## Что делает метод `intern()` в классе `String`?.
Метод `intern()` используется для сохранения строки в пуле строк или получения ссылки, если такая строка уже находится в пуле.

## Можно ли использовать строки в конструкции `switch`?
Да, начиная с Java 7 в операторе `switch` можно использовать строки, ранние версии Java не поддерживают этого. При этом:

+ участвующие строки чувствительны к регистру;
+ используется метод `equals()` для сравнения полученного значения со значениями `case`, поэтому во избежание `NullPointerException` стоит предусмотреть проверку на `null`.
+ согласно документации, Java 7 для строк в `switch`, компилятор Java формирует более эффективный байткод для строк в конструкции `switch`, чем для сцепленных условий `if`-`else`.

## Какая основная разница между `String`, `StringBuffer`, `StringBuilder`?
Класс `String` является неизменяемым (_immutable_) - модифицировать объект такого класса нельзя, можно лишь заменить его созданием нового экземпляра.

Класс `StringBuffer` изменяемый - использовать `StringBuffer` следует тогда, когда необходимо часто модифицировать содержимое.

Класс `StringBuilder` был добавлен в Java 5 и он во всем идентичен классу `StringBuffer` за исключением того, что он не синхронизирован и поэтому его методы выполняются значительно быстрей.

## Что такое `StringJoiner`?
Класс `StringJoiner` используется, чтобы создать последовательность строк, разделенных разделителем с возможностью присоединить к полученной строке префикс и суффикс:

```java
StringJoiner joiner = new StringJoiner(".", "prefix-", "-suffix");
for (String s : "Hello the brave world".split(" ")) {
    joiner.add(s);
}
System.out.println(joiner); //prefix-Hello.the.brave.world-suffix
```

