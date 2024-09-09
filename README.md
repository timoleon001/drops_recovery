# Восстановление простого одностраничного дропа

Восстанавливать дроп можно напрямую сохраняя его с `web.archive.org` через браузер или при помощи стороннего ПО например `wayback-machine-downloader`.

Последовательность действий при восстановлении простого одностраничника:
1. Ознакамливаемся с дропом который предстоит восстановить на сайте вебАрхива `https://web.archive.org/web/*/https://stomorey.com/`, где `*` означает показать архив за все время, а `https://stomorey.com/` сайт который нужно восстановить.
2. Детально ознакамливаемся со снимками сайта по дням перебирая даты снимков когда сайт еще работал, имел актуальную версию и соответствует тематике. Открывать ссылки только синего цвета, зеленым помечены редиректы и там точно не будет

![First image](images/img_01.png?raw=true)

3. Подобрав нужный снимок, перед сохранением, вносим изменения в адресную строку, где указана дата снимка `20171221145549` в конец добавить `id_`.
Это поможет убрать из кода ссылки на `web.archive`.
Полученный урл должен иметь вид `https://web.archive.org/web/20171213174642id_/http://stomorey.com:80/`
Открыть код страницы `(CTRL + U)` выделить все `(CTRL + А)` и скопировать в буфер `(CTRL + С)`
4. Локально создать папку проекта и файл `index.html` куда добавляем скопированные данные с вебАрхива, сохраняем и открываем в браузере.
Для удобства можно создавать локально одноименный домен для проверки полученного результата например используя Open Server для систем Windows
5. Произвести чистку `html` от мусора и стороннего кода такого как яндекс, гугл статистики и друго
Привести html к стандарту HTML5, если код не соответствует

[Пример](index.html)

6. Открыть этот домен или просто файл index.html в браузере и через консоль можно посмотреть все недостающие файлы.

![First image](images/img_02.png?raw=true)

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
[robots.txt](robots.txt) - сайт закрыт на индексирование
[robots-indexed.txt](robots-indexed.txt) - на сайте разрешено интексирование поисковыми робатами, пример для WP
[.htaccess](.htaccess)
[sitemap_index.xml](sitemap_index.xml)
[post-sitemap.xml](post-sitemap.xml)
[images_sitemap.xml](images_sitemap.xml)

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
