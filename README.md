# BitzMath

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

BitzMathは、BitzLabsエコシステム内で数式を構造的に扱い、レンダリングするための専門ライブラリです。

## 主な機能

-   **`MathAST`の定義**: 分数、べき乗、根号といった数式の意味構造を表現する、厳密に型付けされた抽象構文木(AST)。
-   **2つの入力ルート**:
    1.  **パーサー**: LaTeX風の数式記述言語を解釈し、`MathAST`を生成します。
    2.  **専用API (Fluent API)**: C#やTypeScriptのコードから、流れるようなインターフェースで`MathAST`を安全かつプログラム的に構築できます。
-   **レイアウトエンジン**: `MathAST`を受け取り、数式の組版ルール（上付き文字の位置、分数のレイアウトなど）を計算し、描画プリミティブの集合である**`LayoutAST`**を生成します。

## ✅ 初期開発ToDoリスト

1.  **`MathAST`の型定義**:
    *   `BitzAstCore`の`IAstNode`を継承した`IMathNode`インターフェースを定義。
    *   `FractionNode`, `PowerNode`, `IdentifierNode`, `NumberNode`など、主要なASTノードクラスの「殻」を作成。
2.  **専用API (ビルダー) の設計**:
    *   `MathBuilder`静的クラスを作成し、`Fraction(num, den)`のようなメソッドを定義。内部で対応する`FractionNode`をインスタンス化して返す。
3.  **パーサーの骨格**:
    *   `MathParser`クラスを作成し、`BitzParser`の`ParserBase`を継承。
    *   `Parse()`メソッドを実装し、まずは`1`や`x`、`1+2`のような最も単純な式をパースして対応するASTノードを返すロジックを実装。
4.  **レイアウトエンジンのインターフェース**:
    *   `MathLayoutEngine`クラスを作成。
    *   `Render(IMathNode node)`メソッドを定義し、出力として`BitzLayout`の`ILayoutNode`を返すようにする。
    *   初期実装では、`NumberNode`を受け取ったら対応する`TextSpanNode`を返す、という単純な変換ロジックを実装。

## このライブラリの位置づけ

このライブラリは、主に`BitzDoc`から利用され、Markdown文書内に高品質な数式を埋め込む機能を提供します。また、BitzLabsをSDKとして利用するアプリケーションが、動的に数式を描画するためにも使用されます。

このライブラリの最終的な出力は`LayoutAST`です。これをSVGやPDFなどの具体的な形式に変換する処理は、`BitzRenderers`に委譲されます。

### 利用シナリオ

-   **DSL経由**: Markdown文書に埋め込まれた数式文字列 (`$c = \sqrt{a^2 + b^2}$`) をレンダリングする。
-   **API経由**: 計算結果を動的に数式として描画する科学技術計算アプリケーションやレポートジェネレーターを構築する。

### 依存関係

-   `BitzAstCore`
-   `BitzParser`
-   `BitzLayout` (出力型として)
