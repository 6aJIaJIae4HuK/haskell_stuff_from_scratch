### 1.2. Установка

Основные способы установки указаны [здесь](https://www.haskell.org/ghcup/install). Можно добавить, что для GUI-операционок можно предварительно установить Visual Studio Code (не путать c Visual Studio) и воспользоваться расширениями Haskell и Haskell Run.

Ожидается, что после этого у вас доступна команда ghci, которая запускает интерактивную среду Haskell.

Её можно использовать для построчного выполнения кода. Введите в командную строку `ghci`. Ожидается, что вы увидите что-то в духе:

```shell
GHCi, version 9.10.3: https://www.haskell.org/ghc/  :? for help
ghci> 
```

Введите `2+2` и нажмите `Enter`. В результате выведется `4`.

Проверим стандартные функции. Введите `even 6`. В результате получится `True` (поскольку 6 это чётное число). Так же можно убедиться, что `even 1` выдаёт `False`.

Проверим функцию `isLengthEven` из предыдущего раздела. Введите `isLengthEven s = even (length s)` и нажмите `Enter`. Ожидается, что команда выполнится без ошибок и вообще любого вывода. После этого среда будет знать о наличии функции `isLengthEven`. Соответственно, её можно будет вызывать. Обращу внимание, что после перезапуска `ghci` все знания о функциях будут удалены.

Убедитесь, что `isLengthEven ""` выведет `True`. Можете проверить и то, что `incorrectFunction s = (isLengthEven s) + 2` не получится выполнить.

```shell
ghci> incorrectFunction s = (isLengthEven s) + 2 
<interactive>:23:40: error: [GHC-39999]
    * Could not deduce `Num Bool' arising from a use of `+'
      from the context: Foldable t
        bound by the inferred type of
                   incorrectFunction :: Foldable t => t a -> Bool
        at <interactive>:23:1-42
    * In the expression: (isLengthEven s) + 2
      In an equation for `incorrectFunction':
          incorrectFunction s = (isLengthEven s) + 2
```

Как читать эти ошибки, расскажу дальше по документу.

Проверим работу функции sumFromOneTo в ghci:

```shell
ghci> sumFromOneTo 0 = 0
ghci> sumFromOneTo i = sumFromOneTo (i - 1) + i
ghci> sumFromOneTo 5
*** Exception: stack overflow
```

Видим, что при вводе `sumFromOneTo 5` получаем ошибку через несколько секунд. Проблема тут в том, что первая команда определила функцию `sumFromOneTo 0 = 0`. А следующая строка не дополняет (как в коде), а переопределяет функцию. Как следствие есть просто функция `sumFromOneTo i = sumFromOneTo (i - 1) + i`.

При вводе `sumFromOneTo 5` происходит расчёт `sumFromOneTo 4`, `sumFromOneTo 3`, ..., `sumFromOneTo 0`. Т.к. `sumFromOneTo 0 = sumFromOneTo (-1) + 0` (нету определения `sumFromOneTo 0 = 0`), то на самом деле там бесконечная рекурсия.

То есть, в ghci функции надо определять за одну команду. На самом деле любая команда в ghci это или вычисление какого-то выражения, или определение нового объекта.

От ошибки можно избавиться использованием многострочных команд.

```shell
ghci> :{
ghci| <line1>
ghci| <line2>
...
ghci> :}
```

Строки объединяются в единую команду, которую можно выполнить. Можно проверить:

```shell
ghci> :{
ghci| sumFromOneTo 0 = 0
ghci| sumFromOneTo i = sumFromOneTo (i - 1) + i
ghci| :}
ghci> sumFromOneTo 5
15
```
