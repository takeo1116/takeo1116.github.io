# Generate Bell state

## [問題ページ](https://codeforces.com/contest/1001/problem/B)

## 問題の概要

状態$\ket{00}$および整数$index$が与えられる。$index$の番号に対応したBell状態を作れ。

$$
\begin{align*}
\ket{B_0}&=\frac{1}{\sqrt{2}}\left( \ket{00}+\ket{11} \right)\\
\ket{B_1}&=\frac{1}{\sqrt{2}}\left( \ket{00}-\ket{11} \right)\\
\ket{B_2}&=\frac{1}{\sqrt{2}}\left( \ket{01}+\ket{10} \right)\\
\ket{B_3}&=\frac{1}{\sqrt{2}}\left( \ket{01}-\ket{10} \right)
\end{align*}
$$

## 解法
以下、2つのqubitが並んでいる状態を

$$
\begin{align*} \ket{\psi}&=\alpha\ket{0}_0\otimes\ket{0}_1+\beta\ket{0}_0\otimes\ket{1}_1+\gamma\ket{1}_0\otimes\ket{0}_1+\delta\ket{1}_0\otimes\ket{1}_1\\
&=\alpha\ket{00}+\beta\ket{01}+\gamma\ket{10}+\delta\ket{11}
\end{align*}
$$

という4つの直積状態の基底を用いて表します。


Bell状態は2つのqubitが強くエンタングルした状態であり、片方を観測すると直ちにもう片方の状態が定まります。問題で与えられる状態$\ket{00}$からこのような状態を作り出すには、どちらか1つのqubitに作用する量子ゲートだけを用いていても駄目で、2つの入力と2つの出力を持ち、「2つのqubit状態」に作用する操作をしてあげる必要があります。


状態$\ket{00}$からBell状態を作り出すためにはC-NOTゲート（制御NOTゲート）を利用すればよいです。C-NOTゲートは

$$G=\ket{0}_0\bra{0}I_1+\ket{1}_0\bra{1}X_1$$

で表される演算であり、ここで$I$は恒等演算子

$$
\begin{align*}
I\ket{0}=\ket{0}\\
I\ket{1}=\ket{1}
\end{align*}
$$

です。これは「1つめのqubitが$\ket{0}$であれば2つめのqubitにはなにもせず、1つめのqubitが$\ket{1}$であれば2つめのqubitを反転させる」演算であるといえます。「2つめのqubitに1つめのqubitを排他的論理和演算する」と言い換えることもできるでしょう。C-NOTゲートを状態$\ket{+}_1\otimes\ket{0}_2=\ket{+0}$に作用させると

$$
\begin{align*}
G\ket{+0}&=\left( \ket{0}_0\bra{0}I_1+\ket{1}_0\bra{1}X_1\right)\ket{+0}\\
&=\left( \ket{0}_0\bra{0}I_1+\ket{1}_0\bra{1}X_1\right)\frac{\ket{00}+\ket{10}}{\sqrt{2}}\\
&=\frac{\ket{00}+\ket{11}}{\sqrt{2}}
\end{align*}
$$

となり、Bell状態$\ket{B_0}$を得ます。同様に

$$
\begin{align*}
G\ket{-0}&=\frac{\ket{00}-\ket{11}}{\sqrt{2}}\\
&=\ket{B_1}\\
G\ket{+1}&=\frac{\ket{01}+\ket{10}}{\sqrt{2}}\\
&=\ket{B_2}\\
G\ket{-1}&=\frac{\ket{01}-\ket{10}}{\sqrt{2}}\\
&=\ket{B_3}\\
\end{align*}
$$

となります。


状態$\ket{+}$および$\ket{-}$を作りだす方法はA問題ですでにわかっているので、これでこの問題が解けました。


## ソースコード

```
namespace Solution {
    open Microsoft.Quantum.Primitive;
    open Microsoft.Quantum.Canon;

    operation Solve (qs : Qubit[], index : Int) : ()
    {
    body
        {
            if(index==0 || index==2){
                H(qs[0]);   //1つめのqubitを+に
            }
            else{
                X(qs[0]);
                H(qs[0]);   //1つめのqubitを-に
            }
            if(index==2 || index==3){
                X(qs[1]);    //2つめのqubitを1に
            }
            CNOT(qs[0],qs[1]);  //CNOTゲートを作用させる
        }
    }
}
```

[戻る](../index.md)