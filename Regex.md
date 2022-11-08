# Regex

##  шаблоны и флаги

``` regexp = new RegExpt("шаблон", "флаги");```

коротки синтаксис

``` 
regexp = /шаблон/; // без флага
regexpt = /шаблон/gmi; // с флагами gmi

new RegExp - регулярка из динамической сгенерированной строки
```

### Флаги

- ``` i ``` - поиск не зависит от регистра
- ``` g ``` - ищет все сопадения, без него - только первое 
- ``` m ``` - многостроочный режим
- ``` s ``` - включает режим "dotall" при котором точка ``` . ``` может соответствовать символу переводу строки как ``` \n ``` 
- ``` u ``` - включает полную поддержку Юникода. Флаг разрешает корректную обработку суррогатных пар
- ``` y ``` - режим поиска на конкретной позиции в тексте

### Поиск: str.match

``` str.match(regexp) ``` 
Есть три режима работы:
1. Если у рег выражения есть флаг ```g```, то он возвращает массив совпадений
```
let str = "Love, apple, love";

alert( str.match(/love/gi) ); // Love, lova (array);
```

2. Если такого флага нет, то возвращается только первое совпадние в виде массива. С самим элементов и доп. информацией о нём
```
let str = "Love, apple, love";
let res = str.match(/love/i);

alert(res[0]); // Love (first word);
alert(res.length); // 1

alert(res.index); // 0 (index of finded work)
alert(res.input); // Love, apple, lova (all string)
```
3. Если совпадений нет, то, вне зависимости от наличия флага ```g``` возвращает null
```
let matches = "Javascript".match(/HTML/); // = null

if(!matches.length) { // error: null hasn't property length
  alert("Error");
}
```

```
let matches = "Javascript".match(/HTML/) || [];

if(!matches.length) {
  alert("Mateches not founed"); 
}
```

### Замена: str.replace

```str.replace(regexp, replacement)``` заменяет совпадения с ```regexp``` в строке ```str``` на ```replacement```

```
// without flag g
alert("We will, we will".replace(/we/i, "I")); // I will, we will

// witch flag g
alert("We will, we will".replace(/we/ig", "I")); // I will, I will
```

Так же в ```replacement``` можно использовать специальные компинации символов

- $& - вставляет все найденное совпадение
- $` - вставляет часть строки до совпадения
- $' - вставляет часть строки после совпадения
- $n -  если ```n``` это 1-2 значное число, вставляет содержимое n-й скобочной группы регулярного выражения
- $<name> - вставляет содержимое скобочной группы с именем ```name```
- $$ - вставляет символ "$" 

```alert ("Love html".replace(/HTML/, "$& and Javascript)); // "Love html and Javascript"```





