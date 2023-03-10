<!-- =================== РАЗЛИЧНЫЕ РЕНДЕРИНГИ И ГЕНЕРАЦИИ КОНТЕНТА ДЛЯ ВЕБ ПРИЛОЖЕНИЙ =================== -->

1.  CSR - Client side rendering
2.  SSR - Server side rendering
3.  SSG - Static site Generator
4.  ISR - Incremental Static Regeneration

<!-- ================================================================ CSR ================================================================================  -->

                        ----- КРАТКО ----

Рендеринг на стороне клиента (CSR) означает рендеринг страниц непосредственно в браузере с использованием JavaScript.
Вся логика, сбор данных, шаблонирование и маршрутизация обрабатываются на клиенте, а не на сервере.

                        ---- ПОДРОБНО ----

Данный способ известен во всех UI фреймворках и библиотеках как React, Vue, Angular, Svelte

Рендеринг полностью происходит в браузере, также в браузере происходит запрос данных, обработка заданного запроса, обработка ДОМ дерева

При входе на страницу идет запрос на сервер для того чтобы получить некий бандл и HTML документ, HTML документ в это время пустой и заполняется он через ДОМ-дерево

Когда страница отрисована, например когда запрос идет на какой ту хук Unmount, мы запрашиваем данные с сервера (например список пользователей ), мы стучимся в серверный API, далее API запрашивает данные с БД и в виде JSON отправляет данные пользователю и далее этот JSON обрабатывается на стороне Клиента

                       <!-- ======= Преимущества CSR ======= -->

1.  Ресурсы JS допустим CSS они загружаются единожды на первый запрос либо же по мере необходимости, если например реализован Lazy Loading, при этом мы получаем экономию ресурса сервера и далее идет запрос данных который мы можем закэшировать

2.  Приложение мы можем закэшировать на клиенте с помощью Service Worker

                        ----- Service worker -----

    Service Worker — это скрипт, который браузер запускает в фоновом режиме, отдельно от веб-страницы, открывая доступ к функциям, которые не требуют взаимодействия с веб-страницей или с пользователем. Сегодня они уже включают такие функции, как push-уведомления и фоновая синхронизация. В будущем Service Worker могут поддерживать другие вещи, такие как периодическая синхронизация или геозоны.

                            <!-- ====== НЕДОСТАТКИ CSR ===== -->

1.  Из недостатков можно привести пример как много времени может занимать начальная загрузка приложение, что может сказаться негативно на UX и некоторое время Юзер может видеть Loader
2.  Также может сказаться на поисковых роботов, по той же причине не смогу корректно проиндексировать, так как исходный HTML будет пустой

<!-- ================================================================ SSR ================================================================================  -->

Данный способ известен в Next,Nuxt, Angular Universal, Sapper

Особенностью SSR является то что сервер на каждый запрос генерирует HTML на своей стороне и клиент получает заполненную данными страницу
Представим что пользователь также заходит на нашу страницу, приложение делает запрос на сервер и далее сервер стучится на какой то API, либо напрямую в БД
и получает ответ также в виде JSON и уже формирует готовый HTML со всеми данными и со всеми элементами и отдает уже готовую страницу клиенту. Юзер получит готовый HTML со всеми данными и например со списком юзеров и никаких запросов клиенту уже делать не обязтальено так все данные сервер уже подготовил

                    <!-- ========= Особенности SSR ======== -->

Отрисовка данных происходит быстро, размер перессылаемого JS также мала, TTFB возрастает так как формирование страницы на сервер может занять некоторое время особенно если возникает задержка на запрос от сторонних данных

                        ======== Минусы SSR =========

1. Замедляется время перехода между страницами. Да, для первоначального рендера SSR быстрее, чем CSR, но далее при работе с приложением вам приходится отрисовывать данные дважды, один раз на сервере и один раз на клиенте.

