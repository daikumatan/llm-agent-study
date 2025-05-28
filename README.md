# LLM Agent Study

## 1. pythonの仮想環境の準備と必要なパッケージのインストール

以下の手順で **Poetry ＋ Python 3.11.1** の仮想環境を用意し、`pyproject.toml` に書かれた依存関係をインストールできます。


### 1.1. 前提：Python 3.11.1 を用意する

pyenvなどを用いてpython 3.11.1 をインストールします。以下はpyenvを利用する場合

```bash
pyenv install 3.11.1
```

### 1.2. Poetry をインストールする

#### macOS / Linux

```bash
curl -sSL https://install.python-poetry.org | python3 -
# パスが通っていない場合は ~/.poetry/bin を PATH へ
```

### 1.3. プロジェクトを準備する

```bash
git clone <あなたのリポジトリURL> llm-agent-study
cd llm-agent-study           # ← ここに pyproject.toml がある想定
```


### 1.4. Poetry に Python 3.11 を紐づける

```bash
# どの python を使うか明示（パスでもバージョンでも可）
poetry env use 3.11
```

* 既存の 3.11 系バイナリが見つからない場合はフルパスで指定します（`poetry env use /usr/local/bin/python3.11` など）。
* Poetry が `.venv/`（デフォルトはシステム共通の場所）に仮想環境を自動で作ります。
  **プロジェクト直下に置きたい**場合は

  ```bash
  poetry config virtualenvs.in-project true --local
  ```

  を一度だけ実行しておくと `.venv/` がカレントに生成されます（VS Code との連携が楽）。


### 1.5. 依存パッケージをインストール

```bash
poetry install   # pyproject.toml の内容を解決して .lock を生成
```

これで `ipykernel`, `langchain` などが一括で入り、`poetry.lock` が作成されます。


### 1.6. 仮想環境を使う

```bash
poetry shell   # 仮想環境のシェルに入る
python -V      # -> Python 3.11.1 と表示されるはず
```

* **VS Code**
  右下の Python インタプリタ切替で `.venv/bin/python`を選択。Jupyter Notebook で使う場合は下記でカーネルを登録できます。

```bash
python -m ipykernel install --user --name llm-agent-study --display-name "Python (llm-agent-study)"
```

### 1.7. よくある追加操作

| やりたいこと             | コマンド例                                 |
| ------------------ | ------------------------------------- |
| 新しい依存を追加           | `poetry add numpy pandas`             |
| 開発用だけ入れる           | `poetry add --group dev black pytest` |
| lock を更新せずインストールのみ | `poetry install --no-root`            |
| 依存ツリーを見る           | `poetry show --tree`                  |
| 仮想環境の場所を確認         | `poetry env info -p`                  |


### 1.8. トラブルシューティング

| 症状                             | 確認ポイント                                                |
| ------------------------------ | ----------------------------------------------------- |
| `poetry env use` でバージョンが見つからない | `python3.11 -V` が通るか確認／フルパス指定                         |
| VS Code がパッケージを認識しない           | インタプリタ選択が仮想環境になっているか                                  |
| ロックが壊れた／再解決したい                 | `poetry lock --no-update` または `poetry lock --rewrite` |

---

これで `llm-agent-study` プロジェクトの環境構築は完了です。
Poetry は **「pyproject.toml が唯一の真実」** という考え方なので、依存追加やバージョン変更はすべて `poetry add` / `poetry remove` で行うと後々トラブルが減ります。楽しんで開発してください！


## 2. `.env` ファイルの作成

ターミナル上で以下を入力し、`.env`ファイルを作成します

```bash
cat << EOF > .env
OPENAI_API_KEY="<OpenAIのAPI KEYを入力>"
EOF
```
