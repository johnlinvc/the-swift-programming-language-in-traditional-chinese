> 翻譯：[vclwei](https://github.com/vclwei), [coverxit](https://github.com/coverxit), [NicePiao](https://github.com/NicePiao)
> 校對：[coverxit](https://github.com/coverxit), [stanzhai](https://github.com/stanzhai)

# 控制流程
-----------------

本頁包含內容：

- [For 迴圈](#for_loops)
- [While 迴圈](#while_loops)
- [條件語句](#conditional_statement)
- [控制轉移語句（Control Transfer Statements）](#control_transfer_statements)

Swift提供了類似 C 語言的流程控制結構，包括可以多次執行任務的`for`和`while`迴圈，基於特定條件選擇執行不同程式碼分支的`if`和`switch`語句，還有控制流程程跳轉到其他程式碼的`break`和`continue`語句。

除了 C 語言裡面傳統的 for 條件遞增（`for-condition-increment`）迴圈，Swift 還增加了`for-in`迴圈，用來更簡單地遍歷陣列（array），字典（dictionary），區間（range），字串（string）和其他序列型別。

Swift 的`switch`語句比 C 語言中更加強大。在 C 語言中，如果某個 case 不小心漏寫了`break`，這個 case 就會貫穿（fallthrough）至下一個 case，Swift 無需寫`break`，所以不會發生這種貫穿（fallthrough）的情況。case 還可以匹配更多的型別模式，包括區間匹配（range matching），元組（tuple）和特定型別的描述。`switch`的 case 語句中匹配的值可以是由 case 體內部臨時的常數或者變數決定，也可以由`where`分句描述更複雜的匹配條件。

<a name="for_loops"></a>
## For 迴圈

`for`迴圈用來按照指定的次數多次執行一系列語句。Swift 提供兩種`for`迴圈形式：

* `for-in`用來遍歷一個區間（range），序列（sequence），集合（collection），系列（progression）裡面所有的元素執行一系列語句。
* for條件遞增（`for-condition-increment`）語句，用來重複執行一系列語句直到達成特定條件達成，一般通過在每次迴圈完成後增加計數器的值來實作。

<a name="for_in"></a>
### For-In

你可以使用`for-in`迴圈來遍歷一個集合裡面的所有元素，例如由數字表示的區間、陣列中的元素、字串中的字元。

下面的範例用來輸出乘 5 乘法表前面一部分內容：

```swift
for index in 1...5 {
    println("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```

範例中用來進行遍歷的元素是一組使用閉區間運算子（`...`）表示的從`1`到`5`的數字。`index`被賦值為閉區間中的第一個數字（`1`），然後迴圈中的語句被執行一次。在本例中，這個迴圈只包含一個語句，用來輸出當前`index`值所對應的乘 5 乘法表結果。該語句執行後，`index`的值被更新為閉區間中的第二個數字（`2`），之後`println`方法會再執行一次。整個過程會進行到閉區間結尾為止。

上面的範例中，`index`是一個每次迴圈遍歷開始時被自動賦值的常數。這種情況下，`index`在使用前不需要宣告，只需要將它包含在迴圈的宣告中，就可以對其進行隱式宣告，而無需使用`let`關鍵字宣告。

>注意：  
`index`常數只存在於迴圈的生命周期裡。如果你想在迴圈完成後存取`index`的值，又或者想讓`index`成為一個變數而不是常數，你必須在迴圈之前自己進行宣告。

如果你不需要知道區間內每一項的值，你可以使用底線（`_`）替代變數名來忽略對值的存取：

```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
println("\(base) to the power of \(power) is \(answer)")
// 輸出 "3 to the power of 10 is 59049"
```

這個範例計算 base 這個數的 power 次冪（本例中，是`3`的`10`次冪），從`1`（`3`的`0`次冪）開始做`3`的乘法， 進行`10`次，使用`1`到`10`的閉區間迴圈。這個計算並不需要知道每一次迴圈中計數器具體的值，只需要執行了正確的迴圈次數即可。底線符號`_`（替代迴圈中的變數）能夠忽略具體的值，並且不提供迴圈遍歷時對值的存取。

使用`for-in`遍歷一個陣列所有元素：

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    println("Hello, \(name)!")
}
// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!
```

你也可以通過遍歷一個字典來存取它的鍵值對（key-value pairs）。遍歷字典時，字典的每項元素會以`(key, value)`元組的形式回傳，你可以在`for-in`迴圈中使用顯式的常數名稱來解讀`(key, value)`元組。下面的範例中，字典的鍵（key）解讀為常數`animalName`，字典的值會被解讀為常數`legCount`：

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    println("\(animalName)s have \(legCount) legs")
}
// spiders have 8 legs
// ants have 6 legs
// cats have 4 legs
```

字典元素的遍歷順序和插入順序可能不同，字典的內容在內部是無序的，所以遍歷元素時不能保證順序。關於陣列和字典，詳情參見[集合型別](../chapter2/04_Collection_Types.html)。

除了陣列和字典，你也可以使用`for-in`迴圈來遍歷字串中的字元（`Character`）：

```swift
for character in "Hello" {
    println(character)
}
// H
// e
// l
// l
// o
```

<a name="for_condition_increment"></a>
### For條件遞增（for-condition-increment）

除了`for-in`迴圈，Swift 提供使用條件判斷和遞增方法的標準 C 樣式`for`迴圈:

```swift
for var index = 0; index < 3; ++index {
    println("index is \(index)")
}
// index is 0
// index is 1
// index is 2
```

下面是一般情況下這種迴圈方式的格式：

> for `initialization`; `condition`; `increment` {  
>     `statements`  
> }  

和 C 語言中一樣，分號將迴圈的定義分為 3 個部分，不同的是，Swift 不需要使用圓括號將「initialization; condition; increment」包括起來。

這個迴圈執行流程如下：

1. 迴圈首次啟動時，初始化表達式（_initialization expression_）被呼叫一次，用來初始化迴圈所需的所有常數和變數。
2. 條件表達式（_condition expression_）被呼叫，如果表達式呼叫結果為`false`，迴圈結束，繼續執行`for`迴圈關閉大括號
（`}`）之後的程式碼。如果表達式呼叫結果為`true`，則會執行大括號內部的程式碼（_statements_）。
3. 執行所有語句（_statements_）之後，執行遞增表達式（_increment expression_）。通常會增加或減少計數器的值，或者根據語句（_statements_）輸出來修改某一個初始化的變數。當遞增表達式執行完成後，重複執行第 2 步，條件表達式會再次執行。

上述描述和迴圈格式等同於：

> `initialization`  
> while `condition` {  
>     `statements`  
>     `increment`  
> }  

在初始化表達式中宣告的常數和變數（比如`var index = 0`）只在`for`迴圈的生命周期裡有效。如果想在迴圈結束後存取`index`的值，你必須要在迴圈生命周期開始前宣告`index`。

```swift
var index: Int
for index = 0; index < 3; ++index {
    println("index is \(index)")
}
// index is 0
// index is 1
// index is 2
println("The loop statements were executed \(index) times")
// 輸出 "The loop statements were executed 3 times
```

注意`index`在迴圈結束後最終的值是`3`而不是`2`。最後一次呼叫遞增表達式`++index`會將`index`設置為`3`，從而導致`index < 3`條件為`false`，並終止迴圈。

<a name="while_loops"></a>
## While 迴圈

`while`迴圈執行一系列語句直到條件變成`false`。這類別迴圈適合使用在第一次迭代前迭代次數未知的情況下。Swift 提供兩種`while`迴圈形式：

* `while`迴圈，每次在迴圈開始時計算條件是否符合；
* `do-while`迴圈，每次在迴圈結束時計算條件是否符合。

<a name="while"></a>
###While

`while`迴圈從計算單一條件開始。如果條件為`true`，會重複執行一系列語句，直到條件變為`false`。

下面是一般情況下 `while` 迴圈格式：

> while `condition` {  
>     `statements`  
> }  

下面的範例來玩一個叫做_蛇和梯子（Snakes and Ladders）_的小遊戲，也叫做_滑道和梯子（Chutes and Ladders）_：

![image](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/snakesAndLadders_2x.png)

遊戲的規則如下：

* 遊戲盤面包括 25 個方格，遊戲目標是達到或者超過第 25 個方格；
* 每一輪，你通過擲一個 6 邊的骰子來確定你移動方塊的步數，移動的路線由上圖中橫向的虛線所示；
* 如果在某輪結束，你移動到了梯子的底部，可以順著梯子爬上去；
* 如果在某輪結束，你移動到了蛇的頭部，你會順著蛇的身體滑下去。

遊戲盤面可以使用一個`Int`陣列來表達。陣列的長度由一個`finalSquare`常數儲存，用來初始化陣列和檢測最終勝利條件。遊戲盤面由 26 個 `Int` 0 值初始化，而不是 25 個（由`0`到`25`，一共 26 個）：

```swift
let finalSquare = 25
var board = Int[](count: finalSquare + 1, repeatedValue: 0)
```

一些方塊被設置成有蛇或者梯子的指定值。梯子底部的方塊是一個正值，使你可以向上移動，蛇頭處的方塊是一個負值，會讓你向下移動：

```swift
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
```

3 號方塊是梯子的底部，會讓你向上移動到 11 號方格，我們使用`board[03]`等於`+08`（來表示`11`和`3`之間的差值）。使用一元加運算子（`+i`）是為了和一元減運算子（`-i`）對稱，為了讓盤面程式碼整齊，小於 10 的數字都使用 0 補齊（這些風格上的調整都不是必須的，只是為了讓程式碼看起來更加整潔）。

玩家由左下角編號為 0 的方格開始遊戲。一般來說玩家第一次擲骰子後才會進入遊戲盤面：

```swift
var square = 0
var diceRoll = 0
while square < finalSquare {
    // 擲骰子
    if ++diceRoll == 7 { diceRoll = 1 }
    // 根據點數移動
    square += diceRoll
    if square < board.count {
        // 如果玩家還在棋盤上，順著梯子爬上去或者順著蛇滑下去
        square += board[square]
    }
}
println("Game over!")
```

本例中使用了最簡單的方法來模擬擲骰子。 `diceRoll`的值並不是一個隨機數，而是以`0`為初始值，之後每一次`while`迴圈，`diceRoll`的值使用前綴累加運算子(`++i`)來累加 1 ，然後檢測是否超出了最大值。`++diceRoll`呼叫完成_後_，回傳值等於`diceRoll`累加後的值。任何時候如果`diceRoll`的值等於7時，就超過了骰子的最大值，會被重置為`1`。所以`diceRoll`的取值順序會一直是`1`，`2`，`3`，`4`，`5`，`6`，`1`，`2`。

擲完骰子後，玩家向前移動`diceRoll`個方格，如果玩家移動超過了第 25 個方格，這個時候遊戲結束，相應地，程式碼會在`square`增加`board[square]`的值向前或向後移動（遇到了梯子或者蛇）之前，檢測`square`的值是否小於`board`的`count`屬性。

如果沒有這個檢測（`square < board.count`），`board[square]`可能會越界存取`board`陣列，導致錯誤。例如如果`square`等於`26`， 程式碼會去嘗試存取`board[26]`，超過陣列的長度。

當本輪`while`迴圈執行完畢，會再檢測迴圈條件是否需要再執行一次迴圈。如果玩家移動到或者超過第 25 個方格，迴圈條件結果為`false`，此時遊戲結束。

`while` 迴圈比較適合本例中的這種情況，因為在 `while` 迴圈開始時，我們並不知道遊戲的長度或者迴圈的次數，只有在達成指定條件時迴圈才會結束。


<a name="do_while"></a>
###Do-While

`while`迴圈的另外一種形式是`do-while`，它和`while`的區別是在判斷迴圈條件之前，先執行一次迴圈的程式碼區塊，然後重複迴圈直到條件為`false`。

下面是一般情況下 `do-while`迴圈的格式：

> do {  
>     `statements`  
> } while `condition`  

還是蛇和梯子的遊戲，使用`do-while`迴圈來替代`while`迴圈。`finalSquare`、`board`、`square`和`diceRoll`的值初始化同`while`迴圈一樣：

``` swift
let finalSquare = 25
var board = Int[](count: finalSquare + 1, repeatedValue: 0)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```

`do-while`的迴圈版本，迴圈中_第一步_就需要去檢測是否在梯子或者蛇的方塊上。沒有梯子會讓玩家直接上到第 25 個方格，所以玩家不會通過梯子直接贏得遊戲。這樣在迴圈開始時先檢測是否踩在梯子或者蛇上是安全的。

遊戲開始時，玩家在第 0 個方格上，`board[0]`一直等於 0， 不會有什麼影響：

```swift
do {
    // 順著梯子爬上去或者順著蛇滑下去
    square += board[square]
    // 擲骰子
    if ++diceRoll == 7 { diceRoll = 1 }
    // 根據點數移動
    square += diceRoll
} while square < finalSquare
println("Game over!")
```

檢測完玩家是否踩在梯子或者蛇上之後，開始擲骰子，然後玩家向前移動`diceRoll`個方格，本輪迴圈結束。

迴圈條件（`while square < finalSquare`）和`while`方式相同，但是只會在迴圈結束後進行計算。在這個遊戲中，`do-while`表現得比`while`迴圈更好。`do-while`方式會在條件判斷`square`沒有超出後直接執行`square += board[square]`，這種方式可以去掉`while`版本中的陣列越界判斷。

<a name="conditional_statement"></a>
## 條件語句

根據特定的條件執行特定的程式碼通常是十分有用的，例如：當錯誤發生時，你可能想執行額外的程式碼；或者，當輸入的值太大或太小時，向使用者顯示一條訊息等。要實作這些功能，你就需要使用*條件語句*。

Swift 提供兩種型別的條件語句：`if`語句和`switch`語句。通常，當條件較為簡單且可能的情況很少時，使用`if`語句。而`switch`語句更適用於條件較複雜、可能情況較多且需要用到模式匹配（pattern-matching）的情境。

<a name="if"></a>
### If

`if`語句最簡單的形式就是只包含一個條件，當且僅當該條件為`true`時，才執行相關程式碼：

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    println("It's very cold. Consider wearing a scarf.")
}
// 輸出 "It's very cold. Consider wearing a scarf."
```

上面的範例會判斷溫度是否小於等於 32 華氏度（水的冰點）。如果是，則列印一條訊息；否則，不列印任何訊息，繼續執行`if`塊後面的程式碼。

當然，`if`語句允許二選一，也就是當條件為`false`時，執行 *else 語句*：

```swift
temperatureInFahrenheit = 40
if temperatureInFahrenheit <= 32 {
    println("It's very cold. Consider wearing a scarf.")
} else {
    println("It's not that cold. Wear a t-shirt.")
}
// 輸出 "It's not that cold. Wear a t-shirt."
```

顯然，這兩條分支中總有一條會被執行。由於溫度已升至 40 華氏度，不算太冷，沒必要再圍圍巾——因此，`else`分支就被觸發了。

你可以把多個`if`語句鏈接在一起，像下面這樣：

```swift
temperatureInFahrenheit = 90
if temperatureInFahrenheit <= 32 {
    println("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    println("It's really warm. Don't forget to wear sunscreen.")
} else {
    println("It's not that cold. Wear a t-shirt.")
}
// 輸出 "It's really warm. Don't forget to wear sunscreen."
```

在上面的範例中，額外的`if`語句用於判斷是不是特別熱。而最後的`else`語句被保留了下來，用於列印既不冷也不熱時的訊息。

實際上，最後的`else`語句是可選的：

```swift
temperatureInFahrenheit = 72
if temperatureInFahrenheit <= 32 {
    println("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    println("It's really warm. Don't forget to wear sunscreen.")
}
```

在這個範例中，由於既不冷也不熱，所以不會觸發`if`或`else if`分支，也就不會列印任何訊息。

<a name="switch"></a>
### Switch

`switch`語句會嘗試把某個值與若干個模式（pattern）進行匹配。根據第一個匹配成功的模式，`switch`語句會執行對應的程式碼。當有可能的情況較多時，通常用`switch`語句替換`if`語句。

`switch`語句最簡單的形式就是把某個值與一個或若干個相同型別的值作比較：

> switch `some value to consider` {  
> case `value 1`:  
>     `respond to value 1`  
> case `value 2`,  
> `value 3`:  
>     `respond to value 2 or 3`  
> default:  
>     `otherwise, do something else`  
> }  

`switch`語句都由*多個 case* 構成。為了匹配某些更特定的值，Swift 提供了幾種更複雜的匹配模式，這些模式將在本節的稍後部分提到。

每一個 case 都是程式碼執行的一條分支，這與`if`語句類似。與之不同的是，`switch`語句會決定哪一條分支應該被執行。

`switch`語句必須是_完備的_。這就是說，每一個可能的值都必須至少有一個 case 分支與之對應。在某些不可能涵蓋所有值的情況下，你可以使用預設（`default`）分支滿足該要求，這個預設分支必須在`switch`語句的最後面。

下面的範例使用`switch`語句來匹配一個名為`someCharacter`的小寫字元：

```swift
let someCharacter: Character = "e"
switch someCharacter {
case "a", "e", "i", "o", "u":
    println("\(someCharacter) is a vowel")
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
"n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    println("\(someCharacter) is a consonant")
default:
    println("\(someCharacter) is not a vowel or a consonant")
}
// 輸出 "e is a vowel"
```

在這個範例中，第一個 case 分支用於匹配五個母音，第二個 case 分支用於匹配所有的子音。

由於為其它可能的字元寫 case 分支沒有實際的意義，因此在這個範例中使用了預設分支來處理剩下的既不是母音也不是子音的字元——這就保證了`switch`語句的完備性。

<a name="no_implicit_fallthrough"></a>
#### 不存在隱式的貫穿（No Implicit Fallthrough）

與 C 語言和 Objective-C 中的`switch`語句不同，在 Swift 中，當匹配的 case 分支中的程式碼執行完畢後，程式會終止`switch`語句，而不會繼續執行下一個 case 分支。這也就是說，不需要在 case 分支中顯式地使用`break`語句。這使得`switch`語句更安全、更易用，也避免了因忘記寫`break`語句而產生的錯誤。

> 注意：  
你依然可以在 case 分支中的程式碼執行完畢前跳出，詳情請參考[Switch 語句中的 break](#break_in_a_switch_statement)。

每一個 case 分支都*必須*包含至少一條語句。像下面這樣書寫程式碼是無效的，因為第一個 case 分支是空的：

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a":
case "A":
    println("The letter A")
default:
    println("Not the letter A")
}
// this will report a compile-time error
```

不像 C 語言裡的`switch`語句，在 Swift 中，`switch`語句不會同時匹配`"a"`和`"A"`。相反的，上面的程式碼會引起編譯期錯誤：`case "a": does not contain any executable statements`——這就避免了意外地從一個 case 分支貫穿到另外一個，使得程式碼更安全、也更直觀。

一個 case 也可以包含多個模式，用逗號把它們分開（如果太長了也可以分行寫）：

> switch `some value to consider` {  
> case `value 1`,  
> `value 2`:  
>     `statements`  
> }  

> 注意：  
如果想要貫穿至特定的 case 分支中，請使用`fallthrough`語句，詳情請參考[貫穿（Fallthrough）](#fallthrough)。

<a name="range_matching"></a>
#### 區間匹配（Range Matching）

case 分支的模式也可以是一個值的區間。下面的範例展示了如何使用區間匹配來輸出任意數字對應的自然語言格式：

```swift
let count = 3_000_000_000_000
let countedThings = "stars in the Milky Way"
var naturalCount: String
switch count {
case 0:
    naturalCount = "no"
case 1...3:
    naturalCount = "a few"
case 4...9:
    naturalCount = "several"
case 10...99:
    naturalCount = "tens of"
case 100...999:
    naturalCount = "hundreds of"
case 1000...999_999:
    naturalCount = "thousands of"
default:
    naturalCount = "millions and millions of"
}
println("There are \(naturalCount) \(countedThings).")
// 輸出 "There are millions and millions of stars in the Milky Way."
```

<a name="tuples"></a>
#### 元組（Tuple）

你可以使用元組在同一個`switch`語句中測試多個值。元組中的元素可以是值，也可以是區間。另外，使用底線（`_`）來匹配所有可能的值。

下面的範例展示了如何使用一個`(Int, Int)`型別的元組來分類別下圖中的點(x, y)：

```swift
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    println("(0, 0) is at the origin")
case (_, 0):
    println("(\(somePoint.0), 0) is on the x-axis")
case (0, _):
    println("(0, \(somePoint.1)) is on the y-axis")
case (-2...2, -2...2):
    println("(\(somePoint.0), \(somePoint.1)) is inside the box")
default:
    println("(\(somePoint.0), \(somePoint.1)) is outside of the box")
}
// 輸出 "(1, 1) is inside the box"
```

![image](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/coordinateGraphSimple_2x.png)

在上面的範例中，`switch`語句會判斷某個點是否是原點(0, 0)，是否在紅色的x軸上，是否在黃色y軸上，是否在一個以原點為中心的4x4的矩形裡，或者在這個矩形外面。

不像 C 語言，Swift 允許多個 case 匹配同一個值。實際上，在這個範例中，點(0, 0)可以匹配所有_四個 case_。但是，如果存在多個匹配，那麼只會執行第一個被匹配到的 case 分支。考慮點(0, 0)會首先匹配`case (0, 0)`，因此剩下的能夠匹配(0, 0)的 case 分支都會被忽視掉。


<a name="value_bindings"></a>
#### 值綁定（Value Bindings）

case 分支的模式允許將匹配的值綁定到一個臨時的常數或變數，這些常數或變數在該 case 分支裡就可以被參考了——這種行為被稱為*值綁定*（value binding）。

下面的範例展示了如何在一個`(Int, Int)`型別的元組中使用值綁定來分類別下圖中的點(x, y)：

```swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    println("on the x-axis with an x value of \(x)")
case (0, let y):
    println("on the y-axis with a y value of \(y)")
case let (x, y):
    println("somewhere else at (\(x), \(y))")
}
// 輸出 "on the x-axis with an x value of 2"
```

![image](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/coordinateGraphMedium_2x.png)

在上面的範例中，`switch`語句會判斷某個點是否在紅色的x軸上，是否在黃色y軸上，或者不在坐標軸上。

這三個 case 都宣告了常數`x`和`y`的占位符，用於臨時獲取元組`anotherPoint`的一個或兩個值。第一個 case ——`case (let x, 0)`將匹配一個縱坐標為`0`的點，並把這個點的橫坐標賦給臨時的常數`x`。類似的，第二個 case ——`case (0, let y)`將匹配一個橫坐標為`0`的點，並把這個點的縱坐標賦給臨時的常數`y`。

一旦宣告了這些臨時的常數，它們就可以在其對應的 case 分支裡參考。在這個範例中，它們用於簡化`println`的書寫。

請注意，這個`switch`語句不包含預設分支。這是因為最後一個 case ——`case let(x, y)`宣告了一個可以匹配余下所有值的元組。這使得`switch`語句已經完備了，因此不需要再書寫預設分支。

在上面的範例中，`x`和`y`是常數，這是因為沒有必要在其對應的 case 分支中修改它們的值。然而，它們也可以是變數——程式將會創建臨時變數，並用相應的值初始化它。修改這些變數只會影響其對應的 case 分支。

<a name="where"></a>
#### Where

case 分支的模式可以使用`where`語句來判斷額外的條件。

下面的範例把下圖中的點(x, y)進行了分類別：

```swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    println("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    println("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    println("(\(x), \(y)) is just some arbitrary point")
}
// 輸出 "(1, -1) is on the line x == -y"
```

![image](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/coordinateGraphComplex_2x.png)

在上面的範例中，`switch`語句會判斷某個點是否在綠色的對角線`x == y`上，是否在紫色的對角線`x == -y`上，或者不在對角線上。

這三個 case 都宣告了常數`x`和`y`的占位符，用於臨時獲取元組`yetAnotherPoint`的兩個值。這些常數被用作`where`語句的一部分，從而創建一個動態的過濾器(filter)。當且僅當`where`語句的條件為`true`時，匹配到的 case 分支才會被執行。

就像是值綁定中的範例，由於最後一個 case 分支匹配了余下所有可能的值，`switch`語句就已經完備了，因此不需要再書寫預設分支。

<a name="control_transfer_statements"></a>
## 控制轉移語句（Control Transfer Statements）

控制轉移語句改變你程式碼的執行順序，通過它你可以實作程式碼的跳轉。Swift有四種控制轉移語句。

- continue
- break
- fallthrough
- return

我們將會在下面討論`continue`、`break`和`fallthrough`語句。`return`語句將會在[函式](../chapter2/06_Functions.html)章節討論。

<a name="continue"></a>
### Continue

`continue`語句告訴一個迴圈立刻停止本次迴圈迭代，重新開始下次迴圈迭代。就好像在說「本次迴圈迭代我已經執行完了」，但是並不會離開整個迴圈。

>注意：  
在一個for條件遞增（`for-condition-increment`）迴圈中，在呼叫`continue`語句後，迭代增量仍然會被計算求值。迴圈繼續像往常一樣工作，僅僅只是迴圈中的執行程式碼會被跳過。

下面的範例把一個小寫字串中的母音字母和空格字元移除，生成了一個含義模糊的短句：

```swift
let puzzleInput = "great minds think alike"
var puzzleOutput = ""
for character in puzzleInput {
    switch character {
    case "a", "e", "i", "o", "u", " ":
        continue
    default:
        puzzleOutput += character
    }
}
println(puzzleOutput)
    // 輸出 "grtmndsthnklk"
```

在上面的程式碼中，只要匹配到母音字母或者空格字元，就呼叫`continue`語句，使本次迴圈迭代結束，從新開始下次迴圈迭代。這種行為使`switch`匹配到母音字母和空格字元時不做處理，而不是讓每一個匹配到的字元都被列印。

<a name="break"></a>
### Break

`break`語句會立刻結束整個控制流程的執行。當你想要更早的結束一個`switch`程式碼區塊或者一個迴圈時，你都可以使用`break`語句。

<a name="break_in_a_loop_statement"></a>
#### 迴圈語句中的 break

當在一個迴圈中使用`break`時，會立刻中斷該迴圈的執行，然後跳轉到表示迴圈結束的大括號(`}`)後的第一行程式碼。不會再有本次迴圈迭代的程式碼被執行，也不會再有下次的迴圈迭代產生。

<a name="break_in_a_switch_statement"></a>
#### Switch 語句中的 break

當在一個`switch`程式碼區塊中使用`break`時，會立即中斷該`switch`程式碼區塊的執行，並且跳轉到表示`switch`程式碼區塊結束的大括號(`}`)後的第一行程式碼。

這種特性可以被用來匹配或者忽略一個或多個分支。因為 Swift 的`switch`需要包含所有的分支而且不允許有為空的分支，有時為了使你的意圖更明顯，需要特意匹配或者忽略某個分支。那麼當你想忽略某個分支時，可以在該分支內寫上`break`語句。當那個分支被匹配到時，分支內的`break`語句立即結束`switch`程式碼區塊。

>注意：  
當一個`switch`分支僅僅包含注釋時，會被報編譯時錯誤。注釋不是程式碼語句而且也不能讓`switch`分支達到被忽略的效果。你總是可以使用`break`來忽略某個分支。

下面的範例通過`switch`來判斷一個`Character`值是否代表下面四種語言之一。為了簡潔，多個值被包含在了同一個分支情況中。

```swift
let numberSymbol: Character = "三"  // 簡體中文裡的數字 3
var possibleIntegerValue: Int?
switch numberSymbol {
case "1", "١", "一", "๑":
    possibleIntegerValue = 1
case "2", "٢", "二", "๒":
    possibleIntegerValue = 2
case "3", "٣", "三", "๓":
    possibleIntegerValue = 3
case "4", "٤", "四", "๔":
    possibleIntegerValue = 4
default:
    break
}
if let integerValue = possibleIntegerValue {
    println("The integer value of \(numberSymbol) is \(integerValue).")
} else {
    println("An integer value could not be found for \(numberSymbol).")
}
// 輸出 "The integer value of 三 is 3."
```

這個範例檢查`numberSymbol`是否是拉丁，阿拉伯，中文或者泰語中的`1`到`4`之一。如果被匹配到，該`switch`分支語句給`Int?`型別變數`possibleIntegerValue`設置一個整數值。

當`switch`程式碼區塊執行完後，接下來的程式碼通過使用可選綁定來判斷`possibleIntegerValue`是否曾經被設置過值。因為是可選型別的緣故，`possibleIntegerValue`有一個隱式的初始值`nil`，所以僅僅當`possibleIntegerValue`曾被`switch`程式碼區塊的前四個分支中的某個設置過一個值時，可選的綁定將會被判定為成功。

在上面的範例中，想要把`Character`所有的的可能性都列舉出來是不現實的，所以使用`default`分支來包含所有上面沒有匹配到字元的情況。由於這個`default`分支不需要執行任何動作，所以它只寫了一條`break`語句。一旦落入到`default`分支中後，`break`語句就完成了該分支的所有程式碼操作，程式碼繼續向下，開始執行`if let`語句。

<a name="fallthrough"></a>
### 貫穿（Fallthrough）

Swift 中的`switch`不會從上一個 case 分支落入到下一個 case 分支中。相反，只要第一個匹配到的 case 分支完成了它需要執行的語句，整個`switch`程式碼區塊完成了它的執行。相比之下，C 語言要求你顯示的插入`break`語句到每個`switch`分支的末尾來阻止自動落入到下一個 case 分支中。Swift 的這種避免預設落入到下一個分支中的特性意味著它的`switch` 功能要比 C 語言的更加清晰和可預測，可以避免無意識地執行多個 case 分支從而引發的錯誤。

如果你確實需要 C 風格的貫穿（fallthrough）的特性，你可以在每個需要該特性的 case 分支中使用`fallthrough`關鍵字。下面的範例使用`fallthrough`來創建一個數字的描述語句。

```swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
println(description)
// 輸出 "The number 5 is a prime number, and also an integer."
```

這個範例定義了一個`String`型別的變數`description`並且給它設置了一個初始值。函式使用`switch`邏輯來判斷`integerToDescribe`變數的值。當`integerToDescribe`的值屬於列表中的質數之一時，該函式添加一段文字在`description`後，來表明這個是數字是一個質數。然後它使用`fallthrough`關鍵字來「貫穿」到`default`分支中。`default`分支添加一段額外的文字在`description`的最後，至此`switch`程式碼區塊執行完了。

如果`integerToDescribe`的值不屬於列表中的任何質數，那麼它不會匹配到第一個`switch`分支。而這裡沒有其他特別的分支情況，所以`integerToDescribe`匹配到包含所有的`default`分支中。

當`switch`程式碼區塊執行完後，使用`println`函式列印該數字的描述。在這個範例中，數字`5`被準確的識別為了一個質數。

>注意：  
`fallthrough`關鍵字不會檢查它下一個將會落入執行的 case 中的匹配條件。`fallthrough`簡單地使程式碼執行繼續連接到下一個 case 中的執行程式碼，這和 C 語言標準中的`switch`語句特性是一樣的。

<a name="labeled_statements"></a>
### 帶標簽的語句（Labeled Statements）

在 Swift 中，你可以在迴圈和`switch`程式碼區塊中嵌套迴圈和`switch`程式碼區塊來創造複雜的控制流程結構。然而，迴圈和`switch`程式碼區塊兩者都可以使用`break`語句來提前結束整個方法體。因此，顯示地指明`break`語句想要終止的是哪個迴圈或者`switch`程式碼區塊，會很有用。類似地，如果你有許多嵌套的迴圈，顯示指明`continue`語句想要影響哪一個迴圈也會非常有用。

為了實作這個目的，你可以使用標簽來標記一個迴圈或者`switch`程式碼區塊，當使用`break`或者`continue`時，帶上這個標簽，可以控制該標簽代表物件的中斷或者執行。

產生一個帶標簽的語句是通過在該語句的關鍵詞的同一行前面放置一個標簽，並且該標簽後面還需帶著一個冒號。下面是一個`while`迴圈的語法，同樣的規則適用於所有的迴圈和`switch`程式碼區塊。

> `label name`: while `condition` {  
>     `statements`  
> }  

下面的範例是在一個帶有標簽的`while`迴圈中呼叫`break`和`continue`語句，該迴圈是前面章節中_蛇和梯子_的改編版本。這次，遊戲增加了一條額外的規則：

- 為了獲勝，你必須_剛好_落在第 25 個方塊中。

如果某次擲骰子使你的移動超出第 25 個方塊，你必須重新擲骰子，直到你擲出的骰子數剛好使你能落在第 25 個方塊中。

遊戲的棋盤和之前一樣：

![image](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/snakesAndLadders_2x.png)

值`finalSquare`、`board`、`square`和`diceRoll`的初始化也和之前一樣：

```swift
let finalSquare = 25
var board = Int[](count: finalSquare + 1, repeatedValue: 0)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```

這個版本的遊戲使用`while`迴圈和`switch`方法塊來實作遊戲的邏輯。`while`迴圈有一個標簽名`gameLoop`，來表明它是蛇與梯子的主迴圈。

該`while`迴圈的條件判斷語句是`while square !=finalSquare`，這表明你必須剛好落在方格25中。

```swift
gameLoop: while square != finalSquare {
    if ++diceRoll == 7 { diceRoll = 1 }
    switch square + diceRoll {
    case finalSquare:
        // 到達最後一個方塊，遊戲結束
        break gameLoop
    case let newSquare where newSquare > finalSquare:
        // 超出最後一個方塊，再擲一次骰子
        continue gameLoop
    default:
        // 本次移動有效
        square += diceRoll
        square += board[square]
    }
}
println("Game over!")
```

每次迴圈迭代開始時擲骰子。與之前玩家擲完骰子就立即移動不同，這裡使用了`switch`來考慮每次移動可能產生的結果，從而決定玩家本次是否能夠移動。

- 如果骰子數剛好使玩家移動到最終的方格裡，遊戲結束。`break gameLoop`語句跳轉控制去執行`while`迴圈後的第一行程式碼，遊戲結束。
- 如果骰子數將會使玩家的移動超出最後的方格，那麼這種移動是不合法的，玩家需要重新擲骰子。`continue gameLoop`語句結束本次`while`迴圈的迭代，開始下一次迴圈迭代。
- 在剩余的所有情況中，骰子數產生的都是合法的移動。玩家向前移動骰子數個方格，然後遊戲邏輯再處理玩家當前是否處於蛇頭或者梯子的底部。本次迴圈迭代結束，控制跳轉到`while`迴圈的條件判斷語句處，再決定是否能夠繼續執行下次迴圈迭代。

>注意：  
如果上述的`break`語句沒有使用`gameLoop`標簽，那麼它將會中斷`switch`程式碼區塊而不是`while`迴圈。使用`gameLoop`標簽清晰的表明了`break`想要中斷的是哪個程式碼區塊。
同時請注意，當呼叫`continue gameLoop`去跳轉到下一次迴圈迭代時，這裡使用`gameLoop`標簽並不是嚴格必須的。因為在這個遊戲中，只有一個迴圈，所以`continue`語句會影響到哪個迴圈是沒有歧義的。然而，`continue`語句使用`gameLoop`標簽也是沒有危害的。這樣做符合標簽的使用規則，同時參照旁邊的`break gameLoop`，能夠使遊戲的邏輯更加清晰和易於理解。
