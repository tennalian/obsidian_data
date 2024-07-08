[https://angdev.ru/angular/dependency-injection-overview/](https://angdev.ru/angular/dependency-injection-overview/) [https://angdev.ru/archive/angular9/dependency-injection/](https://angdev.ru/archive/angular9/dependency-injection/) 
[https://blog.angular-university.io/angular-dependency-injection/](https://blog.angular-university.io/angular-dependency-injection/) 

**Важно: В 14+ версии мб что-то меняется из-за standalone компонент и исключении zone.js в 16+**

Dependency Injection - широко распространенный паттерн проектирования (сокращенно DI), который позволяет создавать объект, использующий другие объекты. При этом изменения в определении используемых объектов никак не влияют на создаваемый объект.  
  
Ядро Angular имеет свою собственную реализацию паттерна Dependency Injection  
  
Мы выносим зависимости из класса и помещаем их куда-то еще в кодовой базе благодаря декоратору *@Injectable()*. После чего супер просто:  
- заменять имплементацию зависимости для тестирования  
- поддерживать несколько рантайм окружений  
- предоставлять новые\разные версии сервисов каким-то thirs-party, которые используют ваши сервисы в зависимостях и тп  

Эта техника получения зависимостей, не зная как они работают внутри или как они созданы называется dependency injection  
  
Provider - функция-фабрика, просто функция, которую ангуляр вызывает чтобы создать зависимость. Эта функция может создаваться неявно ангуляром (для большинства зависимостей)  

``` js
function coursesServiceProviderFactory(http: HttpClient): CoursesService {  
	return new CoursesService(http);  
} 
```
 
Но как ангуляр поймет, что есть связь между инстансом CoursesService и повайдером? Для этого используются InjectionToken:
```js 
export const COURSES_SERVICE_TOKEN = new InjectionToken<CoursesService>("COURSES_SERVICE_TOKEN");  
```
этот токен будет явно идентефицировать CoursesService в DI системе ангуляра  

InjectionToken - это объект, кторый можно использовать для идентификации уникального набора зависимостей  

Имея provider фабрику и токен мы можем сконфигурировать провайдер в ангуляровском DI:  
```js
providers: [  
	{  
		provide: COURSES_SERVICE_TOKEN,  
		useFactory: coursesServiceProviderFactory,  
		deps: [HttpClient]  
	}  
]  
```

- useFactory - ссылка на proider фабрику, которую мы вызываем для создания зависимости  
- provide - содержит токен, связанный с типом зависимости. Токен поможет ангуляру понять когда функция-фабрика должна быть вызвана или не вызвана  
- deps - массив, сожержащий в себе зависимости, которые нужны useFactory для вызова  

Далее мы должны сказать ангуляру, что он должен использовать этот провайдер для вызова зависимости. Для этого мы используем аннотацию *@Inject()* везде, где CoursesService должен быть заинжекчен:  
```js
constructor( 
	@Inject(COURSES_SERVICE_TOKEN) private coursesService: CoursesService
) { ... }  
```

  
Явное использование декоратора *@Inject* говорит ангуляру, что для вызова этой зависимости мы должны ипользовать специфический провайдер, соединенный с COURSES_SERVICE_TOKEN индект токеном  
  
Но обычно не нужно создавать провайдеры самостоятельно, все это делается под капотом - для каждой зависимости, будь то сервим или компонент создается фабрика и токен


