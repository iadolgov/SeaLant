# SeaLant

Библиотека с инструментом для поиска утечек памяти в процессе выполнения тестов 
в Chrome или Node.js

## Установка
```
pip install sealant
```


### Инструкция

#### Пример использования

Минимальный набор действий для подключения библиотеки:
```python
from mldlib import memleak
from unittest import TestCase

...

# Предназначено для использование с фреймворком UnitTests и основанных на 
# нем сторонних фреймворках
class TestsCaseExample(TestCase):

...

    # Перед тестом необходимо уже иметь запущенную ноду, библиотека
    # подключается к ней перед запуском метода test_case
    @memleak()
    def test_case(self):
        some_actions()
```
#### Параметры подключения к ноде

В представленном выше сценарии используются настройки подключения по умолчанию:
подключение к ноде по localhost:9222

При необходимости можно указать другие хост/порт ноды или прямую
ccылку websocket
```python
    @memleak(host='not_localhost', port='2229', ws='ws://direct_ws:9222')
    def test_case(self):
        actions()
```
Указанный ws имеет приоритет над host/port

Допустим вариант указания настроек подключения для целого класса:
```python
@memleak(host='not_localhost', port='2229')
class TestsCaseExample(TestCase):

...
    @memleak()
    def test_case(self):
        actions()
```
Можно задать настройки по умолчанию для всех использований библиотеки 
в config.py.


#### Выбор типа анализа утечки

Следующим важным этапом настройки является возможность выбора типа heapfile:
heaptimeline или heapsnapshot. По умолчанию используется heaptimeline как
более быстрый и показавший большую стабильность способ. Но для Node.js
допустим только вариант с heapsnapshot:
```python
    @memleak(timeline=False)
    def test_case(self):
        actions()
```

Найденные утечки сравниваются с объемом, указанным в config.py (leak_size_limit).
Если размер утечки больше заданного допустимого после выполнения 8 пункта, то 
вызывается исключение LeakError.

#### Сохранение отчета и артефактов теста

В config.py можно задать, сохранять ли полученные heapfiles и составлять ли 
отчет в случае нахождения утечки. В этом случае составленный отчет и снятые 
heapfiles пакуются в zip архив и помещаются в заданную в config.py папку
(по умолчанию создается папка leaks в папке с тестом).

## Версионирование

Мы используем [SemVer](http://semver.org/) для версионирования. Доступные версии
указаны в  [tags on this repository](https://github.com/your/project/tags). 

## Authors

Юдахин А.Е
 
e-mail: aejudakhin@gmail.com


Докучаев С.В
    
e-mail: sv.dokuchaev@tensor.ru

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
