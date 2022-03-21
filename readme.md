# Большое домашнее задание 2: scheme

В этом задании вам предстоит реализовать интерпретатор для
lisp-подобного языка программирования, а именно некоторого подмножества scheme. 

Язык будет состоять из:
 - Примитивных типов: целых чисел, bool-ов и _символов_ (идентификаторов).
 - Составных типов: пар и списков.
 - Переменных с синтаксической областью видимости.
 - Функций и лямбда-выражений.

Ваша программа должна будет выполнять выражения языка и возвращать результат выполнения.

```
    1 => 1
    (+ 1 2) => 3
```
Обозначение `=>` в примерах здесь и далее разделяет выражение и результат его выполнения.

## Выполнение выражений
Выполнение языка происходит в 3 этапа:

**Токенизация** - преобразует текст программы в последовательность
   атомарных лексем. 

**Синтаксический анализ** - преобразует последовательность токенов
   в [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree).  AST в
   лисп-подобных языках программирования представляется в виде
   списков. 
   
**Вычисление** - рекурсивно обходит AST программы и преобразует его
   в соответствии с набором правил.

### Пример

Выражение 
```
    (+ 2 (/ -3 +4))
``` 
в результате токенизации превратится в список токенов:
```
    { 
        OpenParen(),
        Symbol("+"),
        Number(2),
        OpenParen(),
        Symbol("/"),
        Number(-3),
        Number(4),
        CloseParen(),
        CloseParen()
    }
```
     
 Последовательность токенов в результате синтаксического анализа
 превратится в дерево:
     
```
    Cell{
        Symbol("+"),
        Cell{
            Number(2),
            Cell{
                Cell{
                    Symbol("/"),
                    Cell{
                        Number(-3),
                        Cell{
                            Number(4),
                            nullptr
                        }
                    }
                }
                nullptr
            }
        }
    }
```
Результатом же выполнения выражения будет 

```
    (+ 2 (/ -3 +4)) => 1
```

## С чего начать

Задача разделена на 4 подзадачи.

| №  | Название                             | Стоимость  | О чем пункт                                                                                      |
| ---|--------------------------------------|------------|-----------------------------------------------------------------------------------------------------|
| 1  | **[tokenizer](tokenizer/README.md)** | 2 балла     | Здесь вы напишете токенизатор --- класс, который преобразует входной поток символов в последовательность токенов.                                                   |
| 2  | **[parser](parser/README.md)**       | 2 балла    | В этом пункте вы реализуете парсер --- набор методов, который читает поток токенов и строит по ним синтаксическое дерево.                                                                            |
| 3  | **[basic](basic/README.md)**         | 3 балла    | Имея парсер, можно описывать процесс вычисления. В простой версии не будет переменных и лямбда-функций.                            |
| 4  | **[advanced](advanced/README.md)**   | 3 балла    | В последней части нужно поддержать условные выражения, создание переменных и лямбда-функции, которые будут уметь захватывать контекст.   |


## Дополнительные материалы

* Видео-урок [введение в scheme](https://www.youtube.com/watch?v=AqBxU-Zmx00) объяснит базовые конструкции языка.


* Для погружения в язык стоит воспользоваться интерактивным интерпретатором.
Такого рода среды программирования называются REPL (read-eval-print loop) и существуют для самых разных языков программирования. 
[REPL для BiwaScheme](https://repl.it/languages/scheme) - поэкспереминтируйте в нём с выполнением выражений, разберитесь с польской нотацией, синтаксисом языка.
К сожалению, не существует каноничной реализации языка, все реализации обладают своим уникальным набором фичей и багов. Пример другого интерпретатора [compile scheme online](https://rextester.com/l/scheme_online_compiler).
Готовые интерпретаторы предлагаются только для ознакомления с языком, вам же предстоит написать свой язык.

## Further reading

* Книга [Build Your Own Lisp](http://www.buildyourownlisp.com/) разбирает детали
реализации интерпретатора на языке C.


* Книга [Crafting Interpreters](http://craftinginterpreters.com/) разбирает реализацию
интерпретатора для более сложного языка, чем lisp.