pydonts
=======

Общие рекомендации
------------------

* Следуй [PEP-8](https://www.python.org/dev/peps/pep-0008).
* Используй операторы `is` и `is not` **только** для сравнение с синглтонами,
  например, `None`. Исключение: булевые значения `True` и `False`.
* Помни и применяй *falsy*/*truthy* семантику. *Falsy* значения --- `None`, `False`,
  нули `0`, `0.0`, `0j`, пустые строки и байты и все пустые коллекции. Все
  остальные значения *truthy*.

    ```python
    # Плохо
    if acc == []:
        # ...

    # Плохо
    if len(acc) > 0:
        # ...

    # Лучше
    if not acc:
        # ...

    # Допустимо
    if acc == 0:
        # ...
    ```
* Не копируй без необходимости.

    ```python
    # Плохо
    set([x**2 for x in range(42)])

    for x in list(sorted(xs)):
        # ...

    # Лучше
    {x**2 for x in range(42)}

    for x in sorted(xs):
        # ...
    ```
* Не используй `dict.get` и коллекцию `dict.keys` для проверки наличия ключа в
  словаре:

    ```python
    # Плохо
    if key in g.keys():
        # ...

    if not g.get(key, False):
        # ...

    # Лучше
    if key in g:
        # ...

    if key not in g:
        # ...
    ```
* Используй литералы для создания пустых коллекций. Исключение: `set`, литералов
  пустого множества в Python нет.

  ```python
  # Плохо
  dict(), list(), tuple()

  # Лучше
  {}, [], ()
  ```


Структура кода
--------------

* Не эмулируй оператор `for`, Python --- это не Scala.

    ```python
    # Плохо
    i = 0
    while i < n:
        ...
        i += 1

    # Лучше
    for i in range(n):
        ...
    ```
* Предпочитай итерацию по объекту циклам со счётчиком. Ошибка на 1 в индексе ---
  это классика. Если же индекс требуется, помни про `enumerate`.

    ```python
    # Плохо
    for i in range(len(xs)) :
        x = xs[i]

    # Лучше
    for x in xs:
        ...

    # Или
    for i, x in enumerate(xs):
        ...

    ```
* Не пиши бессмысленных операторов `if` и тернарных операторов.

    ```python
    # Плохо
    if condition:
       return True
    else
       return False

    # Лучше
    return condition
    ```

    ```python
    # Плохо
    if condition:
       return False
    else
       return True

    # Лучше
    return not condition
    ```

    ```python
    # Плохо
    xs = [x for x in xs if predicate]
    return True if xs else False

    # Лучше
    xs = [x for x in xs if predicate]
    return bool(xs)

    # Ещё лучше
    return any(map(predicate, xs))
    ```

Функции
-------

* Избегай изменяемых значений по умолчанию.
* Не злоупотребляй функциональными идиомами. Часто генератор списка, множества
  или словаря понятнее комбинации функций `map`, `filter` и `zip`.

    ```python
    # Плохо
    list(map(lambda x: x ** 2,
             filter(lambda x: x % 2 == 1,
                    range(10))))

    # Лучше
    [x ** 2 for x in range(10) if x % 2 == 1]
    ```
* Не злоупотребляй генераторами коллекций. Часто обычный цикл `for` понятней
  вложенного генератора.
* Не сворачивай функции с эффектами. Первый аргумент `functools.reduce` не
  должен изменять состояние имён во внешних областях видимости или значение
  аккумулятора.

    ```python
    # Плохо
    funtools.reduce(lambda acc, s: acc.update(s), sets,

    # Лучше
    acc = set()
    for set in sets:
        acc.update(set)
    ```


Декораторы
----------

* **Всегда** используй `functools.wraps` или `functools.update_wrapper` при
  написании декоратора.


Строки
------

* Используй методы `str.startswith` и `str.endswith`:

    ```python
    # Плохо
    s[:len(p)] == p
    s.find(p) == len(s) - len(p)

    # Лучше
    s.startswith(p)
    s.endswith(p)
    ```
