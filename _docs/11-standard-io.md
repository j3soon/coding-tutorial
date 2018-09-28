---
title: "Standard I/O"
excerpt: "這篇主要在複習如何讀取輸入的值和輸出東西到螢幕上。"
permalink: /c-language/standard-io/
last_modified_at: 2018-09-27 01:39:00
tags: beginner
---

這篇主要在複習如何讀取輸入的值和輸出東西到螢幕上。

其實沒啥好講的，怎麼用都好好寫在 [stdio.h](http://www.cplusplus.com/reference/cstdio/) 了，有問題就自己查吧。

## Inputs

常用的有 `scanf`, `getchar`, `gets` (`fgets`), ...

### Basic Formats

{% include {{page.id}}/quiz1.html %}

`scanf` 要把 input 的值存進變數裡，所以要傳變數位址。

e.g. `&a`.

{% include {{page.id}}/quiz2.html %}

`%2d` 代表讀進來兩個 digits.

{% include {{page.id}}/quiz3.html %}

在 `scanf` 特別指定要 match 到的字元，輸入就要 match 才 scan 得到。

e.g. `(` 就是叫 `scanf` 期待下一個就是 `(`, 所以輸入要確實是 `(` 才行，空白、換行都不行。

在輸入時，在數字前面可以亂加空格的原因就是因為 `%d` 會忽視在數字前面所有看不到的字元 e.g. 空白、換行...

{% include {{page.id}}/quiz4.html %}

`scanf` float 要用 `%f`, double 要用 `%lf`.

### Usages of `scanf("%c", ...)` and `getchar`

{% include {{page.id}}/quiz5.html %}

`scanf("%c", ...)` 會把空格和換行字元都讀進去。

所以 `a` is `{'A', ' ', 'B', ' ', '\n', 'C'}`.

如果不知道自己哪裡錯，有可能是漏掉最後印出來的空白。

{% include {{page.id}}/quiz6.html %}

因為 `scanf(" %c", ...)` 前面的空白會忽略所有看不到的字元，像是空白和換行；這點很重要，因為像是 `scanf("%d", ...)`, `scanf(" %d", ...)` 這兩個其實根本沒差，因為 `%d` 就已經會自動忽略 (在輸入的數字前面) 那些看不到的字元了。

所以 `a` is `{'A', 'B', 'C', 'D', 'E', 'F'}`.

{% include {{page.id}}/quiz7.html %}

`getchar()` 會把空格和換行字元都回傳給你，其實跟 `scanf("%c", ...)` 差不多。

所以 `a` is `{'A', 'B', ' ', 'C', '\n', 'D'}`.

### Usages of `scanf("%s", ...)`

{% include {{page.id}}/quiz8.html %}

用 `scanf("%s", a)` 就好是因為陣列變數本身可以被當作像 pointer 來 dereference, 當然，用 `scanf("%s", &a)` 也是完全 OK 的，只是有點多此一舉罷了。

`scanf("%s", ...)` 會忽略 (在字串前) 看不到的字元 (跟 `%d` 一樣), 然後會一直讀取直到遇到看不到的字元 (跟 `%d` 一樣), 再在最後補零後結束，以上面題目為例就是遇到 `' '` 後在後面補 `'\0'` 結束。

在 `'\0'` 後面的值並不會被動到，這也代表著 C 語言並不知道你的陣列或指標什麼時候會跑到不該存取的地方，所以要靠自己小心，不要寫出 undefined behavior 的 code.

{% include {{page.id}}/quiz9.html %}

因為 `scanf("%s", ...)`會想要在陣列最後面補一個 `'\0'`, 也就是 C 語言中著名的 Null-terminated string. 但是 `a[3]` 是不應被 access 的，所以雖然在電腦上跑通常都沒事，但是實際上是可能會出事的，是不合法的。

### `gets` vs `fgets`

如果要把一整行輸入讀進來，這兩個就很好用啦～

`gets` 大家不愛用的原因是因為不能定義一次要讀多少個字元，很容易超出去。

{% include {{page.id}}/quiz10.html %}

`fgets` 會把換行也讀進來喔！

{% include {{page.id}}/quiz11.html %}

`gets` 不會把換行讀進去。

### `scanf`'s return value

{% include {{page.id}}/quiz12.html %}

`scanf` 會回傳它成功 `scan` 進去多少個變數，以上面來講就是 `1`, 因為第二個失敗了 (`abc`), 失敗後他就不會去動 `b` 的值，然後直接回傳 `1`. 如果兩個都失敗就會回傳 `0`, 兩個都成功就會回傳 `2`.

{% include {{page.id}}/quiz13.html %}

跟上面一樣，但是因為 `,` 前面沒有 `' '` 所以它不會自動忽略所有 (在他前面) 看不到的字元，如果看到的不是 `,` 他就會當作後面的東西都 scan 失敗。

### `EOF`

`EOF` 被定義為 `-1`, 在所有輸入結束後，如果 `scanf`, `getchar` 他們都會回傳 `EOF`, 也就是 `-1`, `scanf` 也會當作讀取輸入失敗，就不會去動那些變數。

`gets` 和 `fgets` 讀到 `EOF` 則是會回傳 `NULL` (通常會是 `0`).

這邊就不做 Quiz 了，通常用法就是下面這幾種：

```c
while (scanf("%d", &x) != EOF);
while (scanf("%d%d", &a, &b) == 2);
while ((c = getchar()) != EOF);
// Change 'MAX_STRLEN' to a constant (max string length including null-terminator)
while (fgets(s, MAX_STRLEN, stdin) != NULL);
```

在 Windows 作業系統上按 Ctrl+Z 再按 Enter 就可以模擬 `EOF`, Mac 作業系統和 Linux 上則是 Ctrl+D.

## Case Study (Input)

把一個五位數數字的各個位數分離。

如果有

Input:

```
12345

```

我們想把每個位數存到對應的變數裡，e.g. `a`, `b`, `c`, `d`, `e`.

### Math

```c
int n;
scanf("%d", &n);
a = n / 10000 % 10;
b = n / 1000 % 10;
c = n / 100 % 10;
d = n / 10 % 10;
e = n / 1 % 10;
```

### Scan 1-digit at a Time

```c
scanf("%1d%1d%1d%1d%1d", &a, &b, &c, &d, &e);
```

### Scan by char

```c
a = getchar() - '0';
b = getchar() - '0';
c = getchar() - '0';
d = getchar() - '0';
e = getchar() - '0';
```

### Scan by string

```c
char str[6];
scanf("%s", &str);
a = str[0] - '0';
b = str[1] - '0';
c = str[2] - '0';
d = str[3] - '0';
e = str[4] - '0';
```

還有各式各樣的方法，簡單來講，就是 Input 可以有無限多種讀取方式，你爽用哪一種就用哪一種～

## Outputs

常用的有 `printf`, `putchar`, `puts`, ...

其實這比較沒啥好講的，`putchar` 就是一次輸出一個字元，`puts` 就是把一整個字串輸出，並在最後補 `\n`, 簡單來講就是它會幫你換行。

float 和 double 都是用 `printf("%f", ...)`, 你用 `%lf` 也可以啦，但 double 輸出時不用加 `l`.

String 記得最後面都要有 `'\0'`, 這些 function 才會知道要印到什麼時候才停。

{% include {{page.id}}/quiz14.html %}

輸出到 `'\0'` 就會停了。

### Escape Characters

{% include {{page.id}}/quiz15.html %}

`printf` `\\` 會是 `'\'`, `\"` 會是 `'"'`, `%%` 會是 `'%'`.

像上面那個用 `puts` 就很簡單，但用 `printf` 就很心酸，但都可以達到目的啦。

### Output Formatting

{% include {{page.id}}/quiz16.html %}

想補零跟補空白就是這樣做。

如果不知道哪裡錯的話，注意前面的空格數量。

{% include {{page.id}}/quiz17.html %}

float 的確可以那樣指派喔，`e` 後面代表前面的數字乘以十的多少次方。

不指定的話，預設就是小數點後六位。

不要懷疑，`printf` 真的會幫忙四捨五入。

{% include {{page.id}}/quiz18.html %}

`%lld` for `long long`, `%llu` for `unsigned long long`, `printf("%*d", NUM, ...)` will replace the `*` to `NUM`.

{% include {{page.id}}/quiz19.html %}

## Review

1. `scanf("%c", ...)`, `getchar` 並不會忽略看不到的字元。
2. `gets` 不會把換行字元存起來，`fgets` 會儲存換行字元。
3. 讀到 end-of-file 時，`scanf`, `getchar` 回傳 `EOF`； `gets`, `fgets` 回傳 `NULL`。
4. 讀字串時，記得要多開一個字元給 `'\0'` 住。
5. strings should be null-terminated before output using `printf("%s", ...)` or `puts`.

比較常搞混的就是這些，其他就自己查吧！像什麼 `%x`, `%#x`, `%hd` 什麼鬼的就自己想辦法吧。

如果上機考真的忘記像是前面怎麼補零之類的，也可以用 `if` 來處理，只是很麻煩，但仍然可以 AC.

![c_hello_world]({{site.imgs}}{{page.id}}/c_hello_world.jpg)
<details><summary markdown="span">Credit</summary><div markdown="1">
Posted on [Reddit](https://www.reddit.com/r/ProgrammerHumor/comments/6xrxbm/hello_world_using_c/).
</div></details>

## Further Readings
1. [stdio.h](http://www.cplusplus.com/reference/cstdio/)
2. [scanf](http://www.cplusplus.com/reference/cstdio/scanf/)
3. [getchar](http://www.cplusplus.com/reference/cstdio/getchar/)
4. [gets](http://www.cplusplus.com/reference/cstdio/gets/)
5. [fgets](http://www.cplusplus.com/reference/cstdio/fgets/)
6. [printf](http://www.cplusplus.com/reference/cstdio/printf/)
7. [putchar](http://www.cplusplus.com/reference/cstdio/putchar/)
8. [puts](http://www.cplusplus.com/reference/cstdio/puts/)