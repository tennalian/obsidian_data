
[Leaflet](http://leafletjs.com) — это опенсорсная библиотека для интерактивных карт. Она весит всего около 38 КБ, но при этом имеет много разных плюшек, которые придутся по вкусу любому разработчику. В данных момент последнее обновление [v1.2.0](http://leafletjs.com/2017/08/08/leaflet-1.2.0.html) было выпущено 8 августа 2017г. Первая версия библиотеки (v1.0) датируется сентябрем 2016г.

**Поддержка**

**_Desktop_**: Chrome, Firefox, Safari 5+, Opera 12+, IE 7–11, Edge

**_Mobile_**: Safari for iOS 7+; Android browser 2.2+, 3.1+, 4+; Chrome for mobile; Firefox for mobile; IE10+ for Win8 devices

### Инициализация

Для работы с картой достаточно подключить библиотеку, стили и задать контейнер:

```html
<div id="map"></div>  
<link rel="stylesheet" href="leaflet.css" />  
<script src="leaflet.js"></script>
```


В то же время инициализация карты не доставит особых проблем:

```js
const map = L.map('map', {  
  center: [56.4444, 56.4444],  
  zoom: 15,  
  ...  
});
```


Для инициализации нужно обязательно задавать зум и центр карты, без них вы получите ошибку. В то же время можно определить дополнительные опции — макс/мин зум, система координат, слои и т.п.

### Тайлы

Эти простые манипуляции проинициализируют вам карту, но она будет обычным серым блоком с возможностью зума. Чтобы задать карте тайлы используется метод _L.tileLayer._ Библиотека позволяет задавать любые тайлы, которые вы найдете или сделаете сами. Менять их в процессе работы также довольно просто.

```js
L.tileLayer(
	'http://{s}.somedomain.com/blabla/{z}/{x}/{y}{r}.png', 
	{  attribution: '&copy; <a href="copyright">Сopyright</a>'}
).addTo(map);
```


![](https://cdn-images-1.medium.com/max/800/1*ULbFEfzQJZq8o8n18kqFJQ.png)

Что касается тайлов Google Maps и Яндекс Карт — вы можете попробовать задавать тайлы так же. Это будет работать, но политика обоих сервисов разрешает использование тайлов **только** через их API.

Для работы с гугл картами можно использовать плагин [GoogleMutant](https://gitlab.com/IvanSanchez/Leaflet.GridLayer.GoogleMutant).

### Маркеры

Маркеры — чуть ли не основное, что нужно в картах.

```js
L.marker([50.0004, 50.0004], options)  
  .addTo(map)  
  .bindPopup('A pretty CSS3 popup.<br> Easily customizable.')  
  .openPopup();
```


На них можно вешать события (click, drag и т.п.), а так же задавать им кастомные иконки.

```js
const customIcon = L.icon({  
  iconUrl: 'icon.png',  
  iconSize: [100, 100],  
  ...  
});
```


При создании иконки методом _L.icon_ с блоке внутри карты создастся тег `<img>`(_svg_ и т.п, смотря что задаете). На него нельзя повесить класс, и такую иконку тяжело использовать в каких-либо интерактивных действиях. Для этого есть альтернатива — `L.divIcon`.

```js
const customIcon = L.divIcon({  
  className: 'leaflet-div-icon',  
  html: template 
  ...  
});
```

При создании иконки таким образом, в блоке карты создается тег `<div>` с заданным классом, а внутри него уже создается иконка, заданная шаблоном, либо картинкой (вы так же можете не задавать ничего, будет только div).

Создание маркера с кастомной иконкой и добавление его на карту выглядят таким образом:

```js
L.marker([50.0004, 50.0004], {icon: customIcon}).addTo(map);
```


Своей кластеризации у Leaflet нет, но можно использовать сторонний плагин [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster).

### Слои

Все внутри карт построено и держится на слоях — маркеры, точки, кластеры, фигуры и т.п. Можно создавать сколько угодно слоев и добавлять/удалять их по мере необходимости.

```js
let markers = [marker1, marker2, marker3, ... ];

let layer = L.layerGroup(markers);
```


Добавлять слои на карту и удалять их с карты можно несколькими способами:

```js
// при инициализации карты

const map = L.map('map', {  
  ...,  
  layers: [layer1, layer2]  
});

// или

layer.addTo(map);  
layer.remove();

// или

map.addLayer(layer);  
map.removeLayer(layer);
```


### Фигуры

![](https://cdn-images-1.medium.com/max/800/1*bSFUDPQuxKMNYPQulUsiCw.png)

Leaflet дает возможность создавать несколько типов фигур — окружности, многоугольники, треугольники, линии (а так же при желании создавать на карте svg или использовать canvas).

```js
const polyline = L.polyline([latlngs], {options});

const polygon = L.polygon([latlngs], {options});

const circle = L.circle(latlng, {radius: 200});
```


В опциях, как по мне, не хватает только одного свойства — обводки линии (_polyline_). И чтобы ее сделать вам придется отрисовать 2 линии — одну сверху, другую под ней.

Для работы с фигурами используются точки вида _LatLng_. Но библиотека позволяет использовать несколько типов точек.

```js
// точки на карте  
L.LatLng(`[a, b]`) // -> {lat: a, lng: b}

//точки на экране  
L.Point({lat: a, lng: b}) //-> [a, b]
```


### Эвенты

События можно вешать как на карту

```js
map.on('eventname', doSomething);  
map.off('eventname', doSomething);  
map.fireEvent('firename', object);
```

так и на любой элемент в документе

```js
L.DomEvent.on(document, 'eventname', doSomething);  
L.DomEvent.off(document, 'eventname', doSomething);  
L.DomEvent.stopPropagation();  
L.DomEvent.preventDefault();
```


В принципе, вы можете обходиться без jQuery, библиотека поддерживает большинство методов

```js
L.DomUtil.addClass(el, 'class');  
L.DomUtil.removeClass(el, 'class');  
L.DomUtil.hasClass(el, 'class');  
L.DomUtil.create(tagName, 'class', parentElement);  
L.DomUtil.remove(el);

L.Browser.ielt9  
L.Browser.touch  
L.Browser.mobile  
...
```


### Анимации

Для создания анимации используется класс `L.PosAnimation`

```js
const fx = new L.PosAnimation();

fx.run(el, [200, 300] <to>, duration, ease); 

fx.on('end', () => console.log(`I’ve finished`));

fx.on('start', () => console.log(`I’ve run`));

fx.on('step', () => console.log(`Step`));

fx.stop();

```



Можно отслеживать все состояния анимации — начало, шаги и конец, но на практике мне пригодилось только окончание. Все эти состояния совершенно не информативны, присылают по сути только начальные время и точку анимации, хотя хотелось бы, например, время и точку текущего шага. Но не свезло.

Так же стоит учитывать, что конечная точка анимации задается точкой на экране, а не на карте. При зуме обновления позиции точки так же не происходит — все это придется высчитывать самому.

У библиотеки есть возможность конвертировать точки с карты на экран и обратно, получать текущую позицию html-элемента на карте и определять дистанцию между любым типом точек

```js
map.latLngToLayerPoint(latlng) // -> Point_

map.layerPointToLatLng(point) // -> LatLng

L.DomUtil.getPosition(el) // -> Point

currentPosition.distanceTo(nextPointPosition) // -> meters
```



Если вам лень писать свою реализацию анимации, можно взять один из готовых плагинов, например [Leaflet.AnimatedMarker](https://github.com/openplans/Leaflet.AnimatedMarker).

### Контролы

_L.Control_ — это базовый класс для реализации управления картой. Что бы это ни значило :)

На самом деле все довольно просто. Контролы — это элементы управления картой (например, один из стандартных контролов — зум).

![](https://cdn-images-1.medium.com/max/800/1*DmF6qlsCdaFGRwBU1uNSAQ.png)

Используя расширение этого класса, вы можете создавать свои контролы.

```js
//create

L.Control.CustomControl = L.Control.extend({  
  options,  
  onAdd: function(){},  
  onRemove: function(){},  
});

L.control.CustomControl = function(options) {  
  return new L.Control.CustomControl(options);  
};

//add to map

map.addControl(L.control.CustomControl({position: 'topleft'}));
```



При инициализации выполняется функция _onAdd_, в которой вы можете создавать элементы и манипулировать DOM и картой.

При добавлении на карту можно выбрать любую из позиций — `'topleft'`, `'topright'`, `'bottomleft'` or `'bottomright'.`

### Плагины

Простой способ расширить функциональность Leaflet — использование сторонних [плагинов](http://leafletjs.com/plugins.html). У библиотеки довольно большое сообщество и есть плагины практически на все случаи жизни.

Если нужного плагина нет — вы можете сделать свой :)

Немного примеров использования библиотеки можно посмотреть [**здесь**](http://goo.gl/EJyzsZ).