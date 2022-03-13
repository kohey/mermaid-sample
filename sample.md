```mermaid
sequenceDiagram
  actor クライアント
  participant API as API Gateway<br/>Rest API
  participant Lambda as Data Post<br/>Lambda Function
  participant DataTable as Amazon DynamoDB<br/>Data Table
  participant SES as Amazon SES
```