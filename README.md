# 1c-enterprise83-starter

![Скрин 1c-enterprise83-starter](docs/screenshot.png?raw=true "Скрин 1c-enterprise83-starter")

Добавляет в контекстное меню файлового менеджера (Dolphin, Nautilus и др.) для файла `1Cv8.1CD`
пункты запуска 1С:Предприятия:

* **[1C] Конфигуратор**
* **[1C] Предприятие**

Не нужно добавлять каждую базу в список 1С — удобно, когда баз много.

## Требования

* Ubuntu 22.04 и новее (проверено на 26.04) или другой deb-дистрибутив.
* Платформа 1С:Предприятие 8.3.18+ или 8.5 (раскладка `/opt/1cv8`).

Запуск идёт через штатный `/opt/1cv8/common/1cestart`, поэтому версию платформы
выбирает он сам — так же, как при запуске 1С из меню.

## Установка

Скачайте пакет со страницы [Releases](https://github.com/vorohorio/1c-enterprise83-starter/releases) и установите:

```bash
sudo apt install ./1c-enterprise83-starter_*_all.deb
```

## Сборка

Один раз поставить инструменты сборки:

```bash
sudo apt install build-essential debhelper
```

Собрать из корня проекта:

```bash
dpkg-buildpackage -b -us -uc
```

Пакет `1c-enterprise83-starter_<версия>_all.deb` появится в родительском каталоге.
Временные файлы сборки убирает `debian/rules clean`.

Без установки инструментов в систему — в Docker:

```bash
docker run --rm -v "$PWD":/src -w /src -e OWNER="$(id -u):$(id -g)" ubuntu:24.04 sh -c 'apt-get update && apt-get install -y build-essential debhelper && dpkg-buildpackage -b -us -uc && debian/rules clean && mv ../*.deb . && chown -R "$OWNER" .'
```

Здесь пакет окажется в корне проекта.

## Релиз

Пакет собирается GitHub Actions ([release.yml](.github/workflows/release.yml)).
Чтобы выпустить версию, добавьте запись в `debian/changelog` и запушьте тег:

```bash
git tag v2.0
git push origin master v2.0
```

## Структура

```
data/
  applications/  пункты «Открыть с помощью» (все окружения)
  servicemenus/  оба пункта сразу в контекстном меню Dolphin (KDE)
  mime/          тип файла 1Cv8.1CD
debian/    сборка deb-пакета (debhelper)
docs/      скриншот
```

## Лицензия

[MIT](LICENSE)
