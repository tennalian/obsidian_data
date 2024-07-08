###  Ivy

Новый компилятор и рантайм (с v9)  

**Преимущества:** 

- Smaller bundle sizes  
- Faster testing  
- Better debugging  
- Improved CSS class and style binding  
- Improved type checking  
- Improved build errors  
- Improved build times, enabling AOT on by default  
- Improved Internationalization

- https://angdev.ru/archive/angular9/angular-ivy/ 
- [https://blog.angular.io/version-9-of-angular-now-available-project-ivy-has-arrived-23c97b63cfa3](https://blog.angular.io/version-9-of-angular-now-available-project-ivy-has-arrived-23c97b63cfa3)  



### aot 

Ahead-of-time (AOT) compilation  
В отличие от JIT компилирует до того, как код попадет в браузер, что ускоряет загрузку приложения  
По дефолту с v.9
[https://angdev.ru/angular/aot-compiler/](https://angdev.ru/angular/aot-compiler/) 

### Почему плохо «провайдить» сервис из shared-модуля в lazy-loaded модуль? (Вопрос о scope модулей.)

получится два независимых инстанса сервиса - один в ветке di основного приложения, второй в ветке lazy

[https://angular-training-guide.rangle.io/modules/shared-modules-di](https://angular-training-guide.rangle.io/modules/shared-modules-di) 

### Как проводится конфигурация NgZone-модуля? Когда это необходимо?

- https://www.youtube.com/watch?v=KVeX7oKQxlQ   
- [https://nuancesprog.ru/p/5076/](https://nuancesprog.ru/p/5076/) 


### APP_INITIALIZER 

Токен, который выполняется при инициализации приложения. Если внутри возвращается промис или обзервабл, инициализация не закончатся. пока они не выполнятся

[https://angular.io/api/core/APP_INITIALIZER](https://angular.io/api/core/APP_INITIALIZER)

