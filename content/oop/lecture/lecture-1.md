## ООП - это парадигма программирования

Парадигма программирования определяет:
* Какие «строительные блоки» мы используем при написании программ
* Как мы структурируем наши программы
* Как мы связываем наши компоненты, как объединяем их в единую систему

## Какие бывают парадигмы?

* Императивное программирование
* Структурное программирование
* Объектно-ориентированное программирование
* Декларативное программирование* Логическое программирование Функциональное программирование

## А что с языками? Обычно языки программирования поддерживают одну или несколько парадигм, но не все сразу

| Парадигма программирования | Языки                                              |
| -------------------------- | -------------------------------------------------- |
| Императивная               | Pascal, Basic, C++, C, Java, Python, ...           |
| Структурная                | Pascal, Basic, C++, C, Java, Python, ...           |
| ООП                        | C++, C#, Java, Kotlin, Python, ...                 |
| Декларативная              | Proglog, Planner, Ether, R, Python, Scala, ...     |
| Логическая                 | Proglog, Planner, Ether                            |
| Функциональная             | F#, Erlang, R, Wolfram, Python, Kotlin, Scala, ... |

## История ООП

| Очень простые, очень маленькие программы, сотни строк кода | Программы посложнее, тысячи строк кода | Сложные программы с богатым, динамичным функционалом, десятки и сотни тысяч строк кода |
| ---------------------------------------------------------- | -------------------------------------- | -------------------------------------------------------------------------------------- |
| Один программист                                           | Несколько программистов                | Десятки, сотни программистов                                                           |
| Императивное программирование                              | Структурное программирование           | Объектно-ориентированное программирование                                              |

Развитие структурного программирования
* 1967 г. - язык Simula, первый прообраз
* 1972 г. - язык Smalltalk
* 1983 г. -языки Objective-C, C++

## Так в чём же особенность ООП?

Центральное понятие в ООП - это КЛАСС Класс - это объединение данных и операции с этими данными Класс - это описание, схема, в соответствии с которой в программе создаются, работают и уничтожаются экземпляры этого класса - ОБЪЕКТЫ

## А в чём преимущества?

* Легче расширять функционал
* Больше кода используется повторно
* Изменения функциональности затрагивают только классы, ответственные за этот функционал

## Есть и недостатки

- Больше времени и сил на проектирование
- Суммарно пишется больше кода

## Классы в С\#

```C#
using System;
using System.Drawing;

namespace IUDO_OOP
{
    public class Car
    {
        private Color _color;

        public Car(string manufacturer) : this(Color.White, manufacturer) { }

        public Car(Color color, string manufacturer)
        {
            if (!IsColorAllowed(color)) throw new ArgumentOutOfRangeException(nameof(color));
            _color = color;
            Manufacturer = manufacturer;
        }

        public string Manufacturer { get; }

        public Color Color
        {
            get => _color;
            private set
            {
                if (!IsColorAllowed(value)) return;

                var oldColor = _color;
                _color = value;
                ColorChanged?.Invoke(this, oldColor, newColors: value);
            }
        }

        public virtual bool ChangeColor(Color newColor)
        {
            var allowed = IsColorAllowed(newColor);
            if (allowed) Color = newColor;
            return allowed;
        }

        private static bool IsColorAllowed(Color color) => color != Color.Blue;

        public delegate void ColorChangedHandler(Car car, Color oldColor, Color newColor);
        public event ColorChangedHandler ColorChanged;
    }
}
```

