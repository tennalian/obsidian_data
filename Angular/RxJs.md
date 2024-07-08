## Observable - холодный / горячий 

- https://habr.com/ru/companies/tinkoff/articles/729808/   
- [https://www.learn-it-with-examples.com/development/html-css-javascript/angular/cold-hot-observables-angular.html](https://www.learn-it-with-examples.com/development/html-css-javascript/angular/cold-hot-observables-angular.html)

Cold Observable — создает data producer для каждого подписчика. Ждет, пока кто-то подпишется прежде чем начинать передавать данные.  

`httpClient` - холодный, поэтому любая подписка на него в шаблоне будет дублировать запросы на сервер, т.к. для каждой из них создается свой data producer  
  
Hot Observable — создает сначала data producer, а потом все подписчики берут данные из него в момент подписки. При этом он передает данные даже если нет ни одной подписки. (multicast)

## shareReplay 

- [https://buhtatyalexander90.medium.com/%D0%BC%D0%B0%D0%B3%D0%B8%D1%8F-sharing-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2-%D0%B8-%D0%B8%D1%85-%D1%80%D0%B0%D0%B7%D0%BD%D0%B8%D1%86%D0%B0-%D0%B2-rxjs-faed2c866c29](https://buhtatyalexander90.medium.com/%D0%BC%D0%B0%D0%B3%D0%B8%D1%8F-sharing-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2-%D0%B8-%D0%B8%D1%85-%D1%80%D0%B0%D0%B7%D0%BD%D0%B8%D1%86%D0%B0-%D0%B2-rxjs-faed2c866c29) 

share(), publish() и другие multicasting-операторы заставляют холодные observable вести себя как горячие  
  
refCount() отвечает за автоматическую подписку/\отписку от data provider  
  
share и publish — пока есть хотя бы один подписчик на источник(Subject), он будет выдавать значения. После того как не останется подписчиков, источник(Subject) будет отключен от исходного источника(Source).  

share:  
- использует функцию-фабрику которая возвращает экземпляр Subject — multicast(() => new Subject()).refCount()  
- Для любого нового подписчика, неважно был ли завершен поток(Source) или нет, он будет подписан к источику(Source) снова используя new Subject  
  
publish:  
- использует экземпляр Subject — multicast(new Subject())  
- в publish - нетпод капотом refCount , поэтому мы должны использовать publish вместе с refCount  
- Если источник(Source) был завершен то все новые подписчики будут получать “completed”, но если источник(Source) не был завершен он будет переподключен к исходному источнику (Source)  
  
  
shareReplay и publishReplay - возвращают экземпляр replaySubject  
  
refCount: true означает что shareReplay() будет использовать механизм подчета ссылок, похожий на refCount()

## Мапперы

- [https://buhtatyalexander90.medium.com/%D0%BC%D0%B0%D0%B3%D0%B8%D1%8F-sharing-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2-%D0%B8-%D0%B8%D1%85-%D1%80%D0%B0%D0%B7%D0%BD%D0%B8%D1%86%D0%B0-%D0%B2-rxjs-faed2c866c29](https://buhtatyalexander90.medium.com/%D0%BC%D0%B0%D0%B3%D0%B8%D1%8F-sharing-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2-%D0%B8-%D0%B8%D1%85-%D1%80%D0%B0%D0%B7%D0%BD%D0%B8%D1%86%D0%B0-%D0%B2-rxjs-faed2c866c29) 
- https://rxmarbles.com/#switchMap 

Такие обзерваблы называются обзерваблами высшего порядка

- Map - преобразует каждый элемент, излучаемый Observable-ом, и эмиттит измененный элемент  
  
- concatMap - обрабатвает стримы по очереди  
  
- mergeMap - получив следующий эмит, подпишется на новый поток (Observable), не отписываясь от предыдущего. Каждый из них выполниться в рандомном порядке. Все запросы идут параллельно, никто никого не ждёт  
  
- switchMap - получая новое значение, подписывается на новый поток (Observable), тут же отписавшись от предыдущего  
  
- exhaustMap - для игнорирования создания новых стримов, если текущий стрим ещё не был завершён;  
  
- combineLatest - олучить последние значения из каждой последовательности при эммите одного из них

## Отличие observable от promises 

Промис:  
- выполняется 1 раз  
- не может быть отменен (без хаков)  
- выполняется сразу, как только он определен (не дожидаясь then), в отличие от обзервабла, который выполняется после подписки

