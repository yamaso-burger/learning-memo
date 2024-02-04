# 3 レイヤーデザインの考え方
コードを以下の 3 つの概念に沿って切り分けることで管理する
- Presentation (表示)
- Business (データの処理)
- Data Access (データの取得、更新)

## Presentation
- Controller クラスがこれにあたる
- Model を管理して、View を表示させる役割
## Business
- Service クラスがこれにあたる
- Presentation のために、データの処理を行う役割
## Data Access
- Repository クラスがこれにあたる
- Business が処理を行うために必要なデータを取得、更新する役割