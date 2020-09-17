## Лабораторна робота № 5. Зчитування даних з WEB.

В цій лабораторній роботі необхідно зчитати WEB сторінку з сайту IMDB.com зі 100 фільмами 2019 року виходу за посиланням «http://www.imdb.com/search/title?count=100&release_date=2019,2019&title_ty pe=feature». Необхідно створити data.frame «movies» з наступними даними: номер фільму (rank_data), назва фільму (title_data), тривалість (runtime_data). Для виконання лабораторної рекомендується використати бібліотеку «rvest». CSS селектори для зчитування необхідних даних: rank_data: «.text-primary», title_data: «.lister-item-header a», runtime_data: «.text-muted .runtime». Для зчитування url використовується функція read_html, для зчитування даних по CSS селектору – html_nodes і для перетворення зчитаних html даних в текст - html_text. Рекомендується перетворити rank_data та runtime_data з тексту в числові значення. При формуванні дата фрейму функцією data.frame рекомендується використати параметр «stringsAsFactors = FALSE».
```r
library(rvest)

htmlPageUrl <- "http://www.imdb.com/search/title?count=100&release_date=2019,2019&title_type=feature"

htmlPage = read_html(htmlPageUrl)

rank_data_selector <- ".text-primary"
title_data_selector <- ".lister-item-header a"
runtime_data_selector <- ".text-muted .runtime"

getDataBySelector <- function(selector) {
  selectedData <- html_text(html_nodes(htmlPage, selector))
  return(selectedData)
}

rank_data <- as.numeric(getDataBySelector(rank_data_selector))
title_data <- getDataBySelector(title_data_selector)
runtime_data <- as.numeric(gsub(" min", "", getDataBySelector(runtime_data_selector)))

movies <- data.frame(rank = rank_data, title = title_data, runtime = runtime_data, stringsAsFactors = FALSE)
movies
```

В результаті виконання лабораторної роботи необхідно відповісти на запитання:
#### 1. Виведіть перші 6 назв фільмів дата фрейму.
```r
head(movies$title, 6)
```
```
[1] "Ножі наголо"                "Джокер"                     "Паразити"                   "Месники: Завершення"       
[5] "Одного разу... в Голлівуді" "Кролик Джоджо"
```

#### 2. Виведіть всі назви фільмів з тривалістю більше 120хв.
```r
movies[movies$runtime > 120, ]$title
```
```
 [1] "Ножі наголо"                                   "Джокер"                                       
 [3] "Паразити"                                      "Месники: Завершення"                          
 [5] "Одного разу... в Голлівуді"                    "Доктор Сон"                                   
 [7] "Аутсайдери"                                    "Сонцестояння"                                 
 [9] "Маленькі жінки"                                "Джуманджі: Наступний рівень"                  
[11] "Неограновані коштовності"                      "Зоряні війни: Епізод 9 - Скайвокер. Сходження"
[13] "Аладдін"                                       "Король"                                       
[15] "Ірландець"                                     "Мідвей"                                       
[17] "Воно 2"                                        "Річард Джуелл"                                
[19] "Капітан Марвел"                                "Людина-павук: Далеко від дому"                
[21] "Аліта: Бойовий ангел"                          "Портрет дівчини у вогні"                      
[23] "Пофарбоване пташеня"                           "Термінатор: Фатум"                            
[25] "До зірок"                                      "Рокетмен"                                     
[27] "6 футів під землею"                            "Джон Вік 3"                                   
[29] "Форсаж: Гоббс та Шоу"                          "Шазам!"                                       
[31] "Сирота Бруклін"                                "Темні води"                                   
[33] "Шлюбна історія"                                "Квін та Слім"                                 
[35] "Приховане життя"                               "El Camino: A Breaking Bad Movie"              
[37] "Абатство Даунтон"                              "Ґодзілла II: Король Монстрів"                 
[39] "Падіння янгола"                                "Щиголь"
```

#### 3. Скільки фільмів мають тривалість менше 100хв.
```r
nrow(movies[which(movies$runtime < 100),])
```
```
[1] 22
```
