# ネクストハイスクール予算Q&A

ユーザー提供の240件のQ&Aを検索して提示する、スマートフォン向け音声チャットWebアプリ。

## GitHub Pages で動かす（APIキー不要）
1. GitHubに新しい公開/非公開リポジトリを作る。
2. `index.html` と `knowledge.csv` をリポジトリのルートにアップロードする（`worker` フォルダは任意）。
3. Settings → Pages → Deploy from a branch → main / (root) → Save。
4. 公開URLを開いて質問する。CSV検索モードで上位5件が表示される。これは意味理解によるAI回答ではなく**類似文字列検索**のため、原文との照合が必要。

## AIによる会話を有効化（任意）
1. Cloudflare Workers を作り `worker/worker.js` を配置・デプロイする。
2. Workers設定の **Secrets** に `OPENAI_API_KEY` を登録する。GitHubやHTMLにAPIキーを書かない。
3. Workers設定の **Variables** に `ALLOWED_ORIGIN` を正確なGitHub Pagesのオリジンとして登録（例：`https://example.github.io`）。必要なら `OPENAI_MODEL` も指定する（標準 `gpt-4.1-mini`）。
4. Cloudflare側で rate limiting / 認証 / 予算・利用上限を設定する。**オリジン制限だけでは第三者利用を防げません**。安全対策なしでの一般公開は禁止推奨。
5. アプリ内「設定」に Worker URL を入力すると、CSV検索上位5件を参考資料としてAIに送る。

※CSV全文をモデルに学習させる方式ではなく、毎回関連箇所を検索して渡す簡易RAG方式。検索漏れがあり得るため、厳密な制度判断は原文資料で確認してください。
※ブラウザ音声認識・読み上げの可用性はiOS/Android/ブラウザにより異なります。音声入力には通常HTTPSおよびマイク許可が必要です。
※ファイルをダブルクリックして `file://` で開くとCSVのfetchがブロックされることがあります。GitHub Pagesまたは `python -m http.server` で確認してください。
※公開リポジトリに置いたCSVは誰でも閲覧できるため、公開してよいデータか確認してください。
