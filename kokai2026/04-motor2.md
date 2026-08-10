---
layout: default
title: "4. モーター制御②"
parent: "公開講座2026"
nav_order: 40
---

# 4. モーター制御②（左右のタイヤを別々に動かす）

toioには**左右2つのタイヤ（モーター）**があります．この2つを別々のスピードで回すと，カーブを描いて走れます．

---

## 左右を別々に動かす：run_motor

```python
cube.run_motor(left_speed, right_speed, duration)
```

- `left_speed`：左のタイヤのスピード（-100〜100）
- `right_speed`：右のタイヤのスピード（-100〜100）
- `duration`：動かす秒数（最大2.55秒）

まずは実行してみましょう．

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    cube.run_motor(70, 30, 2)  # 左が速い → 右にカーブ
    cube.sleep(1)
    cube.run_motor(30, 70, 2)  # 右が速い → 左にカーブ
```

### 曲がり方のルール

| 左のスピード | 右のスピード | 動き |
| --- | --- | --- |
| 50 | 50 | まっすぐ前進 |
| 70 | 30 | 右にカーブ |
| 30 | 70 | 左にカーブ |
| 50 | -50 | その場で右回り |

**速い方と反対に曲がる**と覚えると簡単です．左右の差が大きいほど，きついカーブになります．

---

## for で正方形を描く

前のページで調べた「90度回る秒数」を使います．

「前に進む → 90度回る」を4回くり返せば，正方形になるはずです．同じことを4回書くかわりに，`for` を使いましょう．

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    for i in range(4):
        cube.move(50, 1)     # 1秒前進（正方形の1辺）
        cube.sleep(0.2)
        cube.spin(30, 0.5)   # 90度回る（秒数は自分の実験の値に）
        cube.sleep(0.2)
```

きれいな正方形になりましたか．ずれてしまう場合は `cube.spin()` の秒数を少しずつ調整してみてください．`0.5` → `0.52` のように，こまかく変えられます．

**うまくいったら**：`cube.move(50, 1)` の `1` を `2` にすると，大きな正方形になります．

---

## LEDと組み合わせる

走りながら光らせることもできます．`duration` を `0` にすると「止めろと言われるまで続ける」という意味になります．

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    cube.turn_on_cube_lamp(0, 255, 0, 0)  # 緑に光らせたまま
    for i in range(4):
        cube.move(50, 1)
        cube.spin(30, 0.5)
    cube.turn_off_cube_lamp()             # 消す
```

---

## やってみよう

**やってみよう1**: `cube.run_motor()` の数字をいろいろ変えて，どんな走り方になるか試しましょう．

- `run_motor(60, 40, 2)`
- `run_motor(80, 20, 2)`
- `run_motor(50, -50, 2)`

**やってみよう2（発展）**: 「8の字」を描いてみましょう．

<details markdown="1">
<summary>ヒント</summary>

8の字は「右まわりの円」と「左まわりの円」をつなげた形です．

- 右まわりの円 → 左のタイヤを速く
- 左まわりの円 → 右のタイヤを速く

1回の `run_motor` は最大2.55秒までなので，`for` を使って何回かくり返すと，まるい輪になります．

</details>

<details markdown="1">
<summary>答えの例</summary>

```python
from toio.simple import SimpleCube

target_cube_name = "toio-xxx"  # ←自分のキューブ名

with SimpleCube(name=target_cube_name) as cube:
    # 右まわりの円
    for i in range(3):
        cube.run_motor(60, 20, 2)
    # 左まわりの円
    for i in range(3):
        cube.run_motor(20, 60, 2)
```

**注意**: スピードとくり返し回数は，床のすべりぐあいで変わります．きれいな8の字になるまで数字を調整してみましょう．

</details>

---

ここまでで，toioを走らせる命令はすべて出そろいました．次はいよいよ，**自分の手で運転**します．

[次へ：キーボード運転](05-keyboard.html)
