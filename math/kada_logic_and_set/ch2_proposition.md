# 命題計算
## 2.1 命題と真理値
命題: 真か偽かを判断できる分や式

例）  
1+1=4 → 命題  
1+1 → 命題ではない  
x+1=3 → 命題ではない（xが決まらないと真偽が判定できない）  

命題pが真か偽かという情報を命題pの真理値という。  
真を意味する真理値をTで、偽を意味する真理値をFで表す。

## 2.2 論理演算子と真理値表
### かつ（論理積）
命題p、qに対して「pかつq」という文もまた命題になる。  
この命題をpとqの論理積、連言、積といい、$p \land q$と表す。

| $p$ | $q$ | $p \land q$ |
|:---:|:---:|:---:|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | F |

### または（論理和）
命題p、qに対して「pまたはq」という文もまた命題になる。  
この命題をpとqの論理和、選言、和といい、$p \lor q$と表す。

| $p$ | $q$ | $p \lor q$ |
|:---:|:---:|:---:|
| T | T | T |
| T | F | T |
| F | T | T |
| F | F | F |

### でない（否定）
命題pに対して「pではない」という文もまた命題になる。  
この命題をpの否定といい、$\lnot p$と表す。

| $p$ | $\lnot p$ |
|:---:|:---:|
| T | F |
| F | T |

### 真理値の計算
真理値表を考えることにより、命題そのものの意味を解釈することなく命題の真理値を計算できる

### 恒真命題と矛盾命題
$p \lor \lnot p$のように、命題の真理値が常にTになるとき、その命題を恒真命題またはトートロジーという。またこの性質を排中律という。

逆に常に真理値がFになる命題を矛盾命題という。（例: $p \land \lnot p$）またこの性質を矛盾律という。

## 2.3 命題代数
### 論理同値
命題p、qがあり、それらの演算により得られる命題r、sを考える。  
p、qの任意の真理値についてrとsの真理値が同じであるようなとき、rとsは論理同値であるといい、$r \equiv s$と表す。

### 命題代数の法則
#### べき等律
$p \lor p \equiv p$  
$p \land p \equiv p$

#### 結合律
$(p \lor q) \lor r \equiv p \lor (q \lor r)$  
$(p \land q) \land r \equiv p \land (q \land r)$

#### 交換律
$p \lor q \equiv q \lor p$  
$p \land q \equiv q \land p$

#### 分配律
$p \lor (q \land r) \equiv (p \lor q) \land (p \lor r)$  
$p \land (q \lor r) \equiv (p \land q) \lor (p \land r)$

#### 同一律
$p \lor f \equiv p$  
$p \land t \equiv p$

$p \lor t \equiv t$  
$p \land f \equiv f$

#### 補元律
$p \lor \lnot p \equiv t$  
$p \land \lnot p \equiv f$

$\lnot t \equiv f$  
$\lnot f \equiv t$

#### 対合律
$\lnot \lnot p \equiv p$

#### ド・モルガンの法則
$\lnot (p \lor q) \equiv \lnot p \land \lnot q$  
$\lnot (p \land q) \equiv \lnot p \lor \lnot q$

### 結合律と $\land$、$\lor$ の拡張
結合律より、$p \land q \land r$と書くことが許される。  
本来は2つの命題に対する演算であった$\land$、$\lor$を3つ以上の命題に対する演算に拡張し、$p_1 \land p_2 \land ... \land p_n$と表す。

### 命題代数の双対性
上記の命題代数の法則の各ペアにおいて、以下の変換を行うともう一方の法則になる。（双対性）

- $\land$ を $\lor$ に置き換え
- $\lor$ を $\land$ に置き換え
- $t$ を $f$ に置き換え
- $f$ を $t$ に置き換え

※対合率は1つしかないが、上記操作を行っても何も変化がなく自身と同じになる。（自己双対）

## 2.4 含意と同値の論理演算子
### 含意
「pならばqである」という形の文（条件文）を表す論理演算子。  
$p \rightarrow q$と表す。

| $p$ | $q$ | $p \rightarrow q$ |
|:---:|:---:|:---:|
| T | T | T |
| T | F | F |
| F | T | T |
| F | F | T |

$p \rightarrow q$と$\lnot p \lor q$は論理同値である。

| $p$ | $q$ | $\lnot p$ | $\lnot p \lor q$ |
|:---:|:---:|:---:|:---:|
| T | T | F | T |
| T | F | F | F |
| F | T | T | T |
| F | F | T | T |

### 同値
「pとqは同値である」という形の文を表す演算子。  
$p \leftrightarrow q$と表す。

| $p$ | $q$ | $p \leftrightarrow q$ |
|:---:|:---:|:---:|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | T |

$p \leftrightarrow q$と$(p \land q) \lor (\lnot p \land \lnot q)$は論理同値である。

| $p$ | $q$ | $p \land q$ | $\lnot p \land \lnot q$ | $(p \land q) \lor (\lnot p \land \lnot q)$ |
|:---:|:---:|:---:|:---:|:---:|
| T | T | T | F | T |
| T | F | F | F | F |
| F | T | F | F | F |
| F | F | F | T | T |

### 逆・裏・対偶
$p \rightarrow q$という命題があるとき

逆: $q \rightarrow p$
裏: $\lnot p \rightarrow \lnot q$
対偶: $\lnot q \rightarrow \lnot p$

$p \rightarrow q$の対偶$\lnot q \rightarrow \lnot p$は

$$\lnot q \rightarrow \lnot p \equiv \lnot \lnot q \lor \lnot p \equiv q \lor \lnot p \equiv \lnot p \lor q \equiv p \rightarrow q$$

となるので、$p \rightarrow q$と論理同値である。
