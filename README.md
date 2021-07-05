Пожалуйста, пришлите ответы на перечисленные вопросы по адресу menshikov@uniweb.ru.


## Что-то пошло не так и вместо `1 2 3` код пишет в консоль `4 4 4`. Какими _способами_ это можно исправить?

```js
for (var i = 1; i <= 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 0);
}
```

ОТВЕТ 1: убрать асинхронность
```js
for (var i = 1; i <= 3; i++) {
    console.log(i);
}
```

ОТВЕТ 2: закешировать i в замыкании
```js
for (var i = 1; i <= 3; i++) {
    f(i);
}

function f(n) {
    setTimeout(() => {
        console.log(n);
    }, 0);
}
```


## Какой вызов сработает раньше? Опишите отличие первого от второго.

```js
Promise.resolve().then(() => console.log('a'));

setTimeout(() => console.log('a'), 0);
```

ОТВЕТ: первый раньше, потому что это микрозадача, а вторая позже, потому что это макрозадача


## Что не так с компонентом?

```js
import debounce from 'lodash.debounce';

const Search = ({ onChange, delay }) => {
    const handleChange = debounce(onChange, delay);
    useEffect(() => handleChange.cancel, []);
    return <input onChange={handleChange} />;
};
```

ОТВЕТ: Вижу при размонтировании отменяется handleChange -> 
Возможно return из useEffect надо сделать вот так
```js
useEffect(() => () => handleChange.cancel, []);
```

## Напишите функцию, преобразующую объект в строку

```js
const qsStringify = (params) => {
    /* ... */
};

console.assert(
    qsStringify({ a: 1, b: '@', c: '%' }) === 'a=1&b=%40&c=%25',
    'qsStringify should convert object to properly encoded querystring'
);
```

ОТВЕТ: 
(неглубокое преобразование)
```js
const qsStringify = (params) => {
    return Object.entries(params).map(arr => {
    	arr[1] = encodeURIComponent(arr[1]);
        return arr.join('=')
    }).join('&');
};
```
