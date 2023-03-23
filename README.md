# htubjy
&НаСервере 
Процедура ОтправитьЗаказНаСервере() 
 Ответ = ""; 
 Попытка 
  //ЗащищенноеСоединение = Новый ЗащищенноеСоединениеOpenSSL(Неопределено, Новый СертификатыУдостоверяющихЦентровОС); 
  HttpСоединение = Новый HTTPСоединение("10.3.108.50"); 
   
 Исключение 
  Сообщить("Не удалось соединиться с сервером"); 
 КонецПопытки; 
 ИмяБазы = "InfoBase55/"; 
 HTTTP = "hs/"; 
 ИмяФункции ="Test/"; 
 НашШаблон = "v1/Otpravka"; 
 Параметр = "?param="; 
 
 ИмяМетода = "Otpravka"; 
 СтруктураОтвета = Новый Структура; 
 Массив = Новый Массив; 
  
 Запрос = Новый Запрос; 
 Запрос.Текст =  
 "ВЫБРАТЬ 
 | Заказы.Услуга КАК Услуга, 
 | Заказы.Дата КАК Дата, 
 | Заказы.Номер КАК Номер, 
 | Заказы.Комментарий КАК Комментарий, 
 | Заказы.Фотография КАК Фотография 
 |ИЗ 
 | Документ.Заказы КАК Заказы"; 
  
 РезультатЗапроса = Запрос.Выполнить(); 
  
 ВыборкаДетальныеЗаписи = РезультатЗапроса.Выбрать(); 
  
 Пока ВыборкаДетальныеЗаписи.Следующий() Цикл 
   
  СтруктураЗадачи = Новый Структура; 
  СтруктураЗадачи.Вставить("Date", ВыборкаДетальныеЗаписи.Дата); 
  СтруктураЗадачи.Вставить("Serv", ВыборкаДетальныеЗаписи.Услуга); 
  СтруктураЗадачи.Вставить("Number", ВыборкаДетальныеЗаписи.Номер); 
  СтруктураЗадачи.Вставить("Komment", ВыборкаДетальныеЗаписи.Комментарий); 
  //ДвоичныеДанные = Base64Строка(ВыборкаДетальныеЗаписи.Фотография.Получить()); 
  //СтруктураЗадачи.Вставить("photo", ДвоичныеДанные); 
  Массив.Добавить(СтруктураЗадачи); 
 КонецЦикла; 
  
  //ЗаголовокЗапросаHTTP = Новый Соответствие(); 
  //ЗаголовокЗапросаHTTP.Вставить("Content-Type", "application/json; charset=utf-8");  
  СтруктураОтвета.Вставить("Fun", Массив); 
  ЗаписьJS = Новый ЗаписьJSON; 
  ЗаписьJS.УстановитьСтроку(); 
     ЗаписатьJSON(ЗаписьJS,СтруктураОтвета); 
  Ответ = ЗаписьJS.Закрыть();  
  HTTPЗапрос = Новый HTTPЗапрос(""+ИмяБазы+HTTTP+ИмяФункции+НашШаблон+Параметр + Ответ); 
  //HTTPЗапрос = Новый HTTPЗапрос("/v1/Otpravka"); 
  //HTTPЗапрос.Заголовки = ЗаголовокЗапросаHTTP;  
  Попытка 
  Ответ = HttpСоединение.Получить(HttpЗапрос); 
  ОтветНасайте = Ответ.ПолучитьТелоКакСтроку(); 
 Исключение 
  Сообщить("Не удалось получить данные!"); 
 КонецПопытки; 
 Сообщить(ОтветНасайте);
