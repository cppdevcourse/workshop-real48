# Практикум 'Real48'

## Задание

Задание описано в файле [`docs/problem.md`](docs/problem.md).
Требования к классу `Real48` изложены в файле
[`docs/requirements.md`](docs/requirements.md).
Полезная информация собрана в файле
[`docs/useful-links.md`](docs/useful-links.md).

## Выполнение

1. Создайте форк репозитория воркшопа.

2. Добавьте @czertyaka в форк контрибьютером.

3. Включите в форке GitHub Actions.

4. Создайте ветку `development`.

5. Внесите изменения: добавьте файл `real48.cpp`, отредактируйте
  [`real48.hpp`](real48.hpp).

6. (Опционально) Соберите проект с тестами и запустите тесты.

7. Запушьте изменения и создайте пулл-реквест в ветку `master`.

8. Убедитесь, что воркфлоу из GtiHub Actions прошел.

## Сборка

Для сборки проекта выполните:

```sh
cmake -B build/
cmake --build build/ -j $(nproc)
```

## Тестирование

### Зависимости

Для тестирования проекта используются следующие зависимости:

* [GoogleTest](https://github.com/google/googletest) &mdash; библиотека/фреймворк
  для написания юнит-тестов для C/C++.

* [libFuzzer](https://llvm.org/docs/LibFuzzer.html) &mdash; часть тулчейна
  LLVM для написания фаззинг-тестов.

GoogleTest можно установить стандартным пакетным менеджером операционной системы.
libFuzzer устанавливается вместе с тулчейном LLVM.
Для его использования достаточно установить Clang как компилятор для C/C++
по-умолчанию.
Если текущий компилятор &mdash; не Clang, то будут собраны только юнит-тесты,
сборка фаззинг-тестов пропускается.

### Сборка и запуск тестов

Для сборки проекта с тестами выполните:

```sh
CC=clang CXX=clang++ cmake -B tests/ -DWITH_TESTS=ON
cmake --build tests/ -j $(nproc)
ctest --test-dir tests/ --output-on-failure
```

Обратите внимание, что последний тест &mdash; фаззинг-тест.
Его выполнение занимает примерно минуту, он не завис.

#### Nix

> [!NOTE]
> Необязательный раздел

Есть возможность установить зависимости выше при помощи пакетного менеджера
[Nix](https://nixos.org/download/).
Преимущество такого подхода в том, что при использовании Nix возможно получить
точно такое же окружение сборки, как и в GitHub Actions.
Для этого установите Nix, включите [Nix Flakes](https://nixos.wiki/wiki/Flakes)
и выполните:

```sh
nix develop
CC=clang CXX=clang++ cmake -B tests/ -DWITH_TESTS=ON
cmake --build tests/ -j $(nproc)
ctest --test-dir tests/ --output-on-failure
```

#### Conan

> [!NOTE]
> Необязательный раздел

Есть возможность установить зависимости выше при помощи пакетного менеджера
[Conan](https://docs.conan.io/2/).
Conan позволяет гибко настраивать разные варианты сборки при помощи профилей,
также он установит за вас все необходимые зависимости в собственный кэш.

Для этого установите Conan (это Python-пакет) и выполните:

```sh
conan install . --build=missing -pr:h conan/profiles/test
cmake --preset conan-release
cmake --build build/Release -j $(nproc)
source build/Release/generators/conanrun.sh
ctest --test-dir build/Release --output-on-failure
source build/Release/generators/deactivate_conanrun.sh
```

### Решение

Решение в зашифрованном виде хранится в директории [`solution`](solution/).
