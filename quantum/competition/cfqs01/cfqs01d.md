# Distinguish plus state and minus state

## [問題ページ](https://codeforces.com/contest/1001/problem/D)

## 問題の概要
状態$\ket{+}=\frac{1}{\sqrt{2}}\left( \ket{0}+\ket{1} \right)$と$\ket{-}=\frac{1}{\sqrt{2}}\left( \ket{0}-\ket{1} \right)$のどちらかが与えられる。与えられた状態がどちらの状態かを判別し、$\ket{+}$であったなら1を、$\ket{-}$であったなら-1を返せ。ただし、操作の後のqubitの状態は問わない。

## 解法

与えられた状態が$\ket{+}$か$\ket{-}$かを判別したいのですが、そのためには測定をしなくてはいけません。


ここで、ある演算子$A$がいくつかの固有状態とその固有値$\ket{\Psi_\lambda},\lambda$を使って

$$A=\sum_\lambda \lambda\ket{\Psi_\lambda}\bra{\Psi_\lambda}$$

と表せるとき、かつその固有値がすべて異なり、固有状態が規格直交であるとき、その固有状態のうちどれかを得る測定を「演算子$A$を対角とする測定」と呼びます。一般の量子状態$\ket{\Phi}$に対して演算子$A$を対角とする測定をすると、それぞれの固有値$\lambda$を、確率

$$
\begin{align*}
P_\lambda&=\bra{\Phi}M_\lambda\ket{\Phi}\\
&=\braket{\Phi}{\Psi_\lambda}\braket{\Psi_\lambda}{\Phi}     
\end{align*}
$$

で測定値として得ることになります。


例として、状態

$$\ket{\psi}=\alpha\ket{0}+\beta\ket{1},\;\;|\alpha|^2+|\beta|^2=1$$


に対してPauli Z演算子

$$Z=\ket{0}\bra{0}-\ket{1}\bra{1}$$

を対角とする測定を行うことを考えます。このとき、確率

$$
\begin{align*}
P_0&=\bra{\psi}M_0\ket{\psi}\\
&=\braket{\psi}{0}\braket{0}{\psi}\\
&=\alpha^*\alpha\\
&=|\alpha|^2
\end{align*}
$$

で$\ket{0}$の固有値である$1$を測定値として得ます。同様に、確率

$$
\begin{align*}
P_1&=\bra{\psi}M_1\ket{\psi}\\
&=|\beta|^2
\end{align*}
$$

で$\ket{1}$の固有値である$-1$を得ます。ここで$1$を得たとき、元の量子状態は状態$\ket{0}$に、-1を得たとき、元の量子状態は状態$\ket{1}$に変化しています。


以上のことから、状態$\ket{+},\ket{-}$はどちらも$\ket{0}$と$\ket{1}$が$1/2$の確率で測定される状態であるということがわかります。これは困ったことで、そのまま測定してもこの2つの状態を識別することはできません。そもそも始状態は1つしか貰えないので、$50\%$どころか$100\%$、確実に判別できる方法を考えたいところです(no-cloning定理によって、未知の量子状態を複製することはできません)。



A問題で状態$\ket{+},\ket{-}$を作ったとき、状態$\ket{0},\ket{1}$にアダマール・ゲートを作用させましたが、アダマール・ゲートはそれ自身がアダマール・ゲートの逆操作でもあるため、状態$\ket{+},\ket{-}$にもう一度作用させると元の状態に戻すことができます。

$$
\begin{align*}
H^2&=\left(\ket{+}\bra{0}+\ket{-}\bra{1}\right)^2\\
&=\ket{+}\braket{0}{+}\bra{0}+\ket{+}\braket{0}{-}\bra{1}+\ket{-}\braket{1}{+}\bra{0}+\ket{-}\braket{1}{-}\bra{1}\\
&=\frac{1}{\sqrt{2}}\left( \ket{+}\bra{0}+\ket{+}\bra{1}+\ket{-}\bra{0}-\ket{-}\bra{1} \right)\\
&=\frac12\left[ \left( \ket{0}+\ket{1} \right)\left( \bra{0}+\bra{1} \right)+\left( \ket{0}-\ket{1} \right)\left( \bra{0}-\bra{1} \right) \right]\\
&=\ket{0}\bra{0}+\ket{1}\bra{1}\\
&=I
\end{align*}
$$

アダマール・ゲートによって状態$\ket{+}$は$\ket{0}$に、状態$\ket{-}$は$\ket{1}$に変化するので、ここでPauli Z演算子を対角とする測定をして、$\ket{0}$であれば1を、$\ket{1}$であれば-1を返せばよいです。状態が最初と変わってしまっていますが、問題文によって状態を壊すことが許されているので問題ありません（そもそももう一度アダマール・ゲートを作用させれば元に戻る）。


これでこの問題が解けたのですが、実は、状態を壊さずにこの問題を解くこともできます。それは、Pauli X演算子を対角とする測定をする方法です。Pauli X演算子はNOTゲートとして使っていた演算子ですが、これは

$$
\begin{align*}
X&=\ket{1}\bra{0}+\ket{0}\bra{1}\\
&=\ket{+}\bra{+}-\ket{-}\bra{-}
\end{align*}
$$

と書き換えられるので、入力された量子状態に対してPauli X演算子を対角とする測定を行えば、状態$\ket{+}$のとき測定値$1$を、状態$\ket{-}$のとき測定値$-1$を得ます。


## ソースコード

Pauli Zを対角とする測定をする方法

```
namespace Solution {
    open Microsoft.Quantum.Primitive;
    open Microsoft.Quantum.Canon;

    operation Solve (q : Qubit) : Int
    {
        body
        {
            H(q);
            //固有値1を得たとき
            if(M(q)==Zero){
                return 1;
            }
            //固有値-1を得たとき
            return -1;
        }
    }
}
```

Pauli Xを対角とする測定をする方法
```
namespace Solution {
    open Microsoft.Quantum.Primitive;
    open Microsoft.Quantum.Canon;

    operation Solve (q : Qubit) : Int
    {
        body
        {
            //固有値1を得たとき
            if(Measure([PauliX],[q])==Zero){
                return 1;
            }
            //固有値-1を得たとき
            return -1;
        }
    }
}
```

[戻る](../index.md)