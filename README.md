<h1 align="center">🚀 CacheLib</h1>

**CacheLib** - библиотека для реализации кэширования на PHP 7.4+

## Установка

```cmd
php qero.phar i KRypt0nn/CacheLib
```

```php
<?php

require 'qero-packages/autoload.php';
```

[Что такое Qero?](https://github.com/KRypt0nn/Qero)

<p align="center">или</p>

Скачайте репозиторий и подключите главный файл пакета:

```php
<?php

require 'CacheLib/CacheLib.php';
```

## Быстрый старт

Данная библиотека представлена двумя типами объектов: элементы и провайдеры

Элементы - объекты класса `Cache\Item` - являются представлением элементов кэша. Они хранят в себе значения кэша, а так же дополнительную информацию, в том числе TTL - время жизни элемента, предоставляя дополнительные элементы управления. К примеру, метод `available` возвращает статус элемента (устарел он или является активным)

Провайдер - это объект интерфейса `Cache\Provider`. Он предоставляет функционал для работы с элементами кэша - добавление, получение, удаление и т.п. Модульная структура библиотеки позволяет в любой момент реализовать новый провайдер и использовать его

Базовым провайдером является класс `Cache\File\Provider` (File - название провайдера). Данный провайдер представляет собой реализацию кэша через файловую систему

Каждый провайдер реализует следующие методы:

| Название | Аргументы | Тип | Описание |
| - | - | - | - |
| __construct | array $params = [] |  | Создаёт новый провайдер с указанными параметрами |
| get | string $id | Item \| null | Получает элемент кэша с индексом $id |
| set | string $id, $value, array $params = [] | bool | Создаёт новый элемент кэша с индексом $id, значение $value и параметрами $params |
| remove | string $id | bool | Удаляет элемент кэша с индексом $id |
| exists | string $id | bool | Проверяет элемент кэша с индексом $id на существование |
| removeExpired | | int | Удаляет все устаревшие элементы кэша |

## Пример работы

```php
<?php

use Cache\File\Provider as Cache;

$cache = new Cache;

echo 'Удалено устаревших элементов: '. $cache->removeExpired () . PHP_EOL;

if (!$cache->exists ('example'))
    $cache->set ('example', 'Буквально что угодно', [
        'ttl' => 30
    ]);

$item = $cache->get ('example');

echo 'Статус: '. ($item->available () ? 'Активен' : 'Устарел');

```

Автор: [Подвирный Никита](https://vk.com/technomindlp). Специально для [Enfesto Studio Group](https://vk.com/hphp_convertation)