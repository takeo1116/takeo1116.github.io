# Generate plus state or minus state

## [���URL](https://codeforces.com/contest/1001/problem/A)

## ���̊T�v
���$\ket{0}$��qubit�Ɛ���$sign$���^������B$sign=1$�̂Ƃ����$\ket{+}=\frac{1}{\sqrt{2}}\left( \ket{0}+\ket{1} \right)$���A$sign=-1$�̂Ƃ����$\ket{-}=\frac{1}{\sqrt{2}}\left( \ket{0}-\ket{1} \right)$�����B

## ���
$\ket{+}$��$\ket{-}$����肽���̂ŁA�A�_�}�[���E�Q�[�g�𗘗p����΂悢�ł��B�A�_�}�[���E�Q�[�g��
$$
\begin{align*}
H&=\frac{\ket{0}+\ket{1}}{\sqrt{2}}\bra{0}+\frac{\ket{0}-\ket{1}}{\sqrt{2}}\bra{1}\\
&=\ket{+}\bra{0}+\ket{-}\bra{1}
\end{align*}
$$
�ŕ\����鉉�Z�ł���A��������$\ket{0}$�ɍ�p�������
$$
\begin{align*}
H\ket{0}&=\left( \frac{\ket{0}+\ket{1}}{\sqrt{2}}\bra{0}+\frac{\ket{0}-\ket{1}}{\sqrt{2}}\bra{1}\right)\ket{0}\\
&=\frac{\ket{0}+\ket{1}}{\sqrt{2}}\braket{0}{0}+\frac{\ket{0}-\ket{1}}{\sqrt{2}}\braket{1}{0}\\
&=\frac{\ket{0}+\ket{1}}{\sqrt{2}}\\
&=\ket{+}
\end{align*}
$$
�ƂȂ�A$\ket{1}$�ɍ�p������΁A���l��
$$
\begin{align*}
H\ket{1}&=\frac{\ket{0}-\ket{1}}{\sqrt{2}}\\
&=\ket{-}
\end{align*}
$$
�𓾂܂��B

���̖��ɂ����ẮA�͂��߂ɗ^����ꂽ���$\ket{0}$��qubit��ړI�̏�Ԃɕω����������̂ŁA$sign=1$�ł���΂��̂܂܃A�_�}�[���E�Q�[�g�ɒʂ��A$sign=-1$�ł���Ώ��$\ket{1}$�ɕω������Ă���A�_�}�[���E�Q�[�g�ɒʂ��܂��B

��Ԃ𔽓]������ɂ�NOT�Q�[�g�iPauli X�Q�[�g�j�𗘗p���܂��BNOT�Q�[�g��
$$X=\ket{1}\bra{0}+\ket{0}\bra{1}$$
�ŕ\����
$$
\begin{align*}
X\ket{0}&=\ket{1}\\
X\ket{1}&=\ket{0}
\end{align*}
$$
�̂悤�ɏ�Ԃ𔽓]�����鉉�Z�ł��B

����ŁA���̖�肪�����܂����B

## �\�[�X�R�[�h

```
namespace Solution {
    open Microsoft.Quantum.Primitive;
    open Microsoft.Quantum.Canon;

    operation Solve (q : Qubit, sign : Int) : ()
    {
        body
        {
            if(sign==-1){
                X(q);   //NOT�Q�[�g����p������
            }
            H(q);	//�A�_�}�[���E�Q�[�g����p������
        }
    }
}
```

[�߂�](../index.md)