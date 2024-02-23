# SQL Databases

## H2
H2 はアプリケーション内のメモリ（in memory）のデータベースマネジメントシステム

### メリット
軽量なシステムで、処理が速く、プロトタイプの段階で用いるのには最適

### デメリット
一度アプリケーションをリスタートすると、その間に蓄積されたデータが消える（データの揮発性）

### Getting started H2
2 つの dependency を追加する
- [H2 Database Engine   ](https://mvnrepository.com/artifact/com.h2database/h2)
- [Spring Boot Starter JPA](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-jpa/3.2.2)

#### Application properties の設定
`application.properties`ファイルに H2 の設定を追加する
例
```
// ユーザインターフェースの有効化
spring.h2.console.enabled=true
// UI アクセス時のパス
spring.h2.console.path=/h2
// データソースアクセスのパス
spring.datasource.url=jdbc:h2:mem:grade-submission
```

## Spring Boot Data JPA
JPA ... Java Persistence API

Spring Boot と　データベースの間を取り持ってくれる API
これを用いることで、データベース操作のために SQL を書く必要がなくなる。

### エンティティの設定
POJO クラスをデータベースに登録するエンティティとして設定することができる
- `@Entity`: POJO クラスに設定する Spring Boot 起動時にエンティティとして認識されて、データベースでテーブルが自動生成される
- `@Table`: name 属性を指定することでテーブル名を指定できる
- `@Column`: フィールドに付与できる。付与することでデータベース上のカラムとして扱うことができる
- `@Id`: フィールドに付与することで、それをプライマリキーとして登録できる
- `@GeneratedValue`: フィールドに付与することで、自動生成される方法を設定できる

例
```java
@Entity
@Table(name = "student")
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id;
    @Column(name = "name", nullable = false)
    private String name;
    @Column(name = "birth_date", nullable = false)
    private LocalDate birthDate;
    // Constructor, getter, setter, etc...
}
```

### CrudRepository
Spring Boot JPA が提供する、データベースへのエンティティの CRUD 処理を行うために用意されているリポジトリインターフェース。
継承して用いる。

```java
public interface StudentRepository extends CrudRepository<Student, Long> {
}
```
#### なぜ interface?
Spring Boot JPA がバックグラウンドで自動でインターフェースを implement した Impl クラスを runtime で生成してくれるため

