---
title: "Variables and the Assignment Operator"
excerpt: "希望大家看完這篇後能釐清宣告變數和把值指派給宣告過的變數的意義。"
permalink: /c-language/variables-and-the-assignment-operator/
last_modified_at: 2018-09-24 23:06:00
tags: beginner
---

希望大家看完這篇後能釐清宣告變數和把值指派給宣告過的變數的意義。

## Declaring Variables (宣告變數)

### Syntax (語法)

向系統要一塊記憶體，讓我們可以存東西，我們將它命名，方便之後存取。

`<type> <var_name>;`

- `<type>` 是變數的資料型別。
- `<var_name>` 是變數的名稱。

### Example

向系統要一塊記憶體，讓我們可以放一個整數 (Integer), 那塊記憶體裡面的值會改變，因此把那塊記憶體稱作變數，我們把它取名為 `my_int`.

```c
int my_int;
```

### Analogy

這就有點像是國高中換到新教室時，老師會給我們一個格子 (置物櫃) 來放東西。我們每次要放東西時，我們會想說：「把東西放到『我的格子』裡」，並不會特別去想說：「把東西放到 XX 高中 XXX 班的 XX 號格子裡」。

在這裡，「格子」裡的東西 (值) 可以一直更換，所以「格子」就是變數，「我的格子」就是變數的名字。

## Assignment Operator (指派運算子)

{% include {{page.id}}/quiz1.html %}

### Syntax

把指派運算子 (等於的符號) 右邊的值計算出來後，存入左邊的變數。

`<var_name> = <value>;`

- `<var_name>` 是宣告過的變數。
- `<value>` 是我們想要存入變數的值。

### Example

可以想成下面任一種概念：
1. 把 `10` 存入我們宣告過的變數 `my_int` 裡面。
2. 把 `10` 指派給 `my_int`。
2. 把 `my_int` 設為 `10`。

口語上「指派」比較難念，所以很多人都還是會講「等於」，但在這邊的意思，並非數學中的等式。

```c
int my_int;
my_int = 10;
```

在數學中，通常用下面來表示上面程式碼的意思。

$$my\_int:=10$$

在演算法的書中，有時候會用左箭頭符號來避免混淆。

$$my\_int\leftarrow 10$$

### Analogy

把原本在格子裡的東西清掉，之後把十塊錢放進格子裡。

1. `int my_int;`

   ![img]({{site.imgs}}{{page.id}}/1.1-indeterminate.png)
2. `my_int = 10`

   ![img]({{site.imgs}}{{page.id}}/1.2-assign.png)
3. `my_int` is `10`

   ![img]({{site.imgs}}{{page.id}}/1.3-result.png)

## Declare and Define Multiple Variables

平常寫程式時，我們可能會宣告很多型別相同的變數。那就可以寫成下面這樣，跟分開宣告的作用一模一樣。

```c
int a, b, c;
```

指派初始值跟變數宣告可以連在一起，可以把下面這行當作跟分成兩行一樣。

```c
int a = 0;
```

{% include {{page.id}}/quiz2.html %}

{% include {{page.id}}/quiz3.html %}

可以視作 `a = (b = (2));`

1. `int a = 0, b = 1`

   ![img]({{site.imgs}}{{page.id}}/2.1-initial.png)

2. `(b = 2)`

   ![img]({{site.imgs}}{{page.id}}/2.2-r-r.png)

3. `b` is `2`

   ![img]({{site.imgs}}{{page.id}}/2.3-result-1.png)

4. `(b = 2)` $$\rightarrow$$ `(b)`

   ![img]({{site.imgs}}{{page.id}}/2.4-get.png)

5. `a = (b = 2)` $$\rightarrow$$ `a = (b)` $$\rightarrow$$ `a = (2)`

   ![img]({{site.imgs}}{{page.id}}/2.5-r.png)

6. `a` is `2`, `b` is `2`

   ![img]({{site.imgs}}{{page.id}}/2.6-result-2.png)

## Indeterminate Value (未定義)

{% include {{page.id}}/quiz4.html %}

沒有被初始化的區域變數，它的初始值並沒有被定義，因此這個變數不應被存取。如果真的不小心存取到，會是 undefined behavior, 也就是說是不合法的，但它通常會是 garbage value, 也就是奇怪的數值，知道一下對 debug 會比較有幫助。

這就像你剛搬到新的教室時，你的格子 (置物櫃) 裡面通常都會有一些別人留下來的垃圾 (garbage value)，很少是乾淨的 (0), 而你放東西前，自然就會把垃圾清掉再放，不然就沒空間了。

{% include {{page.id}}/quiz5.html %}

指派運算子左邊的東西要放變數 (格子)，這樣東西才放得進去。

{% include {{page.id}}/quiz6.html %}

就跟上面畫的那些格子一樣，`x = y` 是把 `y` 的值複製一份拿出來之後放到 `x` 裡，所以當之後 `y` 的值變了，`x` 的值當然不會跟著變。

## Review
1. 指派運算子 (`=`) 並不是數學的等於，而是把 `=` 左邊的變數設為 `=` 右邊經計算而得的某個特定的數值。
2. 區域變數如果沒有被初始化，是不應被存取的。萬一真的存取到它，通常會是一些 garbage value.

![var_name]({{site.imgs}}{{page.id}}/var_name.png)
<details><summary markdown="span">Credit</summary><div markdown="1">
Posted on [Reddit](https://www.reddit.com/r/ProgrammerHumor/comments/5z6n8p/every_time/).
</div></details>
