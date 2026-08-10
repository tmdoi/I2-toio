---
layout: default
title: "2. toio接続とLED制御"
parent: "公開講座2026"
nav_order: 20
---

# 2. toio接続とLED制御

いよいよtoioを動かします．まずはロボットの光（LED）を光らせて，「つながった！」を体験しましょう．

---

## 準備：toioの電源を入れる

toioキューブの**底面**に小さなボタンがあります．押すと電源が入ります．電源を切るときは長押しします．

---

## 準備：自分のキューブの名前を確認する

toioキューブには，1台ずつちがう名前がついています．キューブに貼ってあるシールを見てください．

例：`toio-A1B` ，`toio-C2D`

この名前を使って「どのキューブにつなぐか」をプログラムに教えます．教室にはたくさんのキューブがあるので，これをまちがえると友だちのキューブが動いてしまいます．

---

## 最初のプログラム

Thonnyの**エディタ（上半分）**に，次のプログラムをそのままコピーして貼り付けてください．

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名に書きかえる

with SimpleCube(name=target_cube_name) as cube:
    cube.turn_on_cube_lamp(0, 0, 255, 2)  # 2秒間，青く光る
    print("青色に光らせました")
```

## 【超重要】ここだけは必ず書きかえる

```python
target_cube_name = "toio-xxx"
```

この `toio-xxx` の部分を，**自分のキューブに貼ってあるシールの名前**に書きかえてください．

例：シールが `toio-A1B` なら

```python
target_cube_name = "toio-A1B"
```

**このページから最後のページまで，ずっと同じ書き方をします．新しいプログラムを貼り付けたら，まずここを書きかえるクセをつけましょう．**

書きかえたら，緑の**Run（実行）**ボタンを押します．保存を聞かれたら好きな名前（例：`led1.py`）で保存してください．

数秒待つと，toioが青く光ります．光ったら成功です．

---

## プログラムの読み方

```python
from toio.simple import SimpleCube
```

toioを動かすための道具を呼び出す，おまじないの行です．毎回いちばん上に書きます．

```python
with SimpleCube(name=target_cube_name) as cube:
```

「名前がこれのキューブにつないでね」という行です．つながったキューブに `cube` という名前がつきます．

**この行より下のtoioへの命令は，すべて字下げして書きます．** 字下げされた部分がおわると，toioとの接続は自動的に切れます．

```python
    cube.turn_on_cube_lamp(0, 0, 255, 2)
```

`cube.〜` が「キューブへの命令」です．これは「LEDを光らせて」という命令です．

---

## 色の指定のしかた

```python
cube.turn_on_cube_lamp(r, g, b, duration)
```

- `r`：赤の強さ（0〜255）
- `g`：緑の強さ（0〜255）
- `b`：青の強さ（0〜255）
- `duration`：光らせる秒数

赤・緑・青の3色の混ぜぐあいで色を作ります．

| 色 | r | g | b |
| --- | --- | --- | --- |
| 赤 | 255 | 0 | 0 |
| 緑 | 0 | 255 | 0 |
| 青 | 0 | 0 | 255 |
| 黄 | 255 | 255 | 0 |
| 紫 | 255 | 0 | 255 |
| 水色 | 0 | 255 | 255 |
| 白 | 255 | 255 | 255 |

消したいときは次の命令を使います．

```python
cube.turn_off_cube_lamp()
```

---

## 3色を順番に光らせる

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    cube.turn_on_cube_lamp(255, 0, 0, 1)  # 赤を1秒
    cube.turn_on_cube_lamp(0, 255, 0, 1)  # 緑を1秒
    cube.turn_on_cube_lamp(0, 0, 255, 1)  # 青を1秒
    cube.turn_off_cube_lamp()             # 消す
```

---

## やってみよう

**やってみよう1**: 上のプログラムの数字を変えて，**自分の好きな色**を作ってみましょう．となりの人と見せ合ってみてください．

**やってみよう2**: `for` を使って，赤い光を5回チカチカさせてみましょう．

<details markdown="1">
<summary>ヒント</summary>

`for i in range(5):` のなかで，「1秒光らせる」と「1秒消す」をくり返します．消えている時間を作るには `cube.sleep(1)` を使います．

</details>

<details markdown="1">
<summary>答えの例</summary>

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    for i in range(5):
        cube.turn_on_cube_lamp(255, 0, 0, 0)  # 赤くつける
        cube.sleep(0.5)                       # 0.5秒待つ
        cube.turn_off_cube_lamp()             # 消す
        cube.sleep(0.5)                       # 0.5秒待つ
```

`duration` を `0` にすると「消せと言われるまでずっと光る」という意味になります．

</details>

---

## うまくいかないときは

| 症状 | 見るところ |
| --- | --- |
| なかなかつながらない | toioの電源が入っているか．底のボタンを押す |
| つながらずエラーになる | キューブ名が正しいか．`toio-xxx` のままになっていないか |
| 赤い字でエラーが出る | `"` や `(` `)` を書き忘れていないか |
| 字下げのエラー | `with` の下の行がスペース4個分ずれているか |

それでも直らないときは，手をあげてスタッフを呼んでください．

---

[次へ：モーター制御①](03-motor1.html)
