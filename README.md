
## Weather App
demo - 
## Реалізовано
1. Інпут з автокомплитом містами.
2. Для запитів використатав axios
3. Виведення інформації про погоду у вигляді картки на поточний день
4. Виведення графіка температури по годинниках на поточний день
використав https://www.chartjs.org/
5. Вкладка "Обране". є можливіть додавати туди картку взяту з пошуку,можна видалити(при видаленні зявляться попап підтвердження), якщо картка є в обраному то вона виділяється на основній сторніці,при натисканні на саму картку вся інформація про неї зявиться на головному екрані з графіком і таблицею,обрані міста зберігаються у Local
Storage.Стоїть обмеження на додавання більше 5 карток(зявляється попап попередження).Не можна додати однаковы мыста в "обране"
6. Додаток Адаптивний
7. *Перемикання відображення "день/на 5 днів" відповідно картка погоди та графік
по днях, взявши за середню температуру зі статистики по годинах за день.
8. *Відображається по дефолту погоду користувача визначивши його місто по IP
використав https://ipinfo.io. токен зійсний 7-днів(початок 16.11.2023)
9. Додаткова функціональність це кількість карток доданих в "обране",якщо карток кількість не показується
10. Реалізовано перемикання відображення погоди “День/Ніч”.

# Запити API: 
   1. Визначення міста -- https://api.openweathermap.org/geo/1.0/direct?q=${this.city}&limit=5&appid=${this.ApiKey}
   2. Отримання погоди по вибраному місту --- https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${this.ApiKey}&units=metric
   3. Отримання міста за ІР -- https://ipinfo.io/json?token=${this.token}\
   4. Погода на день --- https://api.openweathermap.org/data/2.5/weather?q=${this.city.name}&appid=${this.ApiKey}&units=metric
   5. Погода на тиждень --- https://api.openweathermap.org/data/2.5/forecast?q=${this.city.name}&appid=${this.ApiKey}&units=metric
   6. Отримання іконок погоди https://openweathermap.org/img/wn/${iconCode}.png

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
# weather-app
