# フロントエンドの開発方法
JavaScript を開発するにあたり、フロントエンド開発でも　node.js　をベースとしたパッケージ管理をした方が良いようなので、メモ。

## 技術
- node.js : 開発環境
- npm : パッケージマネージャ
  - yarn : パッケージ管理
- webpack : パッケージをまとめてビルドする。
- vscode : エディタ

## JavaScriptのみの場合
<details>
<summary>
コードサンプル
</summary>

#### .vscode/launch.json
``` javascript:launch.json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Open index.html",
            "url": "http://127.0.0.1:5500/index.html",
            "webRoot": "${workspaceFolder}",
            "pathMapping": {
                "http://127.0.0.1:5500":"/Path/To/Code"
            }
        }
    ]
}
```
#### index.html
``` javascript: index.html
<head>
    <meta charset="UTF-8">
    <title>Simple Block</title>    
    <script type="importmap">
          {
            "imports": {
              "three": "./three.js-master/build/three.module.js",
              "dat.gui": "./3dEngine/dat.gui.module.js",
              "three/addons/": "./three.js-master/examples/jsm/"
            }
          }
        </script>
        <script>
            if (global === undefined) {
                var global = window;
            }
        </script>
</head>
```
#### main.js
``` 
import * as THREE from "three";
import * as dat from "dat.gui";
import Stats from "three/addons/libs/stats.module.js";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
```
</details>

<details>
<summary>
フォルダ構成例
</summary>

```
.
├── css
│   ├── bootstrap.min.css
│   └── bootstrap.min.css.map
├── js
│   ├── jquery.min.js
│   ├── math.js.map
│   ├── math.min.js
│   ├── plotly-1.58.5.min.js
│   └── shipmotion.js
└── index.html
```

</details>

## npmを使った場合

<details>
<summary>
コードサンプル
</summary>

1. npmの初期化
```
npm init -y
```

1. パッケージのインストール
```
npm install -D webpack webpack-cli webpack-dev-server
```

1. package.jsonのカスタマイズ
npx webpackではなく、npm run buildでOKになる。
また、npm run startでwebpack-dev-serverが起動するようになる。
``` json
{
  "scripts": {
    "build": "webpack",
    "start": "webpack serve"
  },
  "devDependencies": {
    "webpack": "^5.76.2",
    "webpack-cli": "^5.0.1",
    "webpack-dev-server": "^4.15.0"
  },
  "private": true
}
```

1. webpack.config.jsのカスタマイズ

``` javascript:webpack.config.js
module.exports = {
  // メインとなるJavaScriptファイル（エントリーポイント）
  entry: `./src/index.js`,

  // ファイルの出力設定
  output: {
    //  出力ファイルのディレクトリ名
    path: `${__dirname}/dist`,
    // 出力ファイル名
    filename: "main.js",
  },

  // モード値を production に設定すると最適化された状態で、
  // development に設定するとソースマップ有効でJSファイルが出力される
  mode: "production", //"development", 

  // ローカル開発用環境を立ち上げる
  // 実行時にブラウザが自動的に localhost を開く
  devServer: {
    static: "dist",
    open: true,
  },
};
```

1. ./dist/index.htmlの作成
``` html:./dist/index.html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <script src="main.js"></script>
</head>
<body>
</body>
</html>
```

1. ./src/index.jsの作成

``` javascript:./src/index.js
// import 文を使って sub.js ファイルを読み込む。
import { hello } from "./sub";

// sub.jsに定義されたJavaScriptを実行する。
hello();
```

1. sub.jsの作成
``` javascript:./src/sub.js
export function hello() {
  alert("Hello Webpack!");
}
```

1. 自動リロードの実行

```
rpm run start
```

1. ビルド
```
npm run build
```

</details>

<details>
<summary>
フォルダ構成例
</summary>

```
.
├── dist
│   ├── index.html
│   └── main.js
├── node_modules
├── package-lock.json
├── package.json
├── src
│   ├── index.js
│   └── sub.js
└── webpack.config.js
```
</details>

## 参考サイト

[webpackシンプルな回答](https://teratail.com/questions/335567)
[webpack概要](https://ics.media/entry/12140/)
[webpack使い方詳細](https://www.webdesignleaves.com/pr/jquery/webpack_basic_01.html)
[webpack-Bootstrapの使用例](https://www.webdesignleaves.com/pr/plugins/bootstrap5_webpack.html)

[npmパッケージ使い方](https://numb86-tech.hatenablog.com/entry/2020/08/17/020022)
[npm-scripts使い方](https://ics.media/entry/12226/)

[yarnインストール](https://yarnpkg.com/getting-started/install)
[Markdownチートシート](https://qiita.com/Qiita/items/c686397e4a0f4f11683d)
