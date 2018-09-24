---
title: "Program Structure"
excerpt: "目前我們編寫的 C 程式碼，都是 sequential 的，也就是按照順序，一次執行一個指令。"
permalink: /c-language/program-structure/
last_modified_at: 2018-09-24 23:41:00
tags: beginner
---

## C Programs are Sequential in Nature

目前我們編寫的 C 程式碼，都是 sequential 的，也就是按照順序，一次執行一個指令。

可以把它當作是一行一行執行的，通常都是由上往下，由左往右，所以不會有兩段程式碼同時執行。

那為什麼我們程式一跑，結果幾乎一瞬間就出來了呢？那是因為電腦跑很快，所以可以把一堆指令按照順序很快全部執行完畢。電腦 LAG 就是因為指令太多，來不及在時間內執行完畢造成的。

## Example

```c
#include <stdio.h>

int main(void) {
    int a, b, sum;
    // Input 2 integers: a, b.
    scanf("%d%d", &a, &b);
    sum = a + b;
    // Output the sum of the 2 integers.
    printf("%d+%d=%d\n", a, b, sum);
    return 0;
}
// Sample Input:
// 1 2
// Output:
// 1+2=3
//
```

在編譯階段，compiler 大致上會從上往下，從左往右掃。

### Preprocessor Commands

以 `#` 為開頭的程式碼，大部分都是預處理器的指令 (preprocessor command), 也就是說它們會被 preprocessor 看到，並預先做處理，所以它們跟下面 C 的那些程式碼長得很不一樣。

以 `#include <stdio.h>` 為例，它會叫 preprocessor 去找到 `stdio.h` 這個檔案 (這個檔案通常在安裝 IDE 時，就會順便下載下來)，然後把這一整行用 `stdio.h` 裡面的內容替換掉，而 `stdio.h` 顧名思義就是 Standard Input Output Header, 所以它裡面就有定義像是 `printf`, `scanf` 這些函式，讓我們方便使用。

C 有提供很多好用的函式，要用的時候就 include 相對應的 header files, 想知道完整列表的人可以參考 [C Standard Library header files](https://en.cppreference.com/w/c/header).

### Line by Line Explanation

以最上面的 code 為例，它的執行順序及功能如下：

1. `int main(void) {`

   `main` function 被系統呼叫。

   我們寫的程式，因為是按照順序執行的，所以一定會有一個起點，以 C 來講，他會從 `main` 這個 function 開始執行。

   在這邊 `int` 代表回傳值的型別，`void` 代表沒有傳入的值，`void` 省略不寫也可以，雖然會稍稍有[不同的含意](https://stackoverflow.com/q/51032/)。

2. `int a, b, sum;`

   宣告三個整數變數，分別叫 `a`, `b` 和 `sum`.

3. `// Input 2 integers: a, b.`

   這是註解，是給人看的，並不會影響程式執行，通常描述等一下會發生什麼事，也就是他下面那行在幹嘛。

4. `scanf("%d%d", &a, &b);`

   呼叫定義在 Standard I/O Header 裡面的 `scanf`, 顧名思義就是 scan formatted, 就是把你輸入的東西，按照特定的格式 (format) 掃描 (scan) 進你指定的變數裡。

   以這邊來講，`%d` 就是一種 format, 讓 `scanf` 知道你想要使用者輸入數字，而左邊的 `%d` 對應到 `a`, 右邊的 `%d` 對應到 `b`, 所以先輸入的會存進 `a` 裡，後輸入的則存進 `b`.

   你可以把這行想成下面這樣：

   ```c
   a = getInputDecimal();
   b = getInputDecimal();
   ```

5. `sum = a + b;`

   把 `a` 加 `b` 產生的值，存到 `sum` 這個變數裡。

   指派運算子 (`=`) 比較特別，它會先把它右邊的東西算完，再存到左邊的變數裡。

6. `// Output the sum of the 2 integers.`

   這也是註解，在寫程式時，我們會習慣在某一些地方加一些註解，讓別人或以後的自己看時，可以比較快理解這段程式碼在幹嘛。當然，不用一行程式碼搭配三行註解，你認為有必要加註解的地方再加就好。

7. `printf("%d+%d=%d\n", a, b, sum);`

   跟 `scanf` 類似，但這邊是輸出給我們看。通常題目都會要求輸出你程式算出來的答案。

   `printf` 還可以拿還來 debug, 當題目比較複雜時，可以先把計算過程中，比較容易出錯的步驟提前印出來，這樣萬一程式有 bug, 就可以很快找到是哪幾行的問題。等到要 submit 時再註解掉就可以了。

8. `return 0;`

   基本上每個人的程式都會有這行，代表程式執行過程中都很順利，因此回報給系統說，我的程式順利執行完畢。

   至少 C89 版本(含)以前都強迫要加這行。
   {: .notice--info}

   如果是寫 `return 1;` 或是其他的值，就是告知系統你的程式並未順利執行。

9. `}`

   很多人可能都會忽略掉這個後大括號。實際上在 `{` 和 `}` 裡面宣告的變數，只要碰到後面的 `}`，都會被釋放掉並把記憶體還給系統。

{% include {{page.id}}/quiz1.html %}

因為程式碼通常是從上到下一步一步執行的，所以 `printf` 比 `scanf` 早發生，那這樣 `a` 和 `b` 在 `printf` 前都還沒被指派值，所以在 `sum = a + b;` 那行就不合法了。

## Review

1. C 的程式碼是按照特定順序執行的，一次一個指令。
2. `main` function 是程式的進入點，回傳給系統說是否順利執行。

![little_coders]({{site.imgs}}{{page.id}}/little_coders.jpg)
<details><summary markdown="span">Credit</summary><div markdown="1">
[Geek & Poke](http://geek-and-poke.com/geekandpoke/2011/2/24/little-coders.html) is licensed under a [Creative Commons Attribution 3.0 Unported License](https://creativecommons.org/licenses/by/3.0/).
</div></details>
