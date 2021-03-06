
# Gulp workflow
Данная сборка предназначена для сборки статики в многостраничном файле

Используются следующие технологии
* [Pug](http://learn.aleksbasko.com/pages/pug/pug-syntax.html) Шаблонизатор html
* [Scss](http://learn.aleksbasko.com/pages/css/scss-guide.html) Препроцессор css
* [JS](http://learn.aleksbasko.com/) JavaScript
* [SVG](http://learn.aleksbasko.com/) Изображения формата SVG


## Установка
Для успешной работы данной сборки, необходимо что бы был установлен npm и node.js
В корневой директории проекта есть файл [package.json](http://learn.aleksbasko.com/pages/css/scss-guide.html)
Это манифест проекта в котором указанны все зависисомти для разработки и необходимые библиотеки.
Зависимости для разработки и работы gulp модулей находятся в "devDependencies",
импортируемые библиотеки для работы проекта находятся в "dependencies", их необходимо подключить к проекту.

Установить все зависимости можно через введя команду в терминал `npm i` (убедитесь что находитесь в корневой директории проекта)

Так же можно добавлять новые зависимости, по одной или сразу несколько `npm i gulp gulp-sass ...`

Затем просто запустить команду gulp

В данной сборке предусмотренно две версии development и production
За переключениме версий отвечает переменная в gulpfile isProduction.

По умаолчанию она false, и запускается версия develop.
В версиб так же необходимо указать tinifyKey, это ключь к ресурсу по оптимизации изображений.



Объект *path* 
Содержит пути к исзходным данным и пути куда помещаются отработанные данные
например: `path.css.cssSrc` и `path.css.cssDist`

Во всех тасках используем:
 * plumber, notify, для вывода сообщений об ошибках.
 
### Serve
 * browserSync для запуска локального сервера,
  данную сборку можно однавременно запускать в нескольких проектах, локальный сервер будет для всех разный `localhost:3000, lockalhost:3002 ...`
 * вызываетс я вдругих тасках для загрузки изменений без перезагрузки страницы

### CSS
Используем:
 * компиляция scss в css
 * sourcemap на исходные файлы scss,
 * autoprefixer для пяти последних версий браузеров,
 * преобразование и форматирование полученного css кода
 
В версии production добавляется:
 * csso оптимизируем css объеденяя одинаковые стили,
 * минимизируем код, удаляя пробелы и комментарии, cleanCSS
 
### Pug
Используем:
 * компиляция pug в html
 
В версии production добавляется:
 * сжимаем полученный html
 
### JS 
 * Файлы будут объединены в порядке указанном в gulp.src(), в нашем случае в `path.js.jsSrc`
В версии production добавляется:
 * сжимаем объедененный js

### Fonts
Используем:
 * копируем все шрифты в dist, с сохранением иерархии папок,
 * настроенно кеширование, которое отправляет в dist только новые или измененные файлы

### Image
Используем:
 * копируем все шрифты в dist, с сохранением иерархии папок,
 * настроенно кеширование, которое отправляет в dist только новые или измененные файлы
 
В версии production добавляется:
 * оптимизируем изображения через tinify, необходимо зарегестироваться на ресурсе [Tinypng](https://tinypng.com/) и указать в переменной tinifyKey ключь 

### Clean
 * удаляет папку dist (для удаления папок и файлов за пределами текущей директории необхадимо указать {force: true})
 
### Watch
 * Следит за изменениями в указаных директориях и запускает соответствующие таски

# Структура проекта

```
Src
|-- index.pug
|-- templates
|-- pages
|   |-- first
|   |   |-- first.pug
|   |   `-- first.scss
|   `-- second
|       |-- second.pug
|       `-- second.scss
|-- style
|-- js
|   `-- part
|       |-- header.js
|       `-- sidebar.js
|-- libs
|   |-- slider
|   |   |-- slider.scss
|   |   `-- slider.js
|-- image
|   |-- content
|   |   |-- foto_1.jpg
|   |   `-- foto_2.png
|   `-- sprites
|       |-- toSprite
|       |   |-- fb.png
|       |   |-- vk.png
|       `-- toSpriteSVG
|           |-- close.svg
|           `-- user.svg
`-- fonts
    `-- someFonts
        |-- someFonts.ttf
        `-- someFonts.woff
```
Src директория со всеми исходными файлами в ней:
* index.pug это навигационная страница по проекту, с сылками на все страницы
* pages содержит отдельные pug страницы файл стилей для этой страницы,
* fonts содержит все шрифты
* libs все подключаемые библиотеки с их js и css файлами
* image содержит все исходные картинки; картинки для спрайтов находятся в папках toSprite и toSpriteSVG
* js содержит все js файлы
* templates содержит:
    * шаблоны для страниц в папке layout,
    * pug компоненты в папке components, вместе со стилями данного компонента
    иногда компоненты вставляются на станицу не как готовый pug компонент, а в виде миксина, но все ровно они имеют свои отдельные стили, 
    по этому выносим их как отдельный компонент
    * файл mixin.pug содержит подключения всех миксинов из компонентов, 
    и уже этот файл мы подключаем в шаблон layout.pug и будем иметь возможность обратится к любому миксину с любой страницы

```
templates
|-- layout
|   |-- main-layout.pug
|   `-- second-layout.pug
|-- components
|   |-- header
|   |   |-- header.pug
|   |   `-- header.scss
|   `-- footer
|       |-- footer.pug
|       `-- footer.scss
|-- mixin.pug
```

* styles содержит:
 * В файл main.scss мы импортируем все остальные scss файлы, из всех компонентов, библиотек и страниц:

```scss
/* --- global style --- */
@import "part/global/normalize";
@import "part/global/fonts";
/* ------ */

/* --- components style --- */
@import "../templates/components/header/header";
@import "../templates/components/sideMenu/side-menu";;
/* ------ */
```
 * В директории part находятся scss файлы поделенные по смыслу, в index.scss стили для сервисной страницы index.pug.
 * Поскольку все стили страниц находятся в папке страницы, но могут быть общие стили которые не относятся к компонентам или элементам, то их мы выносим в страницу style.scss
 * В файле media.scss медиа запросы.
 * В дирректории global файлы для шрифтов, цветов, scss миксины, переменные, в constants.scss вспомогателные сервесные классы, так же здесь помещается сетка если она используется, обычно это foundation или своя кастомная в grid.scss, если используется foundation? normalize подключать ненадо.
 * В дирректории elements все стили для элементов: блоки, шрифты, ссылки, кнопки.
    
    
```
style
|-- main.scss
`-- part
    |-- style.scss
    |-- media.scss
    |-- index.scss
    |-- global
    |   |-- colors.scss
    |   |-- constants.scss
    |   |-- fonts.scss
    |   |-- mixins.scss
    |   |-- variables.scss
    |   |-- grid.scss
    |   `-- normalize.scss
    `-- elements
        |-- blocks.scss
        |-- buttons.scss
        |-- calendar.scss
        |-- links.scss
        |-- lists.scss
        |-- select.scss
        `-- text.scss
```
