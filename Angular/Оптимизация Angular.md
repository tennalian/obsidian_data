[https://dev-gang.ru/article/10-hitrostei-dlja-optimizacii-vashego-angular-prilozhenija-8fk5qyino3/](https://dev-gang.ru/article/10-hitrostei-dlja-optimizacii-vashego-angular-prilozhenija-8fk5qyino3/)

1. ChangeDetectionStrategy.OnPush - запрещает запуск CD на компоненте и его дочерних элементах. CD будет запущен на компоненте OnPush, только если входные данные были референциально изменены.  
  
2. Отключение детектора изменений (ChangeDetectorRef)  

``` js 
export abstract class ChangeDetectorRef {  
	abstract markForCheck(): void; - глобальный CD  
	abstract detach(): void;  
	abstract detectChanges(): void; - локальный CD  
	abstract checkNoChanges(): void;  
	abstract reattach(): void;  
}  
```
  
3. Обнаружение локальных изменений - cdr.detectChnges - запускается от компонента до его дочерних элементов, в отличие от глобального CD, который запускается от корневого до дочерних компонентов.  
  
4. Запуск вне Angular - для third parties  
  
5. Используйте чистые Pipe (для кеширования)  
  
6. trackBy для директивы \*ngFor - указывает индексы в итерируемом объекте для отслеживания различия. Это предотвратит постоянное разрушение и воссоздание всего DOM.  
  
7. убираем тяжелые вычисления из шаблонов  
  
8. веб воркеры для тяжелых вычислений  
  
9. ленивая загрузка  
  
10. предзагрузка для ленивых модулей  

```js
RouterModule.forRoot(routes, {  
	preloadingStrategy: PreloadAllModules, // or NoPreloading  
}),  
```
  
11. резолверы или canActivate для предзагрузки данных