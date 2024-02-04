# REST API
- API...利用者とコンテンツの仲介役
- REST API ざっくり
    - リクエストに応じてデータに CRUD の操作ができる
    - リクエストを受け取る URI を決められる
    - JSON でデータを受け渡しする

## インターフェースの Implementation
Service layer において、インターフェースを implements することで、変更に柔軟な構造にすることができる。

例
- Controller Class
```java
@Controller
public ExampleController {
    
    @Autowired
    ExampleService exampleService
}
```
- Service Interface
```java
@Service
public ExampleService {
    
}
```
- Service Implementation Class
```java
@Service
public ExampleServiceImpl implements ExampleService {
    // ExampleServiceImpl 独自の処理
}
```
このとき、Spring Container には、ポリモーフィズムによってExampleSerivceImpl クラスが ExampleService の Bean として保持される

ここで実装を変えたくなった時、新たに Service Implementation Class を実装する
- Service Implementation Class 2
```java
@Service
public NewExampleServiceImpl implements ExampleService {
    // NewExampleServiceImpl 独自の処理
}
```
こうすることで、環境によって Bean 登録されるクラスを柔軟に変更することができる

### Bean 登録の制御に用いるアノテーション
- `@ConditionalProperty`
    - 例えば、値としてポート番号を指定することで、受け付けているポートによって登録する Bean を制御できる

## TIPS
### GET API
Controller実装の例
```java
@GetMapping("/contact/{id}")
    @ResponseBody
    public Contact getMethodName(@PathVariable String id) {
        return new Contact("000", "Mock Response", "00000000");
    }
```

- `@ResponseBody`アノテーション
    - メソッドに付与することで値を JSON に変換して返すことができる
- `@PathVariable`
    - 引数に付与することで、`@GetMapping`において`{}`で囲って定義した変数の値を取得できる
    - すなわちパスパラメータを取ってこれる
- `@RestController`
    - `@Controller` + `@ResponseBody`を組み合わせたもの
    - 付与したクラスは Bean として Spring Container に保持される
    - 付与されたクラスのメソッドは値を JSON に変換して返すことができる
- `ResponseEntity<>`
    - この形式で値を返すと、HTTPステータスを指定できる