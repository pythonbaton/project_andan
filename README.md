# project_andan
![Доброе утро, криптоинвесторы]([https://github.com/{username}/{repository}/raw/{branch}/{path}/image.png](https://github.com/pythonbaton/project_andan/blob/main/IMG_5234.WEBP))
Для начала спарсим id токенов, которые были последние добавлены на CoinMarketCap. Лимит на парсинг стоит в 5000, поэтому парсим 5000 айдишников. Код, который использовался для парсинга, находится в файле id_of_tokens.ipynb (сам код находится в id_of_tokens code)

Полученные айдишники сохраняем в текстовый файл. Файл добавлен и называется id_of_tokens_list.txt

Так как по какой то неизвестной причине API не хочет парсить информацию о сразу 5000 токенах, разделим их в группы по 500 токенов и потом объединим эту информацию вместе. Для этого используется код, который в файле group_by_500.ipynb (сам код находится в group_by_500 code)

Получаем группы и распределяем их по файлам, файлы group_1.txt - group_10.txt

Далее получим информацию о наших токенах через один гет запрос, а именно тикер, название, текущую цену. Для этого используем код в файле name_symbol_price code
Результаты возвращаем в файл results_name_price.txt

Далее воспользуемся вторым гет запросом, чтобы получить информацию о самой высокой исторической цене, о самой низкой исторической цене и о начальной цене. Для этого используем код в файле historical_prices code
Результаты возвращаем в файл results_historical_prices.txt в формае id:open_price:all_time_high_price:all_time_low_price

Теперь нам нужно объединить файлы results_historical_prices.txt и results_name_price.txt в один, отсортировать для формата: айди:тикер:название:текущая цена:цена листинга:макс цена:мин цена:рои1:рои2:рои3
Для начала объединяем без роев. Писать для этого код в питоне муторно и бессмысленно, тк это можно сделать вручную. Для этого сначала конвертируем оба файла в .csv, а далее закидываем в общий файлик экселя. После чего сортируем по айди в порядке возрастания и получаем готовую табличку нужного формата.

В файле "Гипотезы" считываем полученный csv и проверяем, все ли в порядке. На этом этапе пришлось вставить 2 костыля, один из которых преобразует объект данных в строку с использованием формата '{:f}', который форматирует числа как числа с плавающей запятой, а второй проверяет, есть ли что - то, что не подходит и где есть ошибки.
После того, как все ошибки найдены (их было до 10 штук, проверяем что необходио исправить и в следующей ячейке за пару строк делаем все данные численными (теперь с ними можно будет работать). 
Далее строим визуализацию рои1, рои2 и рои3 (разумеется, есть пропущенные данные, из - за которых график получается сомнительным, но на нем все равно видно, что присутствуют экстремально высокие значения, а рои1, например, абсолютно плоский. 
После этого проверяем наши 4 гипотезы о гипотетическом портфеле из набора токенов.

Гипотеза 1: заработаем ли, если вкладываем на листинге и продаем на макс
Гипотеза 2: будем ли в плюсе по текущим ценам, если просто держим
Гипотеза 3: когда выгоднее покупать
Гипотеза 4: заработаем ли, если купим по минимальным ценам и продадим по текущим
Здесь у нас обычные математические действия, которые производим на датафрейме, очищенном с помощью dropna()
