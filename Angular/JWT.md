- [https://www.geeksforgeeks.org/json-web-token-jwt/](https://www.geeksforgeeks.org/json-web-token-jwt/) 
- [https://blog.angular-university.io/angular-jwt-authentication/](https://blog.angular-university.io/angular-jwt-authentication/) 


**JSON web token(JWT)** - JSON объект, кот используется для секьюрной передачи информации. Токен в основном состоит из header, payload и signature. Эти три части разделены точками  
`[header].[payload].[signature] (сереализованный)` 
  
Бывает двух форматов - Serialized, Deserialized.  
*Сериализованный* подход в основном используется для передачи данных по сети с каждым запросом и ответом. В то время как десериализованный подход используется для чтения и записи данных в веб-токен.  
*Десериализованный* JWT состоит только из header и payload, оба из которых обычный JSON объекты  
  
- header - используется для описания криптографических операций, применимых к JWT. Так же содержит данные о media/content type информации, которую мы передаем  
- payload - содержит все нужные нам данные, их может быть любое количество, здесь нет ограничений, как в хедере. Если JWT используется для авторизации, он содержит как минимум user ID и expiration timestamp.  
- signature - (подпись) используется для верификации токена.  
  
Как это примерно собирается в токен (на бекенде):  
`HASHINGALGO( base64UrlEncode(header) + “.” + base64UrlEncode(payload),signature)`  
  
Как выглядит:  `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIzNTM0NTQzNTQzNTQzNTM0NTMiLCJleHAiOjE1MDQ2OTkyNTZ9.zG-2FvGegujxoLWwIQfNB5IT46D-xC4e8dEDYwi6aRM`
  
  
#### Как работает:  
1. фронтенд отправляет запрос на логин бекенду с данными юзера  
2. если данные юзаре ОК, бекенд создает на своей стороне JWT и отправляет обратно клиенту. Возможно несколько вариантов:  
	-  кукой. Браузер добавляет куки в хедер запросов автоматически и нам больше ничего не придется делать для авторизации на клиенте только если у нас не кросс-доменность + куки мб секьюрны НО подвержены Cross-Site Request Forgery, also known as XSRF or CSRF (когда ты переходишь по ссылке и твои куки утекают и кто-то может ими воспользоваться для авторизации)  
	-  http response body. Нужно будет как-то хранить jwt без куки (зато больше нет XSRF, но это все еще риски). В этом варианте лучше expiration timestamp присылать отдельно, чтобы использовать на клиенте  
3. сохраняем и используем jwt на фронтенде, например в Local Storage (помним про expiration timestamp). И используем его для отправки каждого запроса (можно использовать интерсептор) в Authorization хедере  
4. на бекенде валидируем Authorization хедер в каждом запросе, если все ок - возвращаем данные, если нет - 401


