<H1>Перед началом работы</H1>
<p>Для того чтобы начать работу с VSF проектами сначала нужно ознакомится с  документацией.</p>
<p>Чтоб повторно протестировать что-либо, нужно сначала очистить весь кэш.</p>
<p>Инкогнито режим возвращает информацию из кэша приложения.</p>
<p>Как правильно чистить кэш описано ниже:</p>
<p>1) Посмотреть код</p>
<p><img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/image%20(1).png"></p>
<p>2) Выбираем application</p>
<p><img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/image%20(2).png"></p>
<p>3) Clear Storage -> Clear Site Data</p>
<p><img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/image%20(3).png"></p>
<p>4) Очистка кеша и жесткая перезагрузка</p>
<p><img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/image%20(4).png"></p>
<br>
<p>Перед тем как заводить баг разработчику убедись что это не баг VSF, а если это так, то проверь <b>issue</b>,
 может он уже исправлен и стоит обновить core.</p>

<p><a href="https://github.com/DivanteLtd/vue-storefront/issues">Classic/Default Theme</a></p>
<p><a href="https://github.com/DivanteLtd/vsf-capybara/issues">Capybara Theme</a></p>
<br>
<p>Важно понимать структуру приложения, и ориентироваться в архитектуре. </p>
<p>Упрощенная схема зависимостей между компонентами</p>
<p><img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/Image(5).png" alt=""></p>
<p>Magento Admin Panel - используется как Backend, и если сбросить только этот кеш, то на фронте не будет изменений. VueStorefront наполняется с Elasticsearch, а Elasticsearch наполняется с М2.</p>

<br>
<p>Список сущностей, которые хранит Elasticsearch из коробки:</p>

<ul>..._attribute</ul>
<ul>..._category</ul>
<ul>..._cms_block</ul>
<ul>..._cms_page</ul>
<ul>..._product</ul>
<ul>..._review</ul>
<ul>..._taxrule</ul>

<br>
<p><b>Убедись что ты тестируешь актуальный стейдж.</b></p>

<br>
<p>Изображения продуктов PWA тянет через API-сервер, который, в свою очередь, обращается к Magento-серверу, загружает изображение к себе, масштабирует его, а затем выдает браузеру клиента. Получается, что каждая картинка продукта опосредствованно подгружается с Magento-сервера. В PWA можно видеть, как изначально вместо изображений используются placeholder'ы, которые потом постепенно замещаются нормальными изображениями продуктов. Таким образом, чтоб увидеть изображение на стороне PWA должно пройти некоторое время.</p>

<h1>Базовая архитектура CMS страниц</h1>
Ниже представлена CMS страница которая состоит из блоков <br>
Блоки виделены красным <br>
<h6>Структура для VSF и М2 практически не отличается</h6>
<img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/documentation_cms_page3_m2.png">
<hr>
Как идентифицировать CMS страниц в М2:
<br>
Открываем <b>Crtl + Shift + I</b> - "Посмотреть код"<br>
<p>Стоит обратить внимамние на тег <i>body</i> <br></p>
<img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/documentation_cms_page2_m2.png">
<br> <p>В нашем случае class:</p>
<i>cms-venta-por-telefono cms-page-view page-layout-1column</i>
<br><br>
<i>cms-venta-por-telefono</i> - Идентификатор CMS страницы(можно найти в админ панели) <br>
<i>cms-page-view</i> - Маркер CMS страницы <br>
<i>page-layout-1column</i> - Тип шаблона <br>
<hr>
<h1>+1 один пример</h1>
<img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/documentation_cms_page_m2.png">
<br> <p>В нашем случае class:</p>
<i>cms-franquicia2 cms-page-view page-layout-1column</i>
<br><br>
<i>cms-franquicia2</i> - Идентификатор CMS страницы(можно найти в админ панели) <br>
<i>cms-page-view</i> - Маркер CMS страницы <br>
<i>page-layout-1column</i> - Тип шаблона <br>
<hr>
<h1>CMS Блоки для М2</h1>
<br>
<img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/documentation_cms_block_m2.png">
<br><br>
<p>Выбрав нужный элемент страницы обратите внимание на его структуру</p>
<p>В имени класса должен быть маркер CMS блока </p>
В нашем случае:<br> tm_subbaner_cms<br>
tm_shipping_cms<br>
tm_latest_product - тоже CMS блок, но без маркера( см. контекст)<br><br>
В блоках могут быть продукты, они добавляются с помощью виджетов
<h1>CMS блоки для VSF</h1>
Чаще всего Блоки имеют маркеры типа <i>cms_content</i>
<img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/documentation_cms_block2_vsf.png.png">
<br>
<br>
<img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/documentation_cms_block_vsf.png">
<h1>CMS страницы для VSF</h1>
<p>Идентификатор для CMS страниц - <i>cms-page</i> </p>
<br>
<img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/documentation_cms_page2_vsf.png.png">
<br>
<p>CMS блок может быть частью CMS страницы, как это показано на картинке выше
</p><br>
<img src="https://raw.githubusercontent.com/anatoliidolia/before_testing/master/documentation_cms_page_vsf.png.png">
<p>Или же внутри CMS страницы может быть функционал, как на рисунке выше</p>



