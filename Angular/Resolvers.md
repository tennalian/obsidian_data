[https://indepth.dev/tutorials/angular/indepth-guide-how-to-implement-resolver-in-angular](https://stackoverflow.com/questions/35655361/angular2-how-to-load-data-before-rendering-the-component)

Resolver - просто миддлваря, которая загружает данные перед тем как произойдет навигация. Мы можем задать множество резолверов на один роут  

**Порядок выполнения:** Сначала гуарды, потом резолверы
  
**Пример: **  
1. определяем резолвер  
```js
@Injectable({  
	providedIn: 'root',  
})  
export class ContactsResolver implements Resolve<UserContacts[]> {  
	constructor() {} 

	resolve(route: ActivatedRouteSnapshot): Observable<UserContacts[]> {  
		// We just mock user information in the resolver used a delay operator  
		// to simulate asynchronous HTTP requests from the service.  
		return of(userContactsMock).pipe(delay(1000));  
	}  
}
```
  
2. добавляем в роут  
```js
const routes: Routes = [  
	{  
		path: 'user/:id',  
		component: ContactsComponent,  
		resolve: {  
			contacts: ContactsResolver,  
			account: AccountResolver  
			// any other resolver  
		}  
	}  
];  

```
  
3. берем данные в компоненте  
```js 
export class ContactsComponent implements OnInit {  
	public contacts: UserContacts[] = [];  
  
	constructor(private route: ActivatedRoute) {}  
  
	ngOnInit(): void {  
		this.contacts = this.route.snapshot.data['contacts'];  
	}  
}  
```

