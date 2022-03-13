```mermaid
sequenceDiagram
  actor クライアント
  participant API as API Gateway<br/>Rest API
  participant Lambda as Data Post<br/>Lambda Function
  participant DataTable as Amazon DynamoDB<br/>Data Table
  participant SES as Amazon SES

  %% -->>, ->>, +, - を区別すること
  クライアント ->>+ API: データ登録
  API ->>+ Lambda: データ登録
  Lambda ->> Lambda: バリデーション
  alt バリデーションエラー
    Lambda ->> API: バリデーションエラー
    API ->> クライアント: 400エラー
  end
  note over API, SES: 登録とメール送信の完了をもって200すること
  Lambda ->>+ DataTable: データ登録
  note left of DataTable: insert
  alt 存在する場合
    Lambda -->> SES: メール依頼はしない
  end
  DataTable -->>- Lambda: 登録OK
  note right of SES: queueに詰める数を調整しておく
  Lambda -->>+ SES: メール送信依頼
  SES -->> Lambda: メール受付OK
  SES -->>- クライアント: メール送信OK
  Lambda -->>-  API: 登録OK
  API -->>- API: 200 OK
```