# データ構造ルール

## quotes.json

### 構造
```json
{
  "quotes": [
    {
      "id": 1,
      "original_text": "Original text in source language",
      "original_language_code": "en",
      "author_id": "author_full_name_snake_case",
      "category_id": "category_snake_case",
      "source": "出典情報（書籍名、講演名など）",
      "translations": {
        "ja": {
          "text": "翻訳されたテキスト"
        },
        "zh-TW": {
          "text": "中文翻譯文本"
        }
      },
      "links": [
        {
          "title": "リンクのタイトル",
          "url": "https://example.com",
          "type": "source"
        }
      ]
    }
  ],
  "metadata": {
    "last_updated": "YYYY-MM-DD",
    "version": "バージョン番号"
  }
}
```

### フィールド説明
- `id`: 名言の一意識別子（数値）
- `original_text`: 原文テキスト（基準となる元の言語）
- `original_language_code`: 原文の言語コード（ISO 639-1）
- `author_id`: 著者ID（スネークケース形式、例: albert_einstein）
- `category_id`: カテゴリID（スネークケース形式、例: motivational, wisdom, success）
- `source`: 出典情報（書籍、講演、インタビュー等）
- `translations`: 各言語への翻訳オブジェクト
- `links`: 関連リンクの配列

### 翻訳構造
- 言語コード（ISO 639-1）をキーとした翻訳オブジェクト
- 各翻訳には`text`フィールドが必須
- 原文言語の翻訳は含めない（originalが基準）

### リンク構造
- `title`: リンクの表示名
- `url`: リンクURL
- `type`: リンクタイプ（source, reference, related, wikipedia, video, audio等）

### ルール
- `original_text`と`original_language_code`は必須
- `translations`オブジェクトに原文言語は含めない
- 翻訳が存在しない言語は`translations`から省略
- `source`が空の場合、UIでは出典情報を表示しない
- `links`が空配列の場合、UIではリンクセクションを表示しない
- アプリは指定言語の翻訳があればそれを表示、なければ原文を表示

## categories.json

### 構造
```json
{
  "categories": [
    {
      "category_id": "category_snake_case",
      "name": "English Category Name",
      "name_translations": {
        "ja": "日本語でのカテゴリ名"
      },
      "description": "Category description in English",
      "description_translations": {
        "ja": "日本語での説明"
      },
      "color": "#3498db",
      "icon": "lightbulb"
    }
  ],
  "metadata": {
    "last_updated": "YYYY-MM-DD",
    "version": "バージョン番号"
  }
}
```

### ルール
- `category_id`: quotes.jsonの`category_id`と一致させる
- `name`: 英語での正式名称（基準名）
- `name_translations`: 他言語でのカテゴリ名翻訳
- `description`: カテゴリの説明（英語）
- `description_translations`: 説明の他言語翻訳
- `color`: UI表示用の色コード（HEX形式）
- `icon`: アイコン名（SF Symbols等）

## authors.json

### 構造
```json
{
  "authors": [
    {
      "author_id": "author_full_name_snake_case",
      "name": "English Full Name",
      "name_translations": {
        "ja": "日本語での著者名"
      },
      "birth_year": 1879,
      "death_year": 1955,
      "nationality": "German-American",
      "profession": "Theoretical Physicist",
      "profession_translations": {
        "ja": "理論物理学者"
      }
    }
  ],
  "metadata": {
    "last_updated": "YYYY-MM-DD",
    "version": "バージョン番号"
  }
}
```

### ルール
- `author_id`: quotes.jsonの`author_id`と一致させる
- `name`: 英語での正式名称（基準名）
- `name_translations`: 他言語での著者名翻訳
- `birth_year`, `death_year`: 生没年（不明の場合はnull）
- `nationality`: 国籍・出身
- `profession`: 職業・専門分野（英語）
- `profession_translations`: 職業の他言語翻訳

## 言語コード

### サポート言語
- `en`: English（基準言語）
- `ja`: 日本語
- `zh-TW`: 繁體中文（台灣）
- `ko`: 한국어
- `es`: Español
- `fr`: Français
- `de`: Deutsch

### 追加ルール
- ISO 639-1標準に従う
- 新しい言語を追加する場合は、既存のすべての項目に対応する翻訳を追加することを推奨
- 翻訳が存在しない場合は、その言語キーを省略する

## リンクタイプ定義

### 標準タイプ
- `source`: 主要出典（書籍、論文、記事等）
- `reference`: 参考資料
- `related`: 関連記事・コンテンツ
- `wikipedia`: Wikipedia記事
- `video`: 動画コンテンツ
- `audio`: 音声コンテンツ

### UI表示ルール
- 各タイプに対応するアイコンを表示
- リンクタイトルとタイプを表示
- 外部リンク矢印アイコンを右端に配置
- タップでブラウザ起動