# Distinguish Bell states

## [問題ページ](https://codeforces.com/contest/1001/problem/E)

## 問題の概要
2つのqubitが与えられるので、それらが4つのBell状態のうちどの状態であるかを調べよ。ただし、操作の後のqubit状態は問わない。

$$
\begin{align*}
\ket{B_0}&=\frac{1}{\sqrt{2}}\left( \ket{00}+\ket{11} \right)\\
\ket{B_1}&=\frac{1}{\sqrt{2}}\left( \ket{00}-\ket{11} \right)\\
\ket{B_2}&=\frac{1}{\sqrt{2}}\left( \ket{01}+\ket{10} \right)\\
\ket{B_3}&=\frac{1}{\sqrt{2}}\left( \ket{01}-\ket{10} \right)
\end{align*}
$$

## 解法

B問題では、Bell状態を作るためにC-NOTゲートを利用しました。以下、C-NOTゲートがそれ自身の逆操作であることを示します。

まず、Pauli X演算子はアダマール演算子と同様にそれ自身の逆演算子になっています。

$$
\begin{align*}
X^2&=\left( \ket{1}\bra{0}+\ket{0}\bra{1} \right)^2\\
&=\ket{1}\braket{0}{1}\bra{0}+\ket{1}\braket{0}{0}\bra{1}+\ket{0}\braket{1}{1}\bra{0}+\ket{0}\braket{1}{0}\bra{1}\\
&=\ket{0}\bra{0}+\ket{1}\bra{1}\\
&=I
\end{align*}
$$

よってC-NOTゲート$G$は

$$
\begin{align*}
G^2&=\left( \ket{0}_0\bra{0}I_1+\ket{1}_0\bra{1}X_1 \right)^2\\
&=\ket{0}_0\bra{0}I_1^2+\ket{1}_0\bra{1}X_1^2\\
&=I
\end{align*}
$$

より、自分自身の逆操作であることが示されました。

以上のことから、与えられた状態にC-NOTゲートを作用させることで

$$
\begin{align*}
G\ket{B_0}&=\ket{+0}\\
G\ket{B_1}&=\ket{-0}\\
G\ket{B_2}&=\ket{+1}\\
G\ket{B_3}&=\ket{-1}
\end{align*}
$$

を得ます。よって、ここからそれぞれのqubitを測定することで4つのBell状態を区別でき、この問題を解くことができます。

## ソースコード

```
namespace Solution {
    open Microsoft.Quantum.Primitive;
    open Microsoft.Quantum.Canon;

    operation Solve (qs : Qubit[]) : Int
    {
        body
        {
            mutable ret=0;
            CNOT(qs[0],qs[1]);
            if(Measure([PauliX],[qs[0]])!=Zero){
                set ret=ret+1;
            }
            if(M(qs[1])!=Zero){
                set ret=ret+2;
            }
            return ret;
        }
    }
}
```

[戻る](../index.md)