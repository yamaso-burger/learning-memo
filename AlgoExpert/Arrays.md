# Arrays TIPS
## 目的
- 配列をソートしたい
- 配列を標準出力に表示する

## 結論

```java
int[] array = new int[]{3, -1, 5, -6, 12, 8}
// array の中身が直接ソートされる
Arrays.sort(array);

// [-6, -1, 3, 5, 8, 12]
System.out.println(Arrays.toString(array));
```