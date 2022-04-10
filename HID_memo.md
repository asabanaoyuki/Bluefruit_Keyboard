# Bluefruitの設定

## キーボードとして認識させる手順
1. bluetooth 接続クラスの呼び出し
```cpp:
Adafruit_BluefruitLE_SPI ble(BLUEFRUIT_SPI_CS, BLUEFRUIT_SPI_IRQ, BLUEFRUIT_SPI_RST);
```
2. 接続の設定（内部的にはpinの設定とSPI通信の開始をしています）
```cpp:
ble.begin(VERBOSE_MODE)
```
3. デバイスの名前設定
```cpp:
ble.sendCommandCheckOK("AT+GAPDEVNAME=Bluefruit_Keyboard") 
```
4. HIDであることを宣言
```cpp:
ble.sendCommandCheckOK(F( "AT+BleHIDEn=On" )
```
5. キーボード機能を有効
```cpp:
ble.sendCommandCheckOK(F( "AT+BleKeyboardEn=On"  )
```
6. 設定の適用のために再起動
```cpp:
ble.reset()
```


## キーの入力
AT+BLEKEYBOARDCODE=　の後にUSB HIDキーコードをつけて送信します。
キーボードの押下と解放を別のコマンドで指定するため、毎回キーの開放を送信する必要があります。
windowsキーは押下時でなく解放時に動作するためマクロを組む際には注意が必要です。

USB HIDキーコードは3~8バイトで構成されます。

* 1バイト目：Modifier（修飾語）

* 2バイト目：Reserved （予約済みで00固定）

* 3から8バイト目：HID Keyboard Codes（複数指定で同時押し）
```cpp:
ble.print("AT+BLEKEYBOARDCODE=00-00-04");//aキーの押下
ble.print("AT+BLEKEYBOARDCODE=00-00-00");//キーの開放
ble.print("AT+BLEKEYBOARDCODE=20-00-04");//Shift+a　Aキーの入力
ble.print("AT+BLEKEYBOARDCODE=00-00-04");//キーの開放
```
### Modifier　以下のいずれかを指定できます。
- __Bit 0 (0x01)__: Left Control
- __Bit 1 (0x02)__: Left Shift
- __Bit 2 (0x04)__: Left Alt
- __Bit 3 (0x08)__: Left Window
- __Bit 4 (0x10)__: Right Control
- __Bit 5 (0x20)__: Right Shift
- __Bit 6 (0x40)__: Right Alt
- __Bit 7 (0x80)__: Right Window

