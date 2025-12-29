# Fire 言語仕様書

**リファレンス実装のための詳細かつ具体的な仕様書**

TODO が書いてある箇所は、書いてる途中であることを意味します。

2026 ringo.

> ------------
> **目次**
>   - [**A. 字句解析 (Lexer)**](#a-字句解析)
>       - [1. 数値リテラル](#1-数値リテラル)
>       - [2. 文字・文字列リテラル](#2-文字文字列リテラル)
>       - [3. 名前 (識別子)](#3-識別子)
>       - [4. キーワード (予約語)](#4-キーワード)
>       - [5. 演算子](#5-演算子)
>       - [6. 記号](#6-記号)
>   - [**B. 構文解析 (Parser)**](#b-構文解析-parser)
>       - [1. シンボル](#1-シンボル-symbol)
>       - [2. 型名](#2-型名-type-name)
>           - [型名を使用可能な場所](#型を記述できる箇所の一覧:)
>           - [式中での型名の使用](#式中に型名が含まれる場合)
>       - [3. 式](#3-式)
>           - [演算子の優先順位](#演算子の優先順位)
>           - [factor: ファクタ](#factor)
>           - [primary: プライマリ](#プライマリ)
>           - [unary: 単項算術演算](#単項算術)
>           - [terms: 二項算術演算](#二項算術)
>           - [shift: シフト演算](#シフト演算)
>           - [compare: 比較](#比較)
>           - [equality: 等価比較](#等価比較)
>           - [bit: ビット演算](#ビット演算)
>           - [log: 論理積・論理和](#論理積論理和)
>           - [assign: 代入](#代入)
>       - [4. 文](#4-文)
>           - [条件文](#条件文)
>               - [if](#if)
>               - [match](#match)
>           - [繰り返し文](#繰り返し文)
>               - [for](#for)
>               - [loop](#loop)
>               - [do-while](#do-while)
>               - [while](#while)
>           - [単文](#単文)
>               - [return](#return)
>               - [break](#break)
>               - [continue](#continue)
>               - [式文](#式文)
>           - [スコープ](#スコープ)
>           - [変数定義 (var)](#変数定義-var)
>           - [例外処理 (try-catch)](#例外処理-try-catchs)
>       - [5. 関数]()
>           - [定義]()
>           - [テンプレート]()
>       - [6. 列挙型]()
>           - [定義]()
>           - [バリアント]()
>       - [7. クラス]()
>           - [定義]()
>           - [インスタンス作成]()
>           - [継承]()
>           - [抽象クラス]()
>           - [テンプレート]()
>       - [8. 名前空間]()
>   - [**C. 型システム (TypeInfo)**]()
>       - [1. ]()
>   - [**D. 意味解析 (Sema)**]()
>       - [1. スコープ構築](#1-スコープ情報の構築)
>       - [2. シンボル収集]()
>       - [3. 名前解決]()
>       - [4. 整合性・型チェック]()
>   - [**E. 構文木の評価 (EvalNode)**]()
>       - [1. スタック]()
>       - [2. 式の評価]()
>       - [3. 文の実行]()
>   - [**F. コンパイル (LLVM-IR)**]()
>       - Feature...
>   - [**G. アプリケーション (Driver)**]()
>       - [1. ソースファイル管理]()
>           - [ファイル読み取り]()
>           - [外部モジュール取り込み (import)]()
>       - [2. コマンドライン引数]()
>   - [**H. コマンドライン実行 (REPL)**]()
> ------------

# A. 字句解析

## 1. 数値リテラル
```
INTEGER     :=  [0-9]+
FLOAT       :=  [0-9]+\.?[0-9]*f?
```

## 2. 文字・文字列リテラル
```
CHAR        :=  '(.|\\[a-z|A-Z])'
STRING      :=  "(.|\\[a-z|A-Z])*"
```

## 3. 識別子

```
IDENTIFIER  :=  (_|[a-z]|[A-Z])(_|[a-z]|[A-Z]|[0-9])*
```

## 4. キーワード

Fire 言語の予約語一覧。<br>
左側の「トークン名」は本仕様書における文法の定義に使われ、言語内で実際に使用するときは右の「キーワード」を使う。

### 基本型
| トークン名      | キーワード  |
|-----------------|-------------|
| `KWD_NONE` | **None** |
| `KWD_INT` | **int** |
| `KWD_FLOAT` | **float** |
| `KWD_BOOL` | **bool** |
| `KWD_CHAR` | **char** |
| `KWD_STRING` | **string** |

### 組み込みクラス
| トークン名      | キーワード  |
|-----------------|-------------|
| `KWD_VEC` | **Vec** |
| `KWD_LIST` | **List** |
| `KWD_DICT` | **Dict** |
| `KWD_OPTION` | **Option** |
| `KWD_FUNCTION` | **Function** |

### 型修飾子
| トークン名      | キーワード  |
|-----------------|-------------|
| `KWD_REF`       | **ref**     |
| `KWD_CONST`     | **const** |

### リテラル
| トークン名      | キーワード  |
|-----------------|-------------|
| `KWD_TRUE`      | **true**    |
| `KWD_FALSE`     | **false**   |
| `KWD_NULL`      | **null**   |
| `KWD_NULLOPT`   | **nullopt**   |
| `KWD_SELF`      | **self**    |

### ステートメント
| トークン名      | キーワード  |
|-----------------|-------------|
| `KWD_VAR`       | **var** |
| `KWD_IF`        | **if** |
| `KWD_ELSE`      | **else** |
| `KWD_MATCH`     | **match** |
| `KWD_LOOP`      | **loop** |
| `KWD_FOR`       | **for** |
| `KWD_IN`        | **in** |
| `KWD_DO`        | **do** |
| `KWD_WHILE`     | **while** |
| `KWD_TRY`       | **try** |
| `KWD_CATCH`     | **catch** |
| `KWD_FINALLY`   | **finally** |
| `KWD_BREAK`     | **break** |
| `KWD_CONTINUE`  | **continue** |
| `KWD_RETURN`    | **return** |

## 関数・クラスの属性
| トークン名      | キーワード  |
|-----------------|-------------|
| `KWD_VIRTUAL` | **virtual** |
| `KWD_OVERRIDE` | **override** |

### トップレベル
| トークン名      | キーワード  |
|-----------------|-------------|
| `KWD_FN`        | **fn** |
| `KWD_ENUM`      | **enum** |
| `KWD_CLASS`     | **class** |
| `KWD_NAMESPACE` | **namespace** |

## 5. 演算子
```
SCOPE_RESOL     ::

INCLEMENT       ++
DECLEMENT       --

REF             &
DEREF           *

ADD             +
SUB             -
MUL             *
DIV             /
MOD             %

LSHIFT          <<
RSHIFT          >>

BIT_NOT         ~
BIT_AND         &
BIT_OR          |
BIT_XOR         ^

LESS            <
GREATER         >
LESS_OR_EQ      <=
GREATER_OR_EQ   >=

EQUAL           ==
NOT_EQUAL       !=

LOG_NOT         !
LOG_AND         &&
LOG_OR          ||

ASSIGN          =
ASSIGN_ADD      +=
ASSIGN_SUB      -=
ASSIGN_MUL      *=
ASSIGN_DIV      /=
ASSIGN_MOD      %=
ASSIGN_AND      &=
ASSIGN_OR       |=
ASSIGN_XOR      ^=
ASSIGN_LSHIFT   <<=
ASSIGN_RSHIFT   >>=
```
## 6. 記号
```
ELLIPSIS        ...

RIGHT_ARROW     ->
LAMBDA_BODY     =>

MATCH_CASE      LAMBDA_BODY

DOT             .
COMMA           ,
COLON           :
SEMICOLON       ;
EXCLAMATION     !
QUESTION        ?

HASH            #
DOLLER          $
BACKTICK        `
TILDE           ~

BRACKET_OPEN    (
BRACKET_CLOSE   )
SCOPE_OPEN      {
SCOPE_CLOSE     }
ANGLE_OPEN      <
ANGLE_CLOSE     >
ARRAY_OPEN      [
ARRAY_CLOSE     ]
```

--------------------------------------

--------------------------------------

# B. 構文解析 (Parser)

#### 構文の規約一覧

- **曖昧な構文の禁止**: Fire では曖昧な構文もしくはそれに準ずる機能を一切使用しません。<br>

--------------

## 1. シンボル (`symbol`)
変数や関数などの名前を参照する識別子トークン、またはスコープ解決式のこと。<br>
テンプレート引数リストを持つことができる。<br>


Syntax: (結合規則なし)<br>
```EBNF
symbol  :=  IDENT template-args? (SCOPE_RESOL symbol)?

template-args  :=
    ANGLE_OPEN type-name (COMMA type-name)* ANGLE_CLOSE
```

Example:
```
abc
a::b
func<int>
MyClass<T>::func
```

- **大小比較式との混合回避**
テンプレート引数リスト `< ... >` をパースしている最中に、リスト内の予期せぬ位置で `"<"` もしくは `">"` が現れた場合、またはそれ以外の原因でパースに失敗した場合、引数リストのパースを行いません。
引数リストのパースを中断して、パース直前のトークン位置に戻します。

--------------

## 2. 型名 (`type-name`)
基本型・組み込みクラス・ユーザー定義クラス・列挙型 のいずれかの名前を指す識別子トークン、またはスコープ解決式。<br>
テンプレート引数を指定可能。（構文はシンボルのものと同義）
**結合規則 = なし**

Syntax:
```EBNF
type-name  :=  IDENT template-args? (SCOPE_RESOL type-name)? KWD_CONST? KWD_REF?
```
Example:
```
int
Vec<string> const
MyNamespace::MyClass
```

Fire 言語では、文とそれより上層の段階では、型名を記述できる場所は完全に決められている。
そこに到達したときに限り、型名のパーサを呼び出す。

### 型を記述できる箇所の一覧:
文またはそれより上層の構文に該当する構文に含まれる、全ての「型名を記述できる場所」を、以下に定義する。
- シンボルもしくは型名のテンプレート引数の中 (`name<T, ...>`)
- 関数の引数     (`a: T`)
- 関数の戻り値   (`-T`)

### 式中に型名が含まれる場合
式中に型名が含まれている場合は、
シンボルと型名のどちらに当てはまるのかという検査や走査はせず、*シンボルとして* パースします。
`ref` または `const` キーワードの出現により、シンボルとしてのパースが失敗した場合、型名としてパースを行います。

--------------

## 3. 式

### 演算子の優先順位
### Pr. 1
**結合規則 = なし**
| 演算子     | 説明                   |
|------------|------------------------|
| **`::`**   | **スコープ解決演算子** |

### Pr. 2
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **`.`**    | **メンバアクセス**     |
| **`[]`**   | **配列インデックス**   |
| **`()`**   | **関数呼び出し**       |
| **`++`**   | **後置きインクリメント** |
| **`--`**   | **後置きデクリメント** |

### Pr. 3
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **`++`**   | **前置きインクリメント** |
| **`--`**   | **前置きデクリメント** |
| **`&`**    | **アドレス取得** |
| **`*`**    | **参照** |
| **`!`**    | **論理Not** |
| **`~`**    | **ビット反転** |
| **`+`**    | **単項プラス** |
| **`-`**    | **単項マイナス** |

### Pr. 4
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **\***     | **乗算** |
| **\/**     | **除算** |
| **\%**     | **余算** |

### Pr. 5
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **+**     | **加算** |
| **-**     | **減算** |

### Pr. 6
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **<<**     | **左シフト** |
| **>>**     | **右シフト** |

### Pr. 7
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **<**      | **より小さい** |
| **>**      | **より大きい** |
| **<=**     | **次の値以下** |
| **>=**     | **次の値以上** |

### Pr. 8
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **==**     | **等式**                 |
| **!=**     | **不等式**                 |

### Pr. 9
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **&**      | **ビット AND**             |

### Pr. 10
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **^**      | **ビット XOR**             |

### Pr. 11
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **\|**     | **ビット OR**              |

### Pr. 12
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **&&**     | **論理積**                 |

### Pr. 13
**結合規則 = 左**
| 演算子     | 説明                   |
|------------|------------------------|
| **\|\|**   | **論理和**                 |

### Pr. 14
**結合規則 = 右**
| 演算子     | 説明                   |
|------------|------------------------|
| **=**      | **代入**                   |
| **+=**     | **加算代入**               |
| **-=**     | **減算代入**               |
| **\*=**    | **乗算代入**               |
| **\%\=**   | **余算代入**               |
| **/=**     | **除算代入**               |
| **<<=**    | **左シフト代入**           |
| **>>=**    | **右シフト代入**           |
| **&=**     | **ビット AND 代入**        |
| **^=**     | **ビット XOR 代入**        |
| **\|\=**   | **ビット OR 代入**         |

### factor
構文木の終端となる要素。
```EBNF
factor  :=  KWD_TRUE | KWD_FALSE | KWD_SELF |
            KWD_NULL | KWD_NULLOPT |
            symbol | type-name | literal

literal :=  INTEGER | FLOAT | CHAR | STIRNG
```

### プライマリ
```EBNF
primary   :=
    (factor DOT)* factor |
    factor (ARRAY_OPEN expr ARRAY_CLOSE)* |
    factor (BRACKET_OPEN (expr COMMA)* expr? BRACKER_CLOSE)* |
    factor (INCLEMENT | DECLEMENT)
```

### 単項算術
```EBNF
unary     :=
    (INCLEMENT | DECLEMENT | REF | DEREF | LOG_NOT | BIT_NOT | ADD | SUB)? primary
```

### 二項算術
```EBNF
term     :=  (unary (MUL | DIV | MOD))* unary
add_sub  :=  (term (ADD | SUB))* term
```

### シフト演算
```EBNF
shift   :=  (add_sub (LSHIFT | RSHIFT))* add_sub
```

### 比較
```EBNF
compare  :=  (shift (LESS | GREATER | LESS_OR_EQ | GREATER_OR_EQ))* shift
```

### 等価比較
```EBNF
equality  :=  (compare (EQUAL | NOT_EQUAL))* compare
```

### ビット演算
```EBNF
bit_and   :=  (equality BIT_AND)* equality
bit_xor   :=  (bit_and BIT_XOR)* bit_and
bit_or    :=  (bit_xor BIT_OR)* bit_xor
```

### 論理積・論理和
```EBNF
log_and   :=  (bit_xor LOG_AND)* bit_xor
log_or    :=  (log_and LOG_OR)* log_and
```

### 代入
```EBNF
assign      :=
    log_or ((ASSIGN | ASSIGN_ADD | ASSIGN_SUB | ASSIGN_MUL | ASSIGN_DIV |
        ASSIGN_MOD | ASSIGN_AND | ASSIGN_OR | ASSIGN_XOR |
        ASSIGN_LSHIFT | ASSIGN_RSHIFT) asign)*
```

--------------

## 4. 文
```
stmt    :=  if | match |
            loop | for | while | do-while |
            return | break | continue | expr-stmt |
            scope | var
```

### 条件文
#### **if**
```EBNF
if   :=  KWD_IF <cond: expr> <then: scope>
       (KWD_ELSE (<if> | <scope>))?
```

#### **match**
```BNF
match   :=  KWD_MATCH <expr> SCOPE_OPEN
                (<expr> MATCH_CASE <scope> COMMA)*
                <expr> MATCH_CASE <scope>
            SCOPE_CLOSE
```

### 繰り返し文
#### **loop**
```
loop    :=  KWD_LOOP <scope>
```

#### **for**
```
for     :=  KWD_FOR (start: <expr> | <var>)? IDENT KWD_IN <content: expr> <scope>
```

#### **while**
```
while   :=  KWD_WHILE <cond: expr> <scope>
```

#### **do-while**
```
do-while    :=  KWD_DO <scope> KWD_WHILE <cond: expr> SEMICOLON
```

### 単文

#### **return**
```
return  :=  KWD_RETURN <expr>? SEMICOLON
```

#### **break**
```
break   :=  KWD_BREAK SEMICOLON
```

#### **continue**
```
continue    :=  KWD_CONTINUE SEMICOLON
```

#### **式文**
```
expr-stmt   :=  <expr> SEMICOLON
```

### スコープ
```
scope   :=  SCOPE_OPEN <stmt>* SCOPE_CLOSE
```

### 変数定義 (`var`)
現在のスコープに変数を定義する。すでに定義済みの場合は、シャドウイングをする。<br>
型名と初期化式を両方省略することは不可能。<br>
Syntax:
```
var     :=  KWD_VAR IDENT (COLON <type-name>)? (ASSIGN <expr>)? SEMICOLON
```

Example:
```EBNF
var a = 10;
var b : int;
var c : string = "aiueo";
```

### 例外処理 (`try-catch`)
Fire では例外処理を使うことができます。<br>
`catch` 文では、全ての例外を受け取りたいときは C++ と同様に省略記号 `...` を使用できます。<br>
Syntax:
```EBNF
try-catch   :=
    <try-scope>
    <catch-scope>+
    <finally-scope>

try-scope   :=
    KWD_TRY <scope>

catch-scope   :=
    KWD_CATCH (IDENT COLON <type-name> | ELLIPSIS) <scope>

finally-scope   :=
    KWD_FINALLY <scope>
```
Example:
```
try {
    println(input().split(' ')[3]);
}
catch e: IndexOutOfRange {
    ...
}
```

--------------

## 5. 関数

### 定義
Syntax:
```EBNF
func    :=
    KWD_FN <func-args> (KWD_RIGHT_ARROW <type-name>)? <body: scope>

func-args   :=
    BRACKET_OPEN (IDENT COLON <type-name> COMMA)* (IDENT COLON <type-name>)? BRACKET_CLOSE
```

### テンプレート


------------

## 6. 列挙型

------------

## 7. クラス

------------

## 8. 名前空間

--------------------------------------

--------------------------------------

# C. 型システム

TODO

--------------------------------------

--------------------------------------

# D. 意味解析

各フェーズごとに分けてプログラムの意味解析を行う。
構文木を探索して処理することは全てのフェーズに共通する。

## 1. スコープ情報の構築

Node を探索し、スコープを持つ Node を元に スコープ情報 (=SI) を構築する。
すべての S は、シンボルテーブル(=SymTBL) を保持する。
また、その S に名前がある場合、S の中にシンボル情報(=SYM) をもたせる。
```cpp
struct ScopeInfo {

}
```

## 2. シンボル収集
シンボルの定義情報を収集してシンボルテーブルを作成する。

## 3. 名前解決


## 4. 整合性・型チェック

------------

# E. 構文木の評価

------------

# F. コンパイル: LLVM-IR を生成

------------

# G. アプリケーション

------------

# H. REPL
