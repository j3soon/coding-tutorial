---
title: "Recursion - Hanoi Tower"
excerpt: "在了解遞迴之前，我們要先知道什麼是遞迴。"
permalink: /c-language/recursion---hanoi-tower/
last_modified_at: 2018-11-16 03:00:00
tags: beginner
---

在了解遞迴之前，我們要先知道什麼是遞迴。

以河內塔為例，在這邊紀錄一下我寫 recursion 時的思路。

## Thoughts

1. 把想寫的 function 用一句簡單的人話表達出來 (在敘述內須包含變數)

   ```c
   move_hanoi(int n, int from, int to, int temp);
   ```

   把 `from` 這根柱子上，靠近上面 `n` 個盤子用符合規則的方法移到 `to` 那根柱子, `temp` 可以拿來暫放其他盤子。

   在寫完這個 function 之前，先把我們想寫出來的這個 function 先當成是可以正確運作的。

   所以如果題目要求我把 n 個盤子從 1 移到編號 3 的柱子上，那我就只需要 call function `move_hanoi(n, 1, 3, 2);` 這樣就結束了。

   ![img]({{site.imgs}}{{page.id}}/abstract1.png)

   ![img]({{site.imgs}}{{page.id}}/abstract2.png)

   這個步驟我覺得是最難想到的，有時候可能要先思考 recursion case 要如何設計，才能想到該怎麼設計這個 function.

2. 定義 Basic Case 並把它處理掉

   假設題目只要求我們移動一個盤子，我們該怎麼移。

   ![img]({{site.imgs}}{{page.id}}/basic1.png)

   ![img]({{site.imgs}}{{page.id}}/basic2.png)

   ```c
   // 直接把盤子移到目標棍子
   printf("Move disk %d from %d to %d\n", 1, from, to);
   ```

   為了避免遞迴無限深入，一定會有最終的 basic case, 也就是終止條件，在這裡就是當只剩一個盤子時，我們就可以直接移動它。

3. 處理 Recursion Case

   Basic Case 出來了所以可以先假設我們 call `move_hanoi(1, ...)`,  `move_hanoi(2, ...)`, ..., `move_hanoi(n-1, ...)` 都是可以使用，並且能正常運作的，然後想辦法處理 `n` 的情況，有點像數學歸納法的感覺。

   記得把 `move_hanoi` 這個 function 當成是對的去利用，先不用管說它還沒寫好，就當 `printf`, `strlen` 這種東西來用。

   我覺得這點特別重要。大部分的人寫遞迴時都會很容易糾結在這個點上，原因就是我 function 還沒寫完，怎麼能 call 他呢？因此不去 call 它的結果就會導致在思考 recursion 的過程時會一直往下深入去想，最後打了一大堆 code 卻都還打不完。

   但是其實就只要大膽的去 call 就好了，把它當作數學歸納法的證明方式來寫，這樣就只要記得當初我們定義的 function 要傳入什麼，有什麼功用就可以很放心地去使用了。

   > ```c
   > move_hanoi(int n, int from, int to, int temp);
   > ```
   >
   > 把 `from` 這根柱子上，靠近上面 `n` 個盤子用符合規則的方法移到 `to` 那根柱子, `temp` 可以拿來暫放其他盤子。

   ![img]({{site.imgs}}{{page.id}}/recur1.png)

   ![img]({{site.imgs}}{{page.id}}/recur2.png)

   ![img]({{site.imgs}}{{page.id}}/recur3.png)

   ![img]({{site.imgs}}{{page.id}}/recur4.png)

   ```c
   // 把上面其他盤子用符合規則的方法先移到另一根暫放
   move_hanoi(n - 1, from, temp, to);
   // 移動最下面的盤子到目的地
   printf("Move disk %d from %d to %d\n", n, from, to);
   // 把上面其他盤子用符合規則的方法從暫放區移回來
   move_hanoi(n - 1, temp, to, from);
   ```

   > 在寫遞迴時，把目前正在寫的這個 function 當作是對的來用。

   在 `temp` 那根柱子上不一定會是全空的，但我們能照樣暫放盤子，因為原本就在柱子上面的盤子一定都比要暫放的盤子大。

4. 把整個 function 用人話講一遍 (像是上面的註解們)，感覺沒問題就對啦！

   因為我們在寫的過程中其實就是用數學歸納法來證明了，所以執行結果就會是對的，當然額外的數學證明會更嚴謹。

5. 完整的 code

   ```c
   void move_hanoi(int n, int from, int to, int temp) {
       if (n == 1)
           // 如果只有一個盤子要移
           // 直接把盤子移到目標棍子
           printf("Move disk %d from %d to %d\n", 1, from, to);
       else {
           // 如果有多個盤子要移
           // 把上面其他盤子用符合規則的方法先移到另一根暫放
           move_hanoi(n - 1, from, temp, to);
           // 移動最下面的盤子到目的地
           printf("Move disk %d from %d to %d\n", n, from, to);
           // 把上面其他盤子用符合規則的方法從暫放區移回來
           move_hanoi(n - 1, temp, to, from);
       }
   }
   ```

放一下我很喜歡的一張圖，來自維基百科。

![img]({{site.imgs}}{{page.id}}/pic.png)

最左邊那個 column 就是把整個 function 變成一個 black box 的感覺，我們完全不用知道裡面在幹嘛，直接呼叫 $$n=N$$ 就好。

左邊第二個就是我們在寫 recursion case 時的思路，就是直接把整個 function 當作已經寫好了來用，但只能用到 $$n=N-1$$。

右邊最下面就是我們寫 basic case 時的做法，直接移過去就好 $$n=1$$。

其他部分就是程式在跑的時候實際會一步一步執行的樣子，但盤子數一多，人腦很難把每一步全部都想一遍，就算可以，還是會很混亂。所以利用遞迴的概念，就可以把這整個部分抽象化，省去很多不必要的麻煩。

## Complexity analysis

假設有 n 個盤子，移動一次盤子需要耗費 c.

列一下式子：

$$\begin{align}
T(0)&=0\\
T(1)&=c\\
T(n)&=2*T(n-1)+c\\
&=2*2*T(n-2)+2*c+c\\
&=2*2*2*T(n-3)+4*c+2*c+c\\
&=2^n*T(0)+\sum^{n-1}_{k=0}(2^k*c)\\
&=c*\sum^{n-1}_{k=0}2^k
\end{align}$$

代入等比級數公式

$$\sum^{n-1}_{k=0}2^k=\frac{2^0*(1-2^{n-1})}{1-2}=2^n-1$$

$$T(n)=(2^n-1)*c$$

這樣我們就知道複雜度是 $$O(2^n)$$, 共要移動 $$2^n-1$$ 次。

## Further Readings

1. [Recursion on GeeksforGeeks](https://www.geeksforgeeks.org/recursion/)

## Review
1. 把想寫的 function 用一句簡單的人話表達出來 (在敘述內須包含變數)
2. 定義 Basic Case 並把它處理掉
3. 處理 Recursion Case
4. 把整個 function 用人話講一遍，沒問題就對啦！

![google-recursion]({{site.imgs}}{{page.id}}/google-recursion.png)
