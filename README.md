# Восстановление простого одностраничного дропа

Восстанавливать дроп можно напрямую сохраняя его с `web.archive.org` через браузер или при помощи стороннего ПО например `wayback-machine-downloader`.

Последовательность действий при восстановлении простого одностраничника:

1. Ознакамливаемся с дропом который предстоит восстановить на сайте вебАрхива `https://web.archive.org/web/*/https://stomorey.com/`, где `*` означает показать архив за все время, а `https://stomorey.com/` сайт который нужно восстановить.
2. Детально ознакамливаемся со снимками сайта по дням перебирая даты снимков когда сайт еще работал, имел актуальную версию и соответствует тематике. Открывать ссылки только синего цвета, зеленым помечены редиректы и там точно не будет

https://prnt.sc/7dJFXEBb-uJT
![First image](https://github.com/timoleon001/drops_recovery/blob/main/images/img_01.png?raw=true)

3. Подобрав нужный снимок, перед сохранением, вносим изменения в урл
В адресе, где указана дата снимка `20171221145549` в конец добавить `id_`.
Это поможет убрать из кода ссылки на `web.archive`.
Полученный урл должен иметь вид `https://web.archive.org/web/20171213174642id_/http://stomorey.com:80/`
Открыть просмотр кода страницы `(CTRL + U)` выделить все `(CTRL + А)` и скопировать в буфер `(CTRL + С)`
4. Локально создать папку проекта и файл `index.html` куда добавляем скопированные данные с вебАрхива, сохраняем и открываем в браузере.
Для удобства можно создавать локально одноименный домен для проверки полученного результата например используя Open Server для систем Windows
5. Произвести чистку `html` от мусора и стороннего кода такого как яндекс, гугл статистики и друго
Привести html к стандарту HTML5, если код не соответствует

[Пример](https://github.com/timoleon001/drops_recovery/index.html)
```
<!DOCTYPE html>
<html lang="ru-RU">
    <head>
        <meta charset="utf-8" >
        <meta content="width=device-width, initial-scale=1, minimum-scale=1" name="viewport" >
        <title>Сто морей | Туристическая компания</title>
        <meta name="description" content="Сто морей - ваша туристическая компания, ">
        <meta content="index, follow, max-snippet:-1, max-image-preview:large, max-video-preview:-1" name="googlebot" >
        <meta content="index, follow" name="yandex" >
        <meta content="index, follow" name="robots" >
        <link href="https://stomorey.com" hreflang="x-default" rel="alternate" >
        <link href="https://stomorey.com" hreflang="ru" rel="alternate" >
        <link href="https://stomorey.com" rel="canonical" >
        <link rel="icon" href="/assets/img/favicons/favicon.ico" >
        <link rel="stylesheet" href="/assets/plain/css/template.css" type="text/css" >
    </head>
    <body>
        <header id="header">
            <a href="#intro" class="b-header_logo b-header_logo__pic">
                <img src="https://github.com/timoleon001/drops_recovery/blob/main/images/logo.png?raw=true" title="Vaims" alt="Logo company Dark"  height="100" class="m-transition" >
            </a>
            <nav class="b-header_nav">
                <a href="#overview">поиск туров</a>
                <a href="#steps">как купить тур</a>
                <a href="#contacts">Контакты</a>
            </nav>
        </header>
        <section class="b-intro b-intro__background b-section" id="intro">
            <div class="b-intro_container container">
                <h1 style="font-size:38px">ПОИСК ТУРОВ СО СКИДКОЙ 10%</h1>
                <p style="font-size:30px">ЭТИ ТУРЫ ЕСТЬ У ВСЕХ, НО У НАС НА 10% ДЕШЕВЛЕ</p>
            </div>
        </section>
        <script type="text/javascript" src="assets/plain/build/template.min.js"></script>
    </body>
</html>
```

6. Открыть этот домен или просто файл index.html в браузере и через консоль можно посмотреть все недостающие файлы.

https://prnt.sc/hePO09n6hPyj
![First image](https://github.com/timoleon001/drops_recovery/blob/main/images/img_02.png?raw=true)

Открываем по очереди недостающие файлы в новой вкладке браузера и в начало подставляем урл вебАрхива с соответствующей датой.
Например для `http://stomorey.com/assets/plain/css/template.css` получим  `https://web.archive.org/web/20171020024130id_/http://stomorey.com/assets/plain/css/template.css`
Полученные данные сохраняем в проект в соответствующие папки и файлы
Если на сайте вебАрхива нет изображения, то можно подобрать по тематики из поиска гугла и сделать его при помощи скриншота, для того чтобы изображение стало уникальным, напрямую сохранять с поисковика плохая практика.
Если нет js или css, как правило это будут файлы библиотек, таких как jQuery, то можно перезаливать из соответствующей библиотеки или темы WP, если дроп был на WP и найти недостающий файл.
Если получаем большое количество битых статических файлов, для ускорения восстановления можно использовать [wayback-machine-downloader](https://github.com/hartator/wayback-machine-downloader)


### Команды для загрузки через wayback-machine
```
Весь домен с базовыми настройками
wayback_machine_downloader https://site.com

Весь домен за определенную дату
wayback_machine_downloader https://site.com --from 20230205104451

Качаем всю статику когда сайт на WP
wayback_machine_downloader https://site.com/wp-content
или только картинки
wayback_machine_downloader https://site.com/wp-content/uploads
или можно добавить определенную дату
wayback_machine_downloader https://site.com/wp-content --from 20230205104451
```

7. Убедиться, что в коде не осталось внешних ссылок.
Все битые и внешние ссылки заменяем на `#`
У изображений проверяем заполненные атрибуты `alt`, `height` и `width` и желательно, особенно изображение превышающие **100 кб** преобразовать в формат `webp`.
Сылки на изображение делаем относительными, т.е. убрать домен
8. Проверить полученный результат на адаптивность и в случае его отсутствия обязательно адаптировать контент под мобильные устройства.

9. Добавить **robots.txt**, .**htaccess** и **sitemap.xml**

Для удобства открытия сайта на индексирование и проверки рабочего *robots* можно создавать их два варианта **robots.txt** и **robots-indexed.txt**
После положительной проверки и разрешение от менеджера открытие сайта на индексирование просто удалить **robots.txt**, а **robots-indexed.txt** переименовываем в **robots.txt**

Примеры
[robots.txt](https://github.com/timoleon001/drops_recovery/robots.txt) - сайт закрыт на индексирование
[robots-indexed.txt](https://github.com/timoleon001/drops_recovery/robots-indexed.txt) - на сайте разрешено интексирование поисковыми робатами, пример для WP
[.htaccess](https://github.com/timoleon001/drops_recovery/.htaccess)
[sitemap_index.xml](https://github.com/timoleon001/drops_recovery/sitemap_index.xml)
[post-sitemap.xml](https://github.com/timoleon001/drops_recovery/post-sitemap.xml)
[images_sitemap.xml](https://github.com/timoleon001/drops_recovery/images_sitemap.xml)

post-sitemap.xml и images_sitemap.xml удобно создавать при помощи  Screaming Frog

10. Проверяем проект через Screaming Frog и/или SiteAnalyzer
    При проверки обратить внимание на:
    - отсутствие битых ссылок (код ответа 301, 404, 500...)
    - дубли или отсутствие важных тегов - title, description, h1, canonical, X-robots-tag
    - есть один уникальный заголовок H1 и желательно не пересекается с title
    - содержимое нормально отображено после отключения JavaScript
    - есть значок Favicon
    - правильные MIME-types
    - страниц максимально похожа на оригинал с вебархива

, отсутствие или дубли title, description, h1 другие ошибки
11. Архивируем проект, заливаем на хостинг и еще раз проверяем через Screaming Frog + [pagespeed](https://pagespeed.web.dev/) + [httpstatus](https://httpstatus.io/) + [validator](https://validator.w3.org) (не должно быть `Error`, `Warning` и `Info` допускается)
12. После положительных результатов всех тестов сдаем на проверку
