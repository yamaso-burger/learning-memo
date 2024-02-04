# Front-end と Backend の切り分け
Thymeleaf + SpringBoot のように Front-end と Backend を同じサーバー(もしくは port)に設置することは現在では時代遅れのデザイン

なぜなら...
- 全ての端末でそれらが使えるとは限らないため

Front-end と Backend を切り分けることで
- PC の場合
    - React + SpringBoot
- スマホの場合
    - iOS
        - Swift + SpringBoot
    - Android OS
        - Kotlin + SpringBoot

などと Backend でのビジネスロジックはそのままに、さまざまなプラットフォームに対応させることが可能