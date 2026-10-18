# tamura keymap

## 対象機器

Adept anyball BTU 44mmです。販売元の案内に基づき、Ploopy Adeptと同じQMKファームウェアを使用する前提です。

- QMKターゲット: `ploopyco/madromys/rev1_001`
- OS: macOS
- VIA対応: 有効

## 動作

ボタン配置は次のとおりです。

```text
上段: 左クリック | DPI切替 | 右クリック | ブラウザ戻る
下段: DRAG_SCROLL                     | ブラウザ進む
```

`DPI_CONFIG` はDPIを順番に切り替えます。`DRAG_SCROLL` は押している間だけ有効になり、離すと通常のカーソル操作に戻ります。カーソル感度と通常のスクロール方式は変更していません。

スクロール速度の設定はこのキーマップの [`config.h`](config.h) にあります。

```c
#define PLOOPY_DRAGSCROLL_MOMENTARY
#define PLOOPY_DRAGSCROLL_DIVISOR_H 32.0
#define PLOOPY_DRAGSCROLL_DIVISOR_V 32.0
```

H/Vの除数を大きくすると、同じボール移動に対するスクロール量が減り、スクロールが遅くなります。縦横はそれぞれ独立して変更できます。

## ローカルビルド（macOS）

Docker Desktopを起動して、リポジトリのルートで次を実行します。QMKのビルド用コンテナを使うため、macOSにARMコンパイラを別途インストールする必要はありません。

```sh
docker run --rm \
  -v "$PWD:/qmk_firmware" \
  -w /qmk_firmware \
  ghcr.io/qmk/qmk_cli \
  qmk compile -kb ploopyco/madromys/rev1_001 -km tamura
```

ビルドに成功すると、リポジトリのルートに `ploopyco_madromys_rev1_001_tamura.uf2` が生成されます。

## GitHub ActionsからUF2を取得

関連するキーマップ、Madromys/Ploopy共通コード、QMKビルド設定、workflowを変更してpushすると、自動でビルドします。GitHubの **Actions** から `Build tamura firmware` の実行を開き、Artifacts欄の `tamura-firmware-<commit>` をダウンロードしてください。手動実行（`workflow_dispatch`）にも対応します。GitHub Actionsで手動実行するには、このworkflowがdefault branchに登録されている必要があります。

## 書き込み

1. 左下ボタンを押したままUSBケーブルを接続します。
2. macOSに認識されたドライブを開きます。
3. ビルドしたUF2ファイルをそのドライブへコピーします。コピー後、デバイスが再起動します。
