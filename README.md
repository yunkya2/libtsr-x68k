# libtsr

libtsr は、Human68k 向けのメモリ常駐プログラム（TSR）を C 言語で作るためのライブラリです。デバイスドライバ型と、デバイスヘッダを持たない常駐プロセス型の両方を扱えます。

主な機能は次のとおりです。

- 常駐プログラムの定義、検索、常駐終了、解除
- CONFIG.SYSで登録可能なデバイスドライバとHuman68k起動後の動的登録・登録解除の両立
- 複数デバイスの定義とブロックデバイスのドライブ接続・切断
- DOS/IOCS ベクタの設定・復元と、旧ハンドラの呼び出し
- 常駐部用のヒープ・スタックの確保
- 常駐部と非常駐部の分離

## ビルド

ビルドには[elf2x68k](https://github.com/yunkya2/elf2x68k)のバージョン 20261004 以降が必要です。リポジトリのルートから以下を実行してください。

```sh
make
```

生成物は `lib/libx68ktsr.a` です。削除する場合は `make clean` を実行します。

## 利用方法

ビルド時に `include/` をヘッダ検索パスへ、`lib/` をライブラリ検索パスへ追加して `-lx68ktsr` をリンクします。

以下は、デバイスヘッダを持たない常駐プロセスの基本形です。初回起動時に常駐し、同じプログラムを再度起動すると検索して解除します。

```c
#include <x68k/tsr.h>

TSR_PROCESS("EXAMPLE TSR");

int main(void)
{
    tsr_process_t proc = tsr_findproc("EXAMPLE TSR");
    if (proc == NULL)
        tsr_keepproc(NULL, 0);
    return tsr_freeproc(proc) < 0;
}
```

割り込みや DOS/IOCS ベクタを変更したプログラムでは、解除前に `tsr_restorevectors()` などで元の状態へ戻してください。通常のヒープやスタックは既定の常駐範囲に含まれません。常駐後も必要な領域には `TSR_HEAP`、`TSR_STACK` を利用できます。

常駐部と初期化部を分ける場合は、常駐部のオブジェクトを `bin/m68k-xelf-tsrgen` に渡して部分リンクし、生成したオブジェクトを初期化部と最終リンクします。

```sh
bin/m68k-xelf-tsrgen -o resident.o resident-src.o
```

このツールは既定で `lib/libx68ktsr.a` を使い、常駐部に残る未解決シンボルを検査します。オプションとリンク時の注意点は [API リファレンス](README-libtsr.md#常駐部と非常駐部の分離)を参照してください。

## ドキュメント

- [API リファレンスと利用例](README-libtsr.md)
- [公開ヘッダ](include/x68k/tsr.h)

## ライセンス

MIT License
