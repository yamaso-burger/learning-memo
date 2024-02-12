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
```java
// ポート番号 8080 でリクエストを受け付けた場合にアノテーションを付与したクラスを依存性注入する
@ConditionalOnProperty(name = "server.port", havingValue = "8080")
```

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
    - 引数に付与することで、リクエストが投げられた時にその引数を初期化した後、対応するパラメータをセットすることができる
- `@PathVariable`
    - 引数に付与することで、`@GetMapping`において`{}`で囲って定義した変数の値を取得できる
    - すなわちパスパラメータを取ってこれる
- `@RestController`
    - `@Controller` + `@ResponseBody`を組み合わせたもの
    - 付与したクラスは Bean として Spring Container に保持される
    - 付与されたクラスのメソッドは値を JSON に変換して返すことができる
    ― ただし、引数として JSON を受け取る場合は個別で付与する必要がある
- `ResponseEntity<>`
    - この形式で値を返すと、HTTPステータスを指定できる

### POST API
GET API では使わない要素を書き下す
```java
@PostMapping("/contact")
    public ResponseEntity<HttpStatus> createContact(@RequestBody Contact contact) {
        contactService.saveContact(contact);
        return new ResponseEntity<>(HttpStatus.CREATED);
    } 
```
- `@PostMapping`
    - メソッドに付与することで、その処理を指定したパスパラメータに対する POST メソッドのリクエスト時に実行することができる
- `@RequestBody`
    - HTTP リクエストボディの JSON を deserialize して引数とすることができる
```json
{
    "name": "Jon Snow",
    "phoneNumber": "41827539"
}
```
```java
contact.getName() // "Jon Snow"
contact.getName() // "41827539"
```

### PUT API, DELETE API
- `@PutMapping`
    - PUT リクエストに対する処理メソッドに付与する

## エラーハンドリング
### RuntimeException
RuntimeException は Exception クラスを継承したものであり、他の例外と異なって `try-catch`の処理を記述する必要がない

### GlobalHandler
グローバルハンドラーを定義することで、アプリケーション内で throw されたエラーを一括で処理することができる。
- `@ControllerAdvice`
    - クラスに付与することで、グローバルハンドラーとして Spring Container にクラスを保持する
- `@ExceptionHandler`
    - メソッドに付与することで、アノテーション引数に指定した Exception クラスが throw されたときに記述された処理を実行する。

### Validation を用いたエラーハンドリング
- `@Valid`
    - Controllerクラスの引数に設定することで、Pojoクラスに設定した Validation に基づいて Exception を投げるようになる
- `ResponseEntityExceptionHandler`クラス
    - Validation によって投げられる例外クラス。
    - バリデーションエラーハンドリングをする場合はグローバルハンドラで継承する
    - `handleMethodArgumentNotValid`メソッド
        - オーバーライドしてバリデーションエラー時の処理を記述する
例
```java
@ControllerAdvice
public class ApplicationExceptionHandler extends ResponseEntityExceptionHandler {

    @Override
    protected ResponseEntity<Object> handleMethodArgumentNotValid(MethodArgumentNotValidException ex,
            HttpHeaders headers, HttpStatus status, WebRequest request) {

        List<String> errors = new ArrayList<>();
        ex.getBindingResult().getAllErrors().forEach(error -> errors.add(error.getDefaultMessage()));
        return new ResponseEntity<>(new ErrorResponse(errors), HttpStatus.BAD_REQUEST);
    }
}

```

## TIPS
- `@JsonFormat`
    - Date, LocalDateTime クラスなどのフォーマットした値を HTTP レスポンスとして返すことができる