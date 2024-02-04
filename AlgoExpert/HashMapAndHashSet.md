# HashMap と HashSet の違い
ハッシュテーブルを使いたいと思ったときにこの 2 つを混同したのでメモ

## 目的
値がハッシュテーブルに含まれている場合は true、含まれていない場合は false を返したい

## 結論
HashSet を使う

## HashSet
初期化
```java
Set<Integer> hashSet = new HashSet<>();
```
`contains()`メソッドを用いて、テーブルに値が入っているかを判定できる

例：
```java
hashSet.add(1);
// true
System.out.println(hashSet.contains(1));
// false
System.out.println(hashSet.contains(2));

```

## HashMap
初期化
```java
Map<Integer, Boolean>() hashMap = new HashMap<>();
```
**Map**なので、キー＆バリューのペアを持っている
→ `HashSet.contains()` のようなメソッドがないため目的には適していない

ちなみに、`HashMap.get()`をすると値がないとき、`null`を返す
```java
hashSet.put(1, true);
// true
System.out.println(hashMap.get(1));
// null
System.out.println(hashMap.get(2));
```