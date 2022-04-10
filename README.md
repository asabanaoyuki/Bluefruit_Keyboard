# Bluefruit_Keyboard
Arduino互換機をbluetoothマクロキーボードとして動作させます。

巷で販売しているマクロキーボードはPC側にアプリを入れる必要がありますが、Arduino側でマクロを組めばPCからはただのbluetoothキーボードに見えるという目論見です。

## ハードウエア

### 用意したもの
* Adafruit Feather 32u4 Bluefruit LE　(bluetooth付きのArduino互換機　チップはArduino Leonardoと同じ)
* リチウムイオンポリマー電池　400ｍAh
* キースイッチ×8
* キーキャップ×8
* ロータリーエンコーダ―×1

## 配線図



# ソフトウエア

* Arduinoスケッチ

[HIDコマンドの覚書](https://github.com/asabanaoyuki/Bluefruit_Keyboard/blob/main/HID_memo.md)


```bash
pip install huga_package
```
