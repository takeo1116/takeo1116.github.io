# Generate plus state or minus state

## [問題ページ](https://codeforces.com/contest/1001/problem/A)

## 問題の概要
状態$\ket{0}$のqubitと整数$sign$が与えられる。$sign=1$のとき状態$\ket{+}=\frac{1}{\sqrt{2}}\left( \ket{0}+\ket{1} \right)$を、$sign=-1$のとき状態$\ket{-}=\frac{1}{\sqrt{2}}\left( \ket{0}-\ket{1} \right)$を作れ。

## 解説
$\ket{+}$と$\ket{-}$を作りたいので、アダマール・ゲートを利用すればよいです。アダマール・ゲートは

$$
\begin{align*}
H&=\frac{\ket{0}+\ket{1}}{\sqrt{2}}\bra{0}+\frac{\ket{0}-\ket{1}}{\sqrt{2}}\bra{1}\\
&=\ket{+}\bra{0}+\ket{-}\bra{1}
\end{align*}
$$

で表される演算であり、これを状態$\ket{0}$に作用させれば

$$
\begin{align*}
H\ket{0}&=\left( \frac{\ket{0}+\ket{1}}{\sqrt{2}}\bra{0}+\frac{\ket{0}-\ket{1}}{\sqrt{2}}\bra{1}\right)\ket{0}\\
&=\frac{\ket{0}+\ket{1}}{\sqrt{2}}\braket{0}{0}+\frac{\ket{0}-\ket{1}}{\sqrt{2}}\braket{1}{0}\\
&=\frac{\ket{0}+\ket{1}}{\sqrt{2}}\\
&=\ket{+}
\end{align*}
$$

となり、$\ket{1}$に作用させれば、同様に

$$
\begin{align*}
H\ket{1}&=\frac{\ket{0}-\ket{1}}{\sqrt{2}}\\
&=\ket{-}
\end{align*}
$$

を得ます。

この問題においては、はじめに与えられた状態$\ket{0}$のqubitを目的の状態に変化させたいので、$sign=1$であればそのままアダマール・ゲートに通し、$sign=-1$であれば状態$\ket{1}$に変化させてからアダマール・ゲートに通します。

状態を反転させるにはNOTゲート（Pauli Xゲート）を利用します。NOTゲートは
$$X=\ket{1}\bra{0}+\ket{0}\bra{1}$$
で表され

$$
\begin{align*}
X\ket{0}&=\ket{1}\\
X\ket{1}&=\ket{0}
\end{align*}
$$

のように状態を反転させる演算です。

これで、この問題が解けました。

## ソースコード

```
namespace Solution {
    open Microsoft.Quantum.Primitive;
    open Microsoft.Quantum.Canon;

    operation Solve (q : Qubit, sign : Int) : ()
    {
        body
        {
            if(sign==-1){
                X(q);   //NOTゲートを作用させる
            }
            H(q);	//アダマール・ゲートを作用させる
        }
    }
}
```

[戻る](../index.md)