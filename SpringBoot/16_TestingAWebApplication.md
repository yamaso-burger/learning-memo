# Unit Testing
ユニットテストを行うときは、loosely coupling である必要がある。
→ 例えば Service クラスを動作させるときを考える
```java
public ExampleService {
    @Autowired
    ExampleRepository exampleRepository;

    ...
}
```
通常であれば、コンポーネントをスキャンして `ExampleRepository` が Spring Container から注入されるが、テストの時はそれが行われず null になる。
しかし、loosely coupling の状態であれば、`@Autowired` が付与されているものに対して Mock を注入することができる。

## Unit Test に用いる Mockモジュール

**Mockito**
テスト対象外で loose coupling しているクラスを Mock として扱うことができる。

## Unit Test に用いるアノテーション
- `@Runwith`
    - テストクラスに付与するアノテーション
    - 何を使ってテストを実行するか
    - MockitoJUnitRunner を使う
- `@Mock`
    - モックとなるクラスに付与するアノテーション
    - これに指定されたクラスは実際の機能を持たない
- `@InjectMocks`
    - これに指定されたクラスはテストクラス外でオブジェクトを生成する
    ― Inject Mock は実際の機能を持つ

- `@Test`
    - テスト対象のメソッドのに付与するアノテーション
    - これにより、実行時にそのメソッドが実行される

## Unit Test の書き方
3 段階に分かれる
- Arrange: テストを実行するのに必要な値の準備
- Act: テスト対象のメソッドを呼び出す
- Assert: メソッドが期待通りの動作をしているかチェックする

### Arrange
モックから返す値を設定したりする。
▼ Mockito の使い方
when(モックのメソッド).then(返す値)

### Assert
- assertEquals(expected, actual)
Act で Inject Mock から得た値を期待値を比較して、テストの成否を判断する。
- verify(mock, times).method(argument)
指定の Mock のメソッドが何回呼ばれているかをチェックする

***
# Integration Testing
ユニットテスト時とは違い、全体の流れ（lifecycle）としてリクエストに対して期待するレスポンスが返されているかテストする
- Spring Context、Spring Container が必要になる

## Integration Testing に用いる Mock モジュール

### **MockMVC**
- リクエストビルダーを用いてリクエストを作成する
- MockMvc インスタンスからリクエストを投げて、レスポンスが期待通りか評価する
```java
// 例
@Autowired
private MockMvc mockMvc;
@Test
public void testSuccessfulSubmission() throws Exception {
    RequestBuilder request = MockMvcRequestBuilders.post("/submitForm")
        .param("name", "Harry")
        .param("subject", "Potions")
        .param("score", "C-");

    mockMvc.perform(request)
        .andExpect(status().is3xxRedirection())
        .andExpect(redirectedUrl("/grades"));
}
```

- `MockMvcRequestBuilders.post(path)
        .param(name, value)`でリクエストボディとして値をセットすることができる
- `MockMvcRequestBuilders.post(path)
        .flashAttr(name, object)`でフラッシュアトリビュート（リダイレクト時にメソッドで渡すもの）として値をセットできる

### MockMvc メソッド覚書
- andExpect メソッドの中身
    - `status().isOK()`: HTTPステータスが200かどうか
    - `content().contentType(MediaType.APPLICATION_JSON)`: 返されたデータが JSON
 形式であるかどうか
    ― `jsonPath("$.name").value("Jon Snow")`: 返された JSON の値が期待値通りかどうか

#### jsonPath
返された JSON の値を検証できる
例：JSON の中身に期待したデータが存在するかどうか
```java

```

## 利用するアノテーション
- `@SpringBootTest`
Integration Test を実行するクラスに付与する

- `@AutoConfigureMockMvc`
    - Integration Test 実行クラスに付与する
    - 付与することで Spring Container に MockMvc を保持できる

# TIPS

## import と import static の違い
[参考](https://workteria.forward-soft.co.jp/blog/detail/10093)
- import
    - クラスやインターフェースをインポートしてインスタンスを生成したり、メソッドを利用可能にする
- import static
    - クラスやインターフェースの特定の static メソッドをインポートして利用可能にする

## SpringBoot テストについてより詳しく
[参考](https://meetup-jp.toast.com/452)
[参考](https://qiita.com/a-pompom/items/3f834119c756e5286730)
`@SpringBootTest`アノテーションを用いることで Application Context つまり、実際にアプリを立ち上げた時と同じようにユニットテストを実行することができる。

- 前述した Unit Testing
```java
@RunWith(MockitoJUnitRunner.class)
public class GradeServiceTest {
    
    @Mock
    private GradeRepository gradeRepository;

    @InjectMocks
    private GradeService gradeService;

    @Test
    public void contextLoads() {
        // Mock が注入されている
        // 実態は機能を持たない
        assertNotNull(gradeService);
        // テストクラスと別で生成したインスタンス
        // Bean 管理されていないため、フィールドに持つ、GradeRepository は注入されていない
        assertNotNull(gradeRepository);
    }
```
- `@SpringBootTest`アノテーションを用いた場合
```java
@SpringBootTest
@RunWith(SpringRunner.class)
public class GradeServiceTest {
    
    @Autowired
    private GradeRepository gradeRepository;

    @Autowired
    private GradeService gradeService;

    @Test
    public void contextLoads() {
        // 双方とも Apllication Context のインスタンスで Bean 管理されている
        assertNotNull(gradeService);
        assertNotNull(gradeRepository);
    }
```