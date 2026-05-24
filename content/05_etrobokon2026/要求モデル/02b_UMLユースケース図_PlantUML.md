---
title: 02b_UMLユースケース図_PlantUML
tags:
  - etrobocon
  - 要求モデル
  - uml
  - ユースケース
  - plantuml
  - アプライドクラス
---

# UMLユースケース図（PlantUML）

構成仕様 [[02_UMLユースケース図]] に基づくPlantUMLソース。

---

```plantuml
@startuml ETラリー攻略システム
left to right direction

skinparam actorStyle awesome
skinparam usecase {
  BackgroundColor White
  BorderColor #444444
  FontSize 10
}
skinparam actor {
  BorderColor #444444
  FontSize 13
}
skinparam ArrowColor #444444
skinparam NoteBackgroundColor #FFFDE7
skinparam NoteBorderColor #AAAAAA

actor "競技者" as Competitor

rectangle "ETラリー攻略システム" {
  usecase "UC-1: ETラリーで高得点を獲得する" as UC1
  usecase "UC-3: ゲートの色と位置を把握する" as UC3
  usecase "UC-4: 計画した経路でゲートを通過する" as UC4

  UC1 ..> UC3 : <<include>>
  UC1 ..> UC4 : <<include>>
}

Competitor --> UC1

@enduml
```

---

## 描画チェックリスト

- [ ] UC-1・UC-3・UC-4 すべて出現
- [ ] include 矢印の向きが正しい（include元 → include先）
- [ ] 競技者と UC-3・UC-4 を直接接続していない（UC-1 経由）
- [ ] 走行体・無線通信デバイスをアクターとして描いていない
- [ ] システム境界「ETラリー攻略システム」のラベルが付いている
- [ ] Fish level の UC名が出現していない

> 構成仕様の詳細は [[02_UMLユースケース図]] 参照
