***

<p align="center">
🎉 Ультра-упрощенное объяснение шаблонов проектирования! 🎉
</p>
<p align="center">
Тема, которая может легко заставить любой разум колебаться. Здесь я пытаюсь сделать её более запоминающейся для вас<br> (и, возможно, для себя), объясняя её самым <i>простым</i> способом.
</p>

***

<br>

|[Порождающие шаблоны проектирования](#порождающие-шаблоны-проектирования)|[Структурные шаблоны проектирования](#структурные-шаблоны-проектирования)|[Поведенческие шаблоны проектирования](#поведенческие-шаблоны-проектирования)|
|:-|:-|:-|
|[Фабрика](#-фабрика)|||
|[Фабричный метод](#-фабричный-метод)|||


<br>

Введение
=================

Шаблоны проектирования — это решения повторяющихся проблем; **рекомендации по решению определенных проблем**. Это не классы, пакеты или библиотеки, которые вы можете подключить к своему приложению и ждать, пока произойдет волшебство. Это, скорее, рекомендации о том, как решать определенные проблемы в определенных ситуациях.

> Шаблоны проектирования — это решения повторяющихся проблем; рекомендации по решению определенных проблем

Wikipedia описывает их как

> В разработке программного обеспечения шаблон проектирования программного обеспечения — это общее многократно используемое решение часто встречающейся проблемы в данном контексте при проектировании программного обеспечения. Это не законченный дизайн, который можно преобразовать непосредственно в исходный или машинный код. Это описание или шаблон того, как решить проблему, который можно использовать во многих различных ситуациях.

⚠️ Будьте осторожны
-----------------
- Шаблоны дизайна не являются серебрянной пулей от всех ваших проблем.
- Не пытайтесь заставить их; если это сделать, то произойдет что-то плохое.
- Имейте в виду, что шаблоны проектирования — это **готовое** решение проблем, а не **поиск** решений; так что не думайте слишком много.
- Если использовать их в правильном месте и правильным образом, они могут оказаться спасителями; иначе они могут привести к ужасному беспорядку в коде.

> Также обратите внимание, что приведенные ниже примеры кода на PHP-7, однако это не должно вас останавливать, потому что концепции в любом случае те же.

Типы шаблонов проектирования
-----------------

* [Порождающие](#creational-design-patterns)
* [Структурные](#structural-design-patterns)
* [Поведенческие](#behavioral-design-patterns)

Порождающие шаблоны проектирования
==========================

Простыми словами
> Порождающие шаблоны сосредоточены на том, как создать экземпляр объекта или группы связанных объектов.

Wikipedia говорит
> В программной инженерии порождающие шаблоны проектирования — это шаблоны проектирования, которые имеют дело с механизмами создания объектов, пытаясь создать объекты способом, подходящим для ситуации. Базовая форма создания объектов может привести к проблемам с дизайном или усложнить дизайн. Порождающие шаблоны проектирования решают эту проблему, тем или иным образом образом контролируя создание этого объекта.

 * [Фабрика](#-фабрика)
 * [Фабричный метод](#-фабричный-метод)

🏠 Фабрика
--------------

Пример из жизни
> Предположим, вы строите дом и вам нужны двери. Вы можете либо надеть свою плотницкую одежду, принести немного дерева, клея, гвоздей и все необходимые инструменты, для изготовления двери и начать строить ее в своем доме, либо вы можете просто позвонить на фабрику и получить готовую дверь, чтобы вам не нужно было ничего узнавать о изготовлении дверей или разбираться с беспорядком, который возникает при ее изготовлении.

Простыми словами
> Фабрика просто генерирует экземпляр для клиента, не раскрывая ему никакой логики инстанцирования

Wikipedia говорит
> В объектно-ориентированном программировании (ООП) фабрика — это объект для создания других объектов — формально фабрика — это функция или метод, который возвращает объекты изменяющегося прототипа или класса из некоторого вызова метода, который предполагается «новым».

**Пример кода**

В первую очередь у нас есть интерфейс Дверь(Door) и реализация
```php
interface Door
{
    public function getWidth(): float;
    public function getHeight(): float;
}

class WoodenDoor implements Door
{
    protected $width;
    protected $height;

    public function __construct(float $width, float $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getWidth(): float
    {
        return $this->width;
    }

    public function getHeight(): float
    {
        return $this->height;
    }
}
```

Затем у нас есть фабрика(DoorFactory) по производству дверей(WoodenDoor), которая изготавливает дверь(WoodenDoor) и возвращает ее
```php
class DoorFactory
{
    public static function makeDoor($width, $height): Door
    {
        return new WoodenDoor($width, $height);
    }
}
```

И тогда её можно использовать как
```php
// Сделай мне дверь 100x200
$door = DoorFactory::makeDoor(100, 200);

echo 'Width: ' . $door->getWidth();
echo 'Height: ' . $door->getHeight();

// Сделай мне дверь 50x100
$door2 = DoorFactory::makeDoor(50, 100);
```

**Когда использовать?**

Когда создание объекта — это не просто несколько действий, а включает в себя некоторую логику, тогда имеет смысл поместить его в выделенную фабрику, а не повторять везде один и тот же код.

🏭 Фабричный метод
--------------

Пример из жизни
> Рассмотрим случай с менеджером по найму. На каждую из должностей невозможно провести собеседование с одним человеком. В зависимости от вакансии она должна принять решение и делегировать этапы собеседования разным людям.

Простыми словами
> Фабричный метод предоставляет способ делегировать логику создания экземпляров дочерним классам.

Wikipedia говорит
> В программировании на основе классов фабричный метод — это шаблон создания, который использует фабричные методы для решения проблемы создания объектов без необходимости указывать точный класс создаваемого объекта. Это делается путем создания объектов путем вызова фабричного метода, либо указанного в интерфейсе и реализованного дочерними классами, либо реализованного в базовом классе и опционально переопределяемого производными классами, а не путем вызова конструктора.

**Пример кода**

Возьмем пример с нашим менеджером по найму выше. Прежде всего, у нас есть интерфейс Интервьюер(Interviewer) и несколько реализаций для него
```php
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo 'Вопрос о шаблонах дизайна';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo 'Вопрос о построении сообщества';
    }
}
```

Теперь давайте создадим нашего менеджера по найму(HiringManager)
```php
abstract class HiringManager
{

    // Фабричный метод
    abstract protected function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}

```

Теперь любой ребенок может расширить его и предоставить необходимого интервьюера(Interviewer)
```php
class DevelopmentManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new Developer();
    }
}

class MarketingManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new CommunityExecutive();
    }
}
```

И тогда это можно использовать как
```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // Вывод: Вопрос о шаблонах проектирования

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // Вывод: Вопрос о построении сообщества
```

**Когда использовать?**

Полезно, когда в классе есть некоторая обобщенная обработка, но требуемый подкласс динамически определяется во время выполнения. Или, другими словами, когда клиент не знает какой именно подкласс ему может понадобиться.
