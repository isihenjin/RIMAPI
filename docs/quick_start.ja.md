# クイックスタートガイド

RIMAPIへようこそ！このガイドでは、Modのインストールから設定、最初のAPIコールまでを5分で行う方法を紹介します。

## 1. 前提条件

- RimWorldバージョン1.5以降。
- **[Harmony](https://steamcommunity.com/workshop/filedetails/?id=2009463077)** Modがインストールされ、有効化されていること。
- REST APIの基本的な知識があること。

## 2. インストール

このModは、Steam Workshop経由または手動でインストールできます。

### 方法1：Steam Workshop（推奨）

1.  Steam Workshopで **[RIMAPI](https://steamcommunity.com/sharedfiles/filedetails/?id=3593423732)** をサブスクライブします。
2.  RimWorldを起動し、`Mods`メニューに移動します。
3.  **RIMAPI** Modを有効化します。
4.  RIMAPIがHarmonyの**後**に読み込まれるよう順序を確認してください。

### 方法2：手動インストール

1.  [GitHub Releases](https://github.com/IlyaChichkov/RIMAPI/releases) ページから最新のリリースをダウンロードします。
2.  ZIPファイルをRimWorldの `Mods` フォルダに解凍します。
3.  RimWorldを起動し、`Mods`メニューでModを有効化します。

## 3. 設定

インストール後、APIサーバーのポートを設定できます。

1.  RimWorldのメインメニューで `オプション` > `MODオプション` に移動します。
2.  Modのリストから `RIMAPI` を選択します。
3.  ここで **REST サーバーポート**（デフォルトは `8765`）を変更できます。

APIサーバーはコロニーの読み込み後、自動的に起動します。

## 4. 最初のAPIコール

Modが起動し、コロニーが読み込まれたら、APIを操作できます。以下の例では現在のゲームの状態を確認します。

=== "Bash (cURL)"

    ```bash
    curl http://localhost:8765/api/v1/game/state
    ```

=== "Python"

    ```python
    import requests

    try:
        response = requests.get('http://localhost:8765/api/v1/game/state')
        response.raise_for_status()  # ステータスコードが異常な場合は例外を発生させる
        print(response.json())
    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")
    ```

=== "JavaScript (Node.js)"

    ```javascript
    fetch('http://localhost:8765/api/v1/game/state')
      .then(response => {
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        return response.json();
      })
      .then(data => console.log(data))
      .catch(error => console.error('Error:', error));
    ```

=== "C#"

    ```csharp
    using System;
    using System.Net.Http;
    using System.Threading.Tasks;

    class Program
    {
        static async Task Main(string[] args)
        {
            using var client = new HttpClient();
            try
            {
                var response = await client.GetStringAsync("http://localhost:8765/api/v1/game/state");
                Console.WriteLine(response);
            }
            catch (HttpRequestException e)
            {
                Console.WriteLine($"Error: {e.Message}");
            }
        }
    }
    ```

成功すると、次のようなJSONレスポンスが返されます：

```json
{
  "success": true,
  "data": {
    "game_tick": 2236,
    "colony_wealth": 13442.50,
    "colonist_count": 3,
    "storyteller": "Cassandra Classic",
    "is_paused": false
  },
  "errors": [],
  "warnings": [],
  "timestamp": "2025-11-28T10:33:26.8675876Z"
}
```


※一部のレスポンス値（ポーン名やラベルなど）は、ゲームの言語設定に依存します。

## 5. 別の例：入植者の取得

より実用的な例として、入植者のリストを取得してみましょう。

```bash
curl http://localhost:8765/api/v1/colonists
```

ID、名前、健康、心情などの基本情報を含む入植者のリストが返されます。

## 6. トラブルシューティング

-   **接続が拒否される（Connection Refused）**：
    -   RimWorldが起動しており、コロニーをロードしていることを確認してください。APIサーバーはメインメニューでは起動しません。
    -   APIコールのポートがMod設定で構成されたポート（デフォルトは `8765`）と一致しているか確認してください。
    -   ファイアウォールが接続をブロックしていないか確認してください。

-   **404 Not Found**：
    -   エンドポイントのURLが正しいか再確認してください。
    -   RIMAPIのModが有効化され、正しく読み込まれているか確認してください。

## 7. 言語モデルとの連携

RIMAPIをLLMと連携したい開発者向けに、シンプルなtxtファイル形式のドキュメントが用意されています。このファイルは開発プロセスを効率化するのに役立ちます。

<a href="https://ilyachichkov.github.io/RIMAPI/llms-full.txt" download="llms-full.txt" class="md-button md-button--primary">llms-full.txt をダウンロード</a>

## 次のステップ

準備完了です！RimWorldと連携する素晴らしいアプリケーションやツールを作り始めましょう。

-   利用可能なすべてのエンドポイントについては、**[APIリファレンス](./api.md)** をご確認ください。
-   質問や作成したものを共有するには、**[Discord](https://discord.gg/Css9b9BgnM)** にご参加ください。