2. Более сложная разработка. ...
3. Кэширование. ...
4. Стоимость.

                        -----TTFB-------
                        ++++Кратко++++

    Это время между кликом по ссылке и поступлением первый фрагмент из содержимого, html страницы генерируется заново при каждом запросе то есть сервер на каждый запрос запрашивать актуальные данные из API, формировать HTML страницу которая будет возвращена пользователю

                        ++++Подробно++++

TTFB — это показатель, который выглядит, мягко говоря, непрозрачным. На него так много всего влияет, что у меня возникает такое ощущение, будто мы всё время просто отмахиваемся от его серьёзного анализа. Многие предполагают, что TTFB — это просто время, необходимое серверу на то, чтобы подготовить ответ, но это, на самом деле, лишь малая часть того, что влияет на TTFB.

Первое, на что мне хотелось бы обратить ваше внимание, это то, узнав о чём, люди обычно очень удивляются. Речь идёт о том, что в TTFB входит время, которое запрос от клиента идёт по сети к серверу, и время, которое занимает путь ответа сервера клиенту. Речь идёт о так называемом «времени приёма-передачи» (Round Trip Time, RTT). TTFB — это не просто некое время, потраченное сервером на подготовку ответа. Это ещё и время которое тратится в пути данными, идущими от клиента к серверу и от сервера к клиенту (в составе этих данных, понятно, и находится интересующий нас «первый байт»).

Теперь мы, вооружённые этим знанием, легко можем понять причину того, что при просмотре сайтов с мобильных устройств показатель TTFB часто оказывается просто неприлично большим. Вполне возможно, что раньше вы в подобных ситуациях задавались примерно таким вопросом: «Уверен, сервер не знает о том, что я смотрю сайт с мобильного. Как он тогда увеличивает TTFB?». Причина этого в том, что, как правило, мобильные сетевые соединения — это соединения с высокой задержкой. Если показатель RTT, отражающий время, необходимое данным для прохождение пути от телефона к серверу и обратно, составляет, например, 250 мс, то на соответствующее значение вырастет и TTFB.

Если бы мне хотелось, чтобы читатели этого материала вынесли бы из него лишь одну важнейшую идею, то я сформулировал бы эту идею так: «Сетевые задержки влияют на TTFB».

Что ещё влияет на TTFB? На самом деле — уйма всего. Вот далеко не полный список того, что вносит вклад в формирование этого показателя. Пункты этого списка расположены в произвольном порядке.

