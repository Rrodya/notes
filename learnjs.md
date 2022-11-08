# LEARN JS

## Array buffer

```
let buffer = new ArrayBuffer(16); // создаётся буфер длиной 16 байт

let view = new Uint32Array(buffer); // интерпретируем содержимое как последовательность 32-битных целых чисел без знака

alert(Uint32Array.BYTES_PER_ELEMENT); // 4 байта на каждое целое число

alert(view.length); // 4, именно столько чисел сейчас хранится в буфере
alert(view.byteLength); // 16, размер содержимого в байтах

// давайте запишем какое-нибудь значение
view[0] = 123456;

// теперь пройдёмся по всем значениям
for(let num of view) {
  alert(num); // 123456, потом 0, 0, 0 (всего 4 значения)
}
```

Что если мы попытаемся записать в типизированный массив значение, которое превышает допустимое для данного массива? Ошибки не будет. Лишние биты просто будут отброшены.

Например, давайте попытаемся записать число 256 в объект типа Uint8Array. В двоичном формате 256 представляется как 100000000 (9 бит), но Uint8Array предоставляет только 8 бит для значений. Это определяет диапазон допустимых значений от 0 до 255.

Если наше число больше, то только 8 младших битов (самые правые) будут записаны, а лишние отбросятся:

```
let uint8array = new Uint8Array(16);

let num = 256;
alert(num.toString(2)); // 100000000 (в двоичном формате)

uint8array[0] = 256;
uint8array[1] = 257;

alert(uint8array[0]); // 0
alert(uint8array[1]); // 1
```

### ArrayBuffer – это корневой объект, ссылка на непрерывную область памяти фиксированной длины.

Чтобы работать с объектами типа ArrayBuffer, нам нужно представление («view»).

- Это может быть типизированный массивTypedArray:
  - Uint8Array, Uint16Array, Uint32Array – для беззнаковых целых по 8, 16 и 32 бита соответственно.
  - Uint8ClampedArray – для 8-битных беззнаковых целых, которые обрезаются по верхней и нижней границе при присвоении.
  - Int8Array, Int16Array, Int32Array – для знаковых целых чисел (могут быть отрицательными).
  - Float32Array, Float64Array – для 32- и 64-битных знаковых чисел с плавающей точкой.
- Или DataView – представление, использующее отдельные методы, чтобы уточнить формат данных при обращении, например, getUint8(offset).

## TextDecoder и TextEncoder

Что если бинарные данные фактически являются строкой? Например, мы получили файл с текстовыми данными.

Встроенный объект TextDecoder позволяет декодировать данные из бинарного буфера в обычную строку.

Для этого прежде всего нам нужно создать сам декодер:

```let decoder = new TextDecoder([label], [options]);```

```
let uint8Array = new Uint8Array([72, 101, 108, 108, 111]);
alert( new TextDecoder().decode(uint8Array) ); // Hello

let uint8Array = new Uint8Array([228, 189, 160, 229, 165, 189]);
alert( new TextDecoder().decode(uint8Array) ); // 你好
```

### TextEncoder
TextEncoder поступает наоборот – кодирует строку в бинарный массив.

Имеет следующий синтаксис:

let encoder = new TextEncoder();
Поддерживается только кодировка «utf-8».

Кодировщик имеет следующие два метода:

- encode(str) – возвращает бинарный массив Uint8Array, содержащий закодированную строку.
- encodeInto(str, destination) – кодирует строку (str) и помещает её в destination, который должен быть экземпляром Uint8Array.
```
let encoder = new TextEncoder();

let uint8Array = encoder.encode("Hello");
alert(uint8Array); // 72,101,108,108,111
```

### Blob to base64

Альтернатива URL.createObjectURL – конвертация Blob-объекта в строку с кодировкой base64.

Эта кодировка представляет двоичные данные в виде строки с безопасными для чтения символами в ASCII-кодах от 0 до 64. И что более важно – мы можем использовать эту кодировку для «data-urls».

data url имеет форму data:[<mediatype>][;base64],<data>. Мы можем использовать такой url где угодно наряду с «обычным» url.

Например, смайлик:

```<img src="data:image/png;base64,R0lGODlhDAAMAKIFAF5LAP/zxAAAANyuAP/gaP///wAAAAAAACH5BAEAAAUALAAAAAAMAAwAAAMlWLPcGjDKFYi9lxKBOaGcF35DhWHamZUW0K4mAbiwWtuf0uxFAgA7">```
  
Браузер декодирует строку и показывает смайлик: 

Для трансформации Blob в base64 мы будем использовать встроенный в браузер объект типа FileReader. Он может читать данные из Blob в множестве форматов. В следующей главе мы рассмотрим это более глубоко.

Вот пример загрузки Blob при помощи base64:
```
let link = document.createElement('a');
link.download = 'hello.txt';

let blob = new Blob(['Hello, world!'], {type: 'text/plain'});

let reader = new FileReader();
reader.readAsDataURL(blob); // конвертирует Blob в base64 и вызывает onload

reader.onload = function() {
  link.href = reader.result; // url с данными
  link.click();
};```
  
  
### File
  
наследуется от Blob
```
<input type="file" onchange="showFile(this)">

<script>
function showFile(input) {
  let file = input.files[0];

  alert(`File name: ${file.name}`); // например, my.png
  alert(`Last modified: ${file.lastModified}`); // например, 1552830408824
}
</script>  
```
  
```
<input type="file" onchange="readFile(this)">

<script>
function readFile(input) {
  let file = input.files[0];

  let reader = new FileReader();

  reader.readAsText(file);

  reader.onload = function() {
    console.log(reader.result);
  };

  reader.onerror = function() {
    console.log(reader.error);
  };

}
</script>```

## Веб компонента
  ```
  class MyElement extends HTMLElement {
  constructor() {
    super();
    // элемент создан
  }

  connectedCallback() {
    // браузер вызывает этот метод при добавлении элемента в документ
    // (может вызываться много раз, если элемент многократно добавляется/удаляется)
  }

  disconnectedCallback() {
    // браузер вызывает этот метод при удалении элемента из документа
    // (может вызываться много раз, если элемент многократно добавляется/удаляется)
  }

  static get observedAttributes() {
    return [/* массив имён атрибутов для отслеживания их изменений */];
  }

  attributeChangedCallback(name, oldValue, newValue) {
    // вызывается при изменении одного из перечисленных выше атрибутов
  }

  adoptedCallback() {
    // вызывается, когда элемент перемещается в новый документ
    // (происходит в document.adoptNode, используется очень редко)
  }

  // у элемента могут быть ещё другие методы и свойства
}```
  
  ``` customElement.difine("el-some", MyElement);
  
  ## Template
  
  ```
  <template id="tmpl">
  <style> p { font-weight: bold; } </style>
  <p id="message"></p>
</template>

<div id="elem">Нажми на меня</div>

<script>
  elem.onclick = function() {
    elem.attachShadow({mode: 'open'});

    elem.shadowRoot.append(tmpl.content.cloneNode(true)); // (*)

    elem.shadowRoot.getElementById('message').innerHTML = "Привет из теней!";
  };
</script>
  ```