### HID Keyboard Codes　以下の6キーまで同時に指定できます。
[Adafruitのサイトを参考にしました。](https://learn.adafruit.com/introducing-adafruit-ble-bluetooth-low-energy-friend/ble-services)
* __0x00__	　Reserved　 _(no event indicated)_
* __0x01__	　Keyboard　 _ErrorRollOver_
* __0x02__	　Keyboard　 _POSTFail_
* __0x03__	　Keyboard　 _ErrorUndefined_
* __0x04__	　Keyboard　 _a and A_
* __0x05__	　Keyboard　 _b and B_
* __0x06__	　Keyboard　 _c and C_
* __0x07__	　Keyboard　 _d and D_
* __0x08__	　Keyboard　 _e and E_
* __0x09__	　Keyboard　 _f and F_
* __0x0A__	　Keyboard　 _g and G_
* __0x0B__	　Keyboard　 _h and H_
* __0x0C__	　Keyboard　 _i and I_
* __0x0D__	　Keyboard　 _j and J_
* __0x0E__	　Keyboard　 _k and K_
* __0x0F__	　Keyboard　 _l and L_
* __0x10__	　Keyboard　 _m and M_
* __0x11__	　Keyboard　 _n and N_
* __0x12__	　Keyboard　 _o and O_
* __0x13__	　Keyboard　 _p and P_
* __0x14__	　Keyboard　 _q and Q_
* __0x15__	　Keyboard　 _r and R_
* __0x16__	　Keyboard　 _s and S_
* __0x17__	　Keyboard　 _t and T_
* __0x18__	　Keyboard　 _u and U_
* __0x19__	　Keyboard　 _v and V_
* __0x1A__	　Keyboard　 _w and W_
* __0x1B__	　Keyboard　 _x and X_
* __0x1C__	　Keyboard　 _y and Y_
* __0x1D__	　Keyboard　 _z and Z_
* __0x1E__	　Keyboard　 _1 and !_
* __0x1F__	　Keyboard　 _2 and @_
* __0x20__	　Keyboard　 _3 and #_
* __0x21__	　Keyboard　 _4 and $_
* __0x22__	　Keyboard　 _5 and %_
* __0x23__	　Keyboard　 _6 and ^_
* __0x24__	　Keyboard　 _7 and &_
* __0x25__	　Keyboard　 _8 and *_
* __0x26__	　Keyboard　 _9 and (_
* __0x27__	　Keyboard　 _0 and )_
* __0x28__	　Keyboard　 _Return (ENTER)_
* __0x29__	　Keyboard　 _ESCAPE_
* __0x2A__	　Keyboard　 _DELETE (Backspace)_
* __0x2B__	　Keyboard　 _Tab_
* __0x2C__	　Keyboard　 _Spacebar_
* __0x2D__	　Keyboard　 _- and (underscore)_
* __0x2E__	　Keyboard　 _= and +_
* __0x2F__	　Keyboard　 _[ and {_
* __0x30__	　Keyboard　 _] and }_
* __0x31__	　Keyboard　 _\ and |_
* __0x32__	　Keyboard　 _Non-US # and ~_
* __0x33__	　Keyboard　 _; and :_
* __0x34__	　Keyboard　 _' and "_
* __0x35__	　Keyboard　 _Grave Accent and Tilde_
* __0x36__	　Keyboard　,_ and <_
* __0x37__	　Keyboard　 _. and >_
* __0x38__	　Keyboard　 _/ and ?_
* __0x39__	　Keyboard　 _Caps Lock_
* __0x3A__	　Keyboard　 _F1_
* __0x3B__	　Keyboard　 _F2_
* __0x3C__	　Keyboard　 _F3_
* __0x3D__	　Keyboard　 _F4_
* __0x3E__	　Keyboard　 _F5_
* __0x3F__	　Keyboard　 _F6_
* __0x40__	　Keyboard　 _F7_
* __0x41__	　Keyboard　 _F8_
* __0x42__	　Keyboard　 _F9_
* __0x43__	　Keyboard　 _F10_
* __0x44__	　Keyboard　 _F11_
* __0x45__	　Keyboard　 _F12_
* __0x46__	　Keyboard　 _PrintScreen_
* __0x47__	　Keyboard　 _Scroll Lock_
* __0x48__	　Keyboard　 _Pause_
* __0x49__	　Keyboard　 _Insert_
* __0x4A__	　Keyboard　 _Home_
* __0x4B__	　Keyboard　 _PageUp_
* __0x4C__	　Keyboard　 _Delete Forward_
* __0x4D__	　Keyboard　 _End_
* __0x4E__	　Keyboard　 _PageDown_
* __0x4F__	　Keyboard　 _RightArrow_
* __0x50__	　Keyboard　 _LeftArrow_
* __0x51__	　Keyboard　 _DownArrow_
* __0x52__	　Keyboard　 _UpArrow_
* __0x53__	　Keypad N　u_m Lock and Clear_
* __0x54__	　Keypad /　 __
* __0x55__	　Keypad *　 __
* __0x56__	　Keypad -　 __
* __0x57__	　Keypad +　 __
* __0x58__	　Keypad E　N_TER_
* __0x59__	　Keypad 1　 _and End_
* __0x5A__	　Keypad 2　 _and Down Arrow_
* __0x5B__	　Keypad 3　 _and PageDn_
* __0x5C__	　Keypad 4　 _and Left Arrow_
* __0x5D__	　Keypad 5　 __
* __0x5E__	　Keypad 6　 _and Right Arrow_
* __0x5F__	　Keypad 7　 _and Home_
* __0x60__	　Keypad 8　 _and Up Arrow_
* __0x61__	　Keypad 9　 _and PageUp_
* __0x62__	　Keypad 0　 _and Insert_
* __0x63__	　Keypad .　 _and Delete_
* __0x64__	　Keyboard　 _Non-US \ and |_
* __0x65__	　Keyboard　 _Application_
* __0x66__	　Keyboard　 _Power_
* __0x67__	　Keypad =　 __
* __0x68__	　Keyboard　 _F13_
* __0x69__	　Keyboard　 _F14_
* __0x6A__	　Keyboard　 _F15_
* __0x6B__	　Keyboard　 _F16_
* __0x6C__	　Keyboard　 _F17_
* __0x6D__	　Keyboard　 _F18_
* __0x6E__	　Keyboard　 _F19_
* __0x6F__	　Keyboard　 _F20_
* __0x70__	　Keyboard　 _F21_
* __0x71__	　Keyboard　 _F22_
* __0x72__	　Keyboard　 _F23_
* __0x73__	　Keyboard　 _F24_
* __0x74__	　Keyboard　 _Execute_
* __0x75__	　Keyboard　 _Help_
* __0x76__	　Keyboard　 _Menu_
* __0x77__	　Keyboard　 _Select_
* __0x78__	　Keyboard　 _Stop_
* __0x79__	　Keyboard　 _Again_
* __0x7A__	　Keyboard　 _Undo_
* __0x7B__	　Keyboard　 _Cut_
* __0x7C__	　Keyboard　 _Copy_
* __0x7D__	　Keyboard　 _Paste_
* __0x7E__	　Keyboard　 _Find_
* __0x7F__	　Keyboard　 _Mute_
* __0x80__	　Keyboard　 _Volume Up_
* __0x81__	　Keyboard　 _Volume Down_
* __0x82__	　Keyboard　 _Locking Caps Lock_
* __0x83__	　Keyboard　 _Locking Num Lock_
* __0x84__	　Keyboard　 _Locking Scroll Lock_
* __0x85__	　Keypad C　o_mma_
* __0x86__	　Keypad E　q_ual Sign_
* __0x87__	　Keyboard　 _International1_
* __0x88__	　Keyboard　 _International2_
* __0x89__	　Keyboard　 _International3_
* __0x8A__	　Keyboard　 _International4_
* __0x8B__	　Keyboard　 _International5_
* __0x8C__	　Keyboard　 _International6_
* __0x8D__	　Keyboard　 _International7_
* __0x8E__	　Keyboard　 _International8_
* __0x8F__	　Keyboard　 _International9_
* __0x90__	　Keyboard　 _LANG1_
* __0x91__	　Keyboard　 _LANG2_
* __0x92__	　Keyboard　 _LANG3_
* __0x93__	　Keyboard　 _LANG4_
* __0x94__	　Keyboard　 _LANG5_
* __0x95__	　Keyboard　 _LANG6_
* __0x96__	　Keyboard　 _LANG7_
* __0x97__	　Keyboard　 _LANG8_
* __0x98__	　Keyboard　 _LANG9_
* __0x99__	　Keyboard　 _Alternate Erase_
* __0x9A__	　Keyboard　 _SysReq/Attention_
* __0x9B__	　Keyboard　 _Cancel_
* __0x9C__	　Keyboard　 _Clear_
* __0x9D__	　Keyboard　 _Prior_
* __0x9E__	　Keyboard　 _Return_
* __0x9F__	　Keyboard　 _Separator_
* __0xA0__	　Keyboard　 _Out_
* __0xA1__	　Keyboard　 _Oper_
* __0xA2__	　Keyboard　 _Clear/Again_
* __0xA3__	　Keyboard　 _CrSel/Props_
* __0xA4__	　Keyboard　 _ExSel_
* __0xE0__	　Keyboard　 _LeftControl_
* __0xE1__	　Keyboard　 _LeftShift_
* __0xE2__	　Keyboard　 _LeftAlt_
* __0xE3__	　Keyboard　 _Left GUI_
* __0xE4__	　Keyboard　 _RightControl_
* __0xE5__	　Keyboard　 _RightShift_
* __0xE6__	　Keyboard　 _RightAlt_
* __0xE7__	　Keyboard　 _Right GUI