Сетевые задержки. Как уже было сказано, в TTFB входит время, которое занимает доставка запроса на сервер и возврат ответа от сервера. Возьмём, например, время, необходимое на сеанс обмена данными между устройством, находящимся в Лондоне, и сервером, находящимся в Нью-Йорке. В идеале, при использовании оптоволоконного соединения, это 28 мс. Но это — если исходить из множества весьма оптимистичных предположений. В реальности стоит ожидать чего-то наподобие 75 мс. Именно поэтому так важно использовать CDN. Даже сейчас, в век интернета, географическая близость некоего бизнеса и его клиентов — это преимущество.
Маршрутизация. Если вы используете CDN (а так и должно быть!), то запрос вашего клиента, скажем, из Лидса, может быть перенаправлен в дата-центр MAN только для того, чтобы в результате выяснилось, что нужный клиенту ресурс отсутствует в соответствующем PoP-кэше. Потом запрос будет перенаправлен к настоящему серверу с вашими материалами для того чтобы всё же передать клиенту то, что ему нужно. Если этот сервер находится, например, в Виргинии, то всё вышеописанное приведёт к серьёзному увеличению TTFB без видимых причин.
Работа с файлами. Даже если сервер просто читает из своей файловой системы статические данные, такие, как изображения или файлы стилей, на выполнение этой операции, всё равно, требуется некоторое время. Это время тоже входит в состав TTFB.
Приоритизация. Протокол HTTP/2 имеет механизмы приоритизации обработки запросов. В результате может оказаться так, что низкоприоритетные запросы могут быть задержаны на сервере, давая дорогу выполнению высокоприоритетных запросов. Если даже не принимать во внимание механизмы приоритизации HTTP/2 и предположить, что всё работает гладко, эти ожидаемые задержки способны внести вклад в TTFB.
Выполнение приложений. Это, на самом деле, вполне очевидно, но мне хотелось бы отметить, что время, необходимое на выполнение серверных приложений, серьёзно влияет на TTFB.
Запросы к базам данных. Если для формирования страницы нужно запросить что-то из базы данных — то время на выполнение подобной операции тоже войдёт в TTFB.
Вызовы API. Если для подготовки страницы нужны данные из неких API (внутренних или внешних), то обращения к этим API отразятся на TTFB.
Серверный рендеринг. Совершенно очевидно то, что серверный рендеринг требует времени, это время несложно оценить, но это не отменяет вклада данной операции в TTFB.
Дешёвый хостинг. Если вы используете дешёвый хостинг, стремясь как можно сильнее сэкономить и жертвуя производительностью, это обычно означает, что сервером, на котором расположен ваш проект, пользуется ещё некоторое количество других проектов. Возможно — немалое количество. В результате тот, кто пользуется дешёвым хостингом, может ожидать падения производительности сервера, что способно повлиять на возможности проекта по обработке запросов. Фактически, речь идёт о том, что мощности серверного «железа» недостаточно для удовлетворения нужд приложения.
DDoS-атаки, высокая нагрузка на проект. Тут мы продолжаем тему, затронутую в предыдущем пункте этого списка. А именно, если нагрузка на сервер растёт, а проект не предусматривает гибкого масштабирования серверных мощностей, это приводит к тому, что оборудование начинает работать на пределе возможностей. И, как результат, происходит падение производительности приложений.
WAF, балансировщики нагрузки. Сервисы, такие, как WAF или балансировщики нагрузки, расположенные перед серверным приложением, увеличивают TTFB.
Некоторые возможности CDN. Использование CDN — это фактор, безусловно, благотворно влияющий на TTFB, но некоторые возможности CDN могут ухудшить этот показатель. Например — это свёртывание запроса, ESI и т.п.
Задержки на «последней миле». Когда мы говорим о компьютере из Лондона, который обращается к серверу, находящемуся в Нью-Йорке, мы обычно чрезмерно упрощаем ситуацию, почти сводя всё к тому, что компьютер и сервер соединены друг с другом напрямую. Но в реальности всё устроено гораздо сложнее. Сигнал между компьютером и сервером идёт через множество посредников. Наш маршрутизатор шлёт его провайдеру; из беспроводной сети он попадает в кабель, проложенный по дну океана… Задержки на «последней миле» включают в себя все те сложности, которые встают на пути передачи данных между конечными устройствами.

TTFB в 0 мс — это несбыточная мечта. Поэтому важно отметить, что в списке нет чего-то такого, что всегда плохо сказывается на TTFB или всегда ухудшает этот показатель. Этот список лучше всего воспринимать как описание структуры TTFB некоего проекта. Моя цель заключается не в том, чтобы критиковать некие технологии, а в том, чтобы показать, как те или иные технологии могут влиять на TTFB. И, если честно, учитывая то, как много всего происходит до того, как клиент получит первый байт ответа от сервера, удивительно уже то, что сайты вообще загружаются.

Снятие покрова тайны с TTFB

Теперь, надеюсь, TTFB выглядит уже не таким уж и таинственным показателем. А если потратить немного времени на реализацию API Server Timing, то можно приступать к измерениям хитрых серверных временных показателей и к отправке их на клиентские системы. Это позволит веб-разработчикам обнаруживать и устранять потенциальные узкие места производительности, которые раньше были скрыты от их взоров.

API Server Timing позволяет разработчикам расширять ответы на запросы с помощью дополнительного HTTP-заголовка Server-Timing. Он содержит сведения о временных показателях, измеренные самим приложением.
