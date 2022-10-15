# Generate GHZ state

## [問題ページ](https://codeforces.com/contest/1001/problem/C)

## 問題の概要
$N$桁の量子状態$\ket{0...0}$が与えられる。GHZ状態

$$\ket{GHZ}=\frac{1}{\sqrt{2}}\left( \ket{0...0}+\ket{1...1}\right)$$

を作れ。

## 解法

まず、問題文にあるように、GHZ状態とはA問題で作った状態$\ket{+}$やB問題で作ったBell状態$\ket{B_0}$をqubitの数$N$について一般化したものであることがわかります。すると、それらの状態を作る操作も$N$について一般化してやることができて、初期状態$\ket{+0...0}$に対して、0番目のqubitを制御bitとして1番目からN-1番目のqubitに対して順番にC-NOTゲートを作用させることでGHZ状態を作り出すことが可能です。

つまり、i番目のqubitとj番目のqubitに作用するC-NOTゲートを

$$G_{i,j}=\ket{0}_i\bra{0}I_j+\ket{1}_i\bra{1}X_j$$

と書くことにすれば、GHZ状態は初期状態$\ket{+0...0}$を用いて

$$\ket{GHZ}=G_{0,N-1}...G_{0,1}\ket{+0...0}$$

と表せることになります。

また、0番目のqubitの代わりに1つ前のqubitを使うことでも、この問題を解くことができます。

## ソースコード

```
namespace Solution {
    open Microsoft.Quantum.Primitive;
    open Microsoft.Quantum.Canon;

    operation Solve (qs : Qubit[]) : ()
    {
        body
        {
            H(qs[0]);
            for(i in 1..Length(qs)-1){
                CNOT(qs[0],qs[i]);    //0番目のqubitを制御bitに、i番目のqubitをターゲットbitにしてCNOTゲートを作用させる
            }
        }
    }
}
```

[戻る](../index.md)