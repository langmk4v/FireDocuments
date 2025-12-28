# Fire 言語仕様書
**リファレンス実装のための詳細かつ具体的な仕様書**

2026 ringo.

> **目次**
>   - [A. 字句解析 (Lexer)]()
>       - [1. 数値リテラル]()
>       - [2. 文字・文字列リテラル]()
>       - [3. 名前 (識別子)]()
>       - [4. 演算子]()
>       - [5. 記号]()
>   - [B. 構文解析 (Parser)]()
>       - [1. 式]()
>           - [演算子の優先順位]()
>           - [factor: リテラル・識別子]()
>           - [primary: 関数呼び出し・配列添字・メンバアクセス]()
>           - [unary: 単項算術演算]()
>           - [terms: 二項算術演算]()
>           - [shift: シフト演算]()
>           - [compare: 比較]()
>           - [equality: 等価比較]()
>           - [bit: ビット演算]()
>           - [log: ブール型比較]()
>           - [assign: 代入]()
>           - [expr: 式]()
>       - [2. 文]()
>           - [条件文]()
>               - [if]()
>               - [match]()
>               - [switch]()
>           - [繰り返し文]()
>               - [for]()
>               - [loop]()
>               - [do-while]()
>               - [while]()
>           - [単文]()
>               - [let]()
>               - [try-catch]() 
>               - [return]()
>               - [break]()
>               - [continue]()
>           - [式文]()
>           - [スコープ]()
>       - [3. 関数]()
>       - [4. 列挙型]()
>       - [5. クラス]()
>       - [6. 名前空間]()
>   - [C. 型システム (TypeInfo)]()
>       - [1. ]()
>   - [D. 意味解析 (Sema)]()
>       - [1. スコープ情報]()
>       - [2. シンボル収集]()
>       - [4. 名前解決]()
>       - [5. 整合性・型チェック]()
>   - [E. 構文木の評価 (EvalNode)]()
>       - [1. スタック]()
>       - [2. 式の評価]()
>       - [3. 文の実行]()
>   - [F. コンパイル (LLVM-IR)]()
>       - Feature...
>   - [G. アプリケーション (Driver)]()
>       - [1. ソースファイル管理]()
>           - [ファイル読み取り]()
>           - [外部モジュール取り込み (import)]()
>       - [2. コマンドライン引数]()
>   - [H. コマンドライン実行 (REPL)]()




------------

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
IDENTIFIER  :=  (_|[a-z]|[A-Z])([a-z]|[A-Z]|[0-9])*
```
## 4. 演算子
```
SCOPE_RESOL     ::

ADD             +
SUB             -
MUL             *
DIV             /
MOD             %

LSHIFT          <<
RSHIFT          >>

BIT_AND         &
BIT_OR          |
BIT_XOR         ^

INCLEMENT       ++
DECLEMENT       --

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
## 5. 記号
```
RIGHT_ARROW     ->
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

------------

# B. 構文解析

## 1. 式
### 演算子の優先順位

優先度 1
| 演算子 | 名称               | 結合 |
|--------|--------------------|------|
| `::`   | スコープ解決演算子 | 左   |

優先度 1
| 演算子 | 名称               | 結合 |
|--------|--------------------|------|
| `()`   | 関数呼び出し       | 左   |
| `[]`   | 配列インデックス   | 左   |




### 終端
- リテラル
- 識別子

### プライマリ
### 単項算術
### 二項算術
### シフト演算
### 比較
### 等価比較
### ビット演算

## 2. 文

### 2-1. Conditional-Stmt
- `if`
```
if ::=
    "if" <cond: expr>
    <then-code: scope>
    ("else" (<if> | <scope>))?
```
- `for`
```
```
- `for`
```
```
- `for`
```
```


## 2-2. Loop-Stmt
- `loop`
```
```
- `for`
```
```
- `foreach`
```
```
- `while`
```
```
- `do-while`
```
```

## Try-catch
```
try-catch   ::=
    <try-scope>
    <catch-scope> +
    <finally-scope>

try-scope   ::=
    "try" <scope>

catch-scope   ::=
    "catch" 
```


------------

# C. 型システム

TODO

------------

# D. 意味解析

## 

------------

# E. 構文木の評価

------------

# F. コンパイル: LLVM-IR を生成

------------

# G. アプリケーション

------------

# H. REPL
