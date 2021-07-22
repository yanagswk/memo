- vscodeでhtmlファイルを開いて「!」を入力してenterをおすとhtmlのテンプレートが出力される。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>

</body>
</html>

```


- VSCode スニペットの追加
`Code(File)->Preferences(基本設定)->User Snippets(ユーザースペニット)->html` へ移動
以下、追記。
```json
"php": {
    "prefix": "php",
    "body": [
        "<?php $1 ?>"
    ],
    "description": "php tag"
}
```  

# 設定  
- trim : 空白設定

# vscode拡張機能
