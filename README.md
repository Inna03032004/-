Это задание представляет собой полноценный SPA-проект. Чтобы получить максимальный балл, лучше сразу разбить его на модули и реализовывать по этапам.

## 1. Структура проекта

```text
bookshop/
│
├── src/
│   ├── assets/
│   │   ├── images/
│   │   └── icons/
│   │
│   ├── css/
│   │   ├── style.css
│   │   ├── header.css
│   │   ├── slider.css
│   │   ├── books.css
│   │   └── categories.css
│   │
│   ├── js/
│   │   ├── index.js
│   │   ├── api.js
│   │   ├── slider.js
│   │   ├── books.js
│   │   ├── categories.js
│   │   ├── cart.js
│   │   ├── storage.js
│   │   ├── render.js
│   │   └── utils.js
│   │
│   ├── index.html
│   └── placeholder.jpg
│
├── webpack.config.js
├── package.json
└── .gitignore
```

---

# 2. Основные модули

## api.js

Работает только с Google Books API.

```js
const API_KEY = "YOUR_KEY";

export async function getBooks(category, startIndex = 0) {
    const url =
        `https://www.googleapis.com/books/v1/volumes?q=subject:${category}` +
        `&key=${API_KEY}` +
        `&printType=books` +
        `&startIndex=${startIndex}` +
        `&maxResults=6` +
        `&langRestrict=en`;

    const response = await fetch(url);
    const data = await response.json();

    return data.items;
}
```

---

## categories.js

```js
export const categories = [
    "Architecture",
    "Art",
    "Biography",
    "Business",
    "Crafts",
    "Drama",
    "Fiction",
    "Food",
    "Health",
    "History",
    "Poetry"
];
```

---

## slider.js

Использовать свой слайдер.

Требования:

* автопрокрутка 5 сек
* бесконечный цикл
* точки
* переключение по точкам

---

## cart.js

```js
let cart = JSON.parse(localStorage.getItem("cart")) || [];

export function addBook(id){
    cart.push(id);
    save();
}

export function removeBook(id){
    cart = cart.filter(item => item !== id);
    save();
}

function save(){
    localStorage.setItem("cart", JSON.stringify(cart));
}

export function isInCart(id){
    return cart.includes(id);
}

export function getCount(){
    return cart.length;
}
```

---

## books.js

Отвечает за

* загрузку книг
* render
* Load More

Переменные

```js
let category = "Architecture";
let startIndex = 0;
```

При смене категории

```js
category = newCategory;
startIndex = 0;
container.innerHTML = "";
loadBooks();
```

Кнопка Load More

```js
startIndex += 6;
loadBooks();
```

---

# 3. Карточка книги

Необходимо вывести

✔ Обложку

```js
book.volumeInfo.imageLinks?.thumbnail || placeholder
```

---

✔ Автора

```js
book.volumeInfo.authors?.join(", ")
```

---

✔ Название

```js
book.volumeInfo.title
```

---

✔ Рейтинг

```js
book.volumeInfo.averageRating
book.volumeInfo.ratingsCount
```

рисовать звезды

★★★★★

через цикл.

Если рейтинга нет

не выводить блок.

---

✔ Описание

```js
book.volumeInfo.description
```

CSS

```css
display: -webkit-box;
-webkit-line-clamp: 3;
-webkit-box-orient: vertical;
overflow: hidden;
```

---

✔ Цена

```js
book.saleInfo.listPrice.amount
book.saleInfo.listPrice.currencyCode
```

Если цены нет

не показывать.

---

# 4. Buy Now

Кнопка

```
Buy now
```

после клика

```
In the cart
```

и меняет класс.

```js
if(isInCart(id)){
    removeBook(id);
}else{
    addBook(id);
}
```

После каждого изменения

обновлять бейдж корзины.

---

# 5. Header

Содержит

* логотип
* меню
* пользователя
* поиск
* корзину

Корзина

```
3
```

должна брать количество книг из localStorage.

Шапка

```css
position: sticky;
top: 0;
z-index: 100;
```

---

# 6. Ленивое получение книг

Google API

```
startIndex=0
maxResults=6
```

После кнопки

```
Load more
```

```
startIndex=6
```

еще

```
startIndex=12
```

и т.д.

---

# 7. Webpack

Минимальная конфигурация

```
webpack
webpack-cli
webpack-dev-server
html-webpack-plugin
css-loader
style-loader
mini-css-extract-plugin
clean-webpack-plugin
css-minimizer-webpack-plugin
terser-webpack-plugin
copy-webpack-plugin
```

Структура

```
entry:
src/js/index.js

output:
dist
```

Режим

```
development
```

и

```
production
```

Минификация

```
TerserPlugin
CssMinimizerPlugin
```

---




