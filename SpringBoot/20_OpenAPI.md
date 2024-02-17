# OpenAPI
- REST API の機能を定義する標準規格
- 自動でドキュメントを生成できるツール

## 基本機能
- ローカルでアプリを立ち上げて`{RootPath}/v3/api-docs`にアクセスすることで自動生成されたアプリの OpenAPI 仕様書が JSON で取得できる
- `{RootPath}/swagger-ui/index.html`にアクセスすることで OpenAPI 仕様書を UI を介して閲覧することができる

## AppConfig で OpenAPI ドキュメントを書き換える
OpenAPI を Bean として Spring Container で保持するとき、設定値を変更することができる

Config ファイルを用いる。
```java
@Configuration
public class OpenApiConfig {

    @Bean
    OpenAPI openApi() {
        return new OpenAPI()
            .info(new Info()
            .title("Contact API")
            .description("An API that can manage contacts")
            .version("v1.0"));
    }
}
```

## Controller のメソッドに API 仕様を記述する
Controller のクラスやメソッドに OpenAPI 定義を追記することで仕様書を記述することができる
- `@Tag()`
    - `name`属性などをしていして、API のグループ分けができる
- `@Operation`
    - API の概要、説明などを記述することができる
- `@ApiResponse`
    - API レスポンスの値、説明、HTTPステータスなどを記述することができる
- `produces`
    - `@RequestMapping`アノテーションの属性として指定することでレスポンスのデータ形式を記述することができる