# Bean と D.I. 概要
## Bean とは
Spring が管理できるオブジェクト

## Dependency Injection

### Tight coupling
クラスの中で Dependency となるクラス（これがないと動作しない）を初期化する方法
```Java
public class exampleController {
    ExampleService exampleService = new ExampleService();
    ...
}
```
- ユニットテストができなくなるという特徴を持つ


### Dependency Injection（依存性注入）の仕組み

- `@Component`アノテーションを付与することで Spring Container にクラスを保存できる

- Dependency となるクラスを宣言するとき、`@Autowire`アノテーションを付与することで Spring Container からオブジェクトが注入される

# TIPS
## Bean にするアノテーション
- `@Component`
- `@Service`
- `@Repository`
基本 3 つに違いはないが、ベストプラクティスとしてサービスクラス、リポジトリクラスにはそれぞれ専用のアノテーションを付与する

## Bean を注入するアノテーション
- `@Autowired`

`@Autowired`　は、付与したクラスやコンストラクターの引数が Spring Container に存在する場合、アプリケーションを起動したときに自動でそれらを初期化する役割をもつ

### アノテーションが必要ない例

コンストラクターを定義した場合は、アノテーションを付けずとも自動で Bean は注入される

## Spring Container の設定するアノテーション
- `@Bean`
`@Configuration`を付与したクラスで設定することで、あるクラスの依存性注入に対する処理を定義できる

```java
@Configuration
public class AppConfig {

    @Bean
    public GradeRepository gradeRepository() {
        return new GradeRepository();
    }
    
}
```

```java
public class gradeService {
    @Autowired
    GradeRepository gradeRepository;
}
```

上記だと、GradeRepositoryクラスに`@Component`または`@Repository`を付与したときと挙動が同じ
→ 基本はこちらを使わなくてよい

# XML での Dependency Injection

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">


	<bean id="repository" class="com.ltp.gradesubmission.repository.GradeRepository"> </bean>


	<bean id="service" class="com.ltp.gradesubmission.service.GradeService"> </bean>
</beans>
```

Bean を定義したファイルを `@Configuration` アノテーションを付与したクラスに `@ImportResouce` アノテーションでインポートすることで、DI することができる。
