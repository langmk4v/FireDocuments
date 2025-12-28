# Fire 言語仕様書
リファレンス実装のための詳細かつ具体的な仕様書。

> 目次
> - [A. 字句 (Token)]()
> - [B. 構文 (Node)]()
> - [C. データ型]()
> - [D. オブジェクト]()
> - [E. ]()
> - [F. ]()
> - [G. ]()
> - [H. ]()




# A. 字句



# B. 構文

## 1. 式 (Expr)
### 1-1. 演算子の優先順位
```

```

## 2. 文 (Stmt)

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


# 変数



























-------------------
