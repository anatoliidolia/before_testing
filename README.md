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

