---
layout: default
title: "3. モーター制御①"
parent: "公開講座2026"
nav_order: 30
---

# 3. モーター制御①（前後に進む・その場で回る）

いよいよtoioを走らせます．まずは基本の2つの命令を覚えましょう．

**注意**: toioは机から落ちます．床か，机のまん中の広いところで動かしてください．

---

## 前後に進む：move

```python
cube.move(speed, duration)
```

- `speed`：スピード（-100〜100）．**プラスで前進，マイナスで後退**
- `duration`：動かす秒数（最大2.55秒）

まずは実行してみましょう．

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    cube.move(30, 1)  # スピード30で1秒間，前に進む
```

---

## 待つ・止める

```python
cube.sleep(seconds)
```

`seconds` 秒のあいだ，何もしないで待ちます．

### 【注意】`time.sleep()` は使わない

Pythonには `time.sleep()` という「待つ」命令もありますが，**toioを動かすプログラムでは必ず `cube.sleep()` を使ってください**．`time.sleep()` を使うと，待っているあいだにtoioとの通信が切れてしまうことがあります．

```python
cube.stop_motor()
```

モーターを止めます．

---

## 前進 → 停止 → 後退

`move` と `sleep` を組み合わせると，動きをつなげられます．

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    cube.move(50, 2)   # 2秒 前に進む
    cube.sleep(1)      # 1秒 止まる
    cube.move(-50, 2)  # 2秒 後ろにさがる
```

もとの場所に戻ってきましたか．

---

## その場で回る：spin

```python
cube.spin(speed, duration)
```

- `speed`：スピード（-100〜100）．**プラスで右回り（時計回り），マイナスで左回り**
- `duration`：回る秒数（最大2.55秒）

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    cube.spin(30, 1)   # 右回りに1秒
    cube.sleep(1)
    cube.spin(-30, 1)  # 左回りに1秒
```

---

## 実験：ちょうど90度回すには？

つぎのページで「正方形を描く」ことに挑戦します．そのために，**何秒回すとちょうど90度（直角）になるか**を，自分のtoioで調べておきましょう．

床にテープなどで目印をつけて，秒数を少しずつ変えながら試します．

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    cube.spin(30, 0.5)  # ←この 0.5 をいろいろ変えて試す
```

`0.3` `0.4` `0.5` `0.6` … と変えて，いちばん直角に近くなった数字をメモしておいてください．

**メモ欄**：スピード30のとき，90度回るのは ______ 秒

床の材質やtoioの個体差で，答えは人によってちがいます．自分の数字を見つけるのがこの実験です．

---

## やってみよう

**やってみよう1**: スピードを `20` `50` `100` に変えて `cube.move()` を実行し，進む距離のちがいを見てみましょう．

**やってみよう2**: 「2秒前進 → 1秒待つ → 右に90度回る → 2秒前進」というプログラムを作ってみましょう．90度回る秒数は，上の実験でメモした数字を使います．

<details markdown="1">
<summary>答えの例</summary>

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    cube.move(50, 2)     # 2秒前進
    cube.sleep(1)        # 1秒待つ
    cube.spin(30, 0.5)   # 右に90度（秒数は自分の実験の値に）
    cube.move(50, 2)     # 2秒前進
```

</details>

---

[次へ：モーター制御②](04-motor2.html)
