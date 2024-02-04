# 参照情報
- [validation ライブラリ（maven repository）](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-validation)
- 

# TIPS
- dependency 追加時、バージョンは spring-boot-starter の親要素に記載されているから、書かなくてよい

``` xml
    <parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.6.7</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>
	...
	<dependencies>
        ...

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-validation</artifactId>
		</dependency>

	</dependencies>
```

## Validation ライブラリ
### @NotBlank
クラスのフィールドに付与する。以下の効果がある。
- フィールドが空の値の時にハンドラメソッドで BindingResult.hasErrors を true にする
- デフォルトまたは設定した message を `th:error=${object.field}` として表示できる

### @Valid
ハンドラメソッドの引数に付与する。以下の効果がある
- クラスのフィールドに付与した Validation の結果を BindingResult に格納する

### BindingResult
ハンドラクラスで利用する。Validation での用途は以下
- `BindingResult.hasErrors()` の true/false によって遷移先を制御することができる

### @Valid と BindingResult
引数として、`@Valid`アノテーションを付与したオブジェクトと`BindingResult`クラスを扱うときの注意点は以下である。
- 必ず`@Valid`オブジェクトの後に`BindingResult`クラスの引数をとること
    * さもなければ、Exception が発生する
``` java
//良い例
public String handleSubmit(@Valid Item item, BindingResult result, RedirectAttributes redirectAttributes)

//悪い例
public String handleSubmit(@Valid Item item, RedirectAttributes redirectAttributes, BindingResult result)
        
```

### 複数のフィールドを用いた Validation
例えば...
```html
<form method="post" th:object th:action="">

```

### 独自 Validator
実装手順は以下の通り
1. アノテーションインターフェース（呼び名違うかも）を実装
2. Validator クラスを実装

#### アノテーションインターフェース実装
- これを実装することによって任意の方法でフィールドを Validate するアノテーションを付与できるようになる
- 但し、Validate の処理自体は Validator クラスに実装して、それを参照する
```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = ScoreValidator.class)
public @interface Score {
    String message() default "Invalid Data";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};
}
```
- `@Target` : Validation 対象のタイプ
- `@Retention` : Validation をどのタイミングでするのか
- `@Constraint` : Validate の処理をどこから参照するのか
- インターフェース中身：最低限設定が必要な項目（詳しくは調べていない）

### Validator クラス実装
- アノテーションクラスから参照することで、そのアノテーションに任意の Validate 処理を付加できる
```java
public class ScoreValidator implements ConstraintValidator<Score, String> {
    
    List<String> scores = Arrays.asList(
        "A+", "A", "A-",
        "B+", "B", "B-",
        "C+", "C", "C-",
        "D+", "D", "D-",
        "F"
    );
    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        for (String string: scores) {
            if (value.equals(string)) return true;
        }
        return false;
    }
}
```
- `ConstraintValidator<?, ?>`: 第一引数に Validate 対象のクラス、第二引数に Validate に使う値の型を指定する
- `isValid()`: オーバーライドすることで、Validate 処理を付加できる

## VS Code
- プレビュー：`Ctrl`+`Shift`+`V`
- インデント整列：`Shift`+`Alt`+`F`

# 用語
- POJO : Plain Old Java Object
