---
title: 02_UMLユースケース図_PlantUML
tags:
  - etrobocon
  - 要求モデル
  - アプライドクラス
  - ユースケース
  - PlantUML
---
# ユースケース図 PlantUML ソースコード

UMLの厳密な定義（アクターの一本化）および Cockburn の Sea levelポリシーに準拠した、ETラリー攻略におけるユースケース図の PlantUML ソースコードです。このコードを PlantUML レンダラー（VS Code 拡張機能やオンラインエディタ等）に入力することで、ダイアグラム画像を出力できます。

## 1. PlantUML コード

```plantuml
@startuml
left to right direction
skinparam packageStyle rect
skinparam shadowing false
skinparam monochrome true

' スタイルの微調整 (プレミアムデザインに調和するシンプルなモノトーン)
skinparam usecase {
    BackgroundColor white
    BorderColor black
    ArrowColor black
}
skinparam actor {
    BackgroundColor white
    BorderColor black
}

' アクターの定義 (唯一の主アクター)
actor "競技者\n(Contestant)" as Contestant

' システム境界とユースケースの定義
rectangle "SOROT2026連携システム\n(System Boundary)" {
    usecase "UC-200: ETラリーを攻略する" as UC200
    usecase "UC-201: ゲート位置を特定する" as UC201
    usecase "UC-202: ゲート通過経路を計画する" as UC202
    usecase "UC-203: ゲートを順次通過する" as UC203
}

' アソシエーション (主アクターから最上位UCへの一本化)
Contestant -- UC200

' ユースケース間の関係性 (Cockburnポリシーの徹底)
UC200 ..> UC201 : <<include>>
UC200 ..> UC202 : <<include>>
UC200 ..> UC203 : <<include>>

@enduml
```

## 2. 関係性の解説

1. **アソシエーションの一本化 (`Contestant -- UC200`)**  
   主アクターである「競技者」からのアソシエーションは、唯一のユーザー目標レベルである `UC-200` (ETラリーを攻略する) のみに伸びています。部分的な手段に過ぎない Fish level（UC-201〜203）への直接の矢印を排除することで、UMLの論理的整合性を守り、図の可読性を劇的に高めています。

2. **必須機能の包含関係 (`UC200 ..> UC203`)**  
   ETラリーを攻略するためには、ゲートを順次通過して周回走行することが必須であるため、`UC-200` が `UC-203` を包含 (`<<include>>`) します。

3. **包含関係による基本フローの構成 (`UC200 ..> UC201`, `UC200 ..> UC202`)**  
   スタート後のヒント特定（`UC-201`）および経路計画（`UC-202`）は、システムがスタートした直後に標準の基本フロー（ハッピーパス）として無条件で自動実行を試みる主要なサブ目標です。そのため、これらは包含関係 (`<<include>>`) として最上位目標 `UC-200` から矢印を向けています。ヒント特定や経路計画の「失敗やスキップ」というオプショナル（任意）な側面は、ユースケース記述の「代替フロー」にて自律的カラー追従走行へのフォールバックとして美しく処理することで、図の不必要な複雑化（拡張点 Extension Point の定義等）を避けています。
