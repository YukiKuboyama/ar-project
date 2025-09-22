# AR PROJECT

### Discription

大阪万博2025の公式キャラクター「ミャクミャク」をARで表示するプロジェクト。


## 仕組み

- NFC（CodePenのURL）タッチで起動。
- CodePen（実際のコード）で書いたアプリケーション（Webカメラ）が起動。
- AR.js MarkerTrainingで作成したマーカー（.pattファイル）の読み込み。
- Blenderで作成したミャクミャクのglbファイルをAR表示。

むかしはARKitかなんかでコード書けたのに、サービス終了してた。
今回初めてCodePenってやつを使った。


### 環境

#### CodePen
ここにカメラを起動する、.glbファイルへのアクセス、.pattファイルのパスなどのソースが書いてある。（2025/09/22時点最新）

```html
<!-- A-Frame & AR.js 読み込み -->
<script src="https://aframe.io/releases/1.4.1/aframe.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/AR-js-org/AR.js/aframe/build/aframe-ar.min.js"></script>

<a-scene
  embedded
  arjs="sourceType: webcam;"
  renderer="antialias: true; logarithmicDepthBuffer: true; colorManagement: true; physicallyCorrectLights: true"
>
  <!-- 環境光 & 補助ライト -->
  <a-light type="ambient" color="#ffffff" intensity="1.0"></a-light>
  <a-light type="directional" color="#ffffff" intensity="0.5" position="1 2 1"></a-light>

  <!-- マーカー -->
  <a-marker
    type="pattern"
    url="https://yukikuboyama.github.io/ar-project/pattern-expo2025_marker.patt"
  >
    <!-- GLBモデル配置 -->
    <a-entity
      gltf-model="https://yukikuboyama.github.io/ar-project/myakumyaku1.glb"
      scale="0.5 0.5 0.5"
      position="0 1 1"
      rotation="0 270 90"
      material="side: double; transparent: false; depthTest: true; depthWrite: true"
    ></a-entity>
  </a-marker>

  <!-- カメラ -->
  <a-entity camera></a-entity>
</a-scene>
```

このCodePenのコーディングページをFullScreenで表示するとカメラが起動する。
この画面のURL [https://codepen.io/Yuki-Kuboyama/full/NPxPxYB](https://codepen.io/Yuki-Kuboyama/full/NPxPxYB) をNFCチップに書き込み。



### AR.js Marker Training

[https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html)
これはまじで簡単。
作成した画像をここにアップしてマーカー用の画像（.patt）をつくってもらう。
→この.pattファイルをGitHubにアップ。ファイルへのリンクをURL形式でCodePenに記載。（GitHubにあげないとその.pattファイルへの直接アクセスするhttpsパスを取得できない。）

ついでに、.pngファイルもダウンロードしてそれをマーカーに使う。
これは印刷物とかに貼り付けとけばOK。



## つまづいたこと

なんせ「Blender」。
自分で一から作るのは大変ということで、とりあえずモデル探し。
- CGTrader [https://www.cgtrader.com/welcome] →ほぼ有料。
- Sketchfab [https://sketchfab.com/3d-models/super-hero-japanese-expo-3e9496a6a707488697f24d15f9884fa6] でダウンロード可能だった
ミャクミャクのモデルをもらってきた。

これがまた、周りに不要なオブジェクトがついてて削除したり、
光の反射を変えたり地味に苦労した。
とりあえず、ミャクミャクだけ残して.glbファイルエクスポートして、CodePenにセット。

・・・はい。なんかモシャモシャする。ちらつきすぎて鬱陶しいので

- 複数の体のパーツオブジェクトを1つにまとめる。
- それぞれについていたマテリアルを1つにする。（テクスチャを1つに）←これがまじで大変。
- ボーンの削除。（ボーン同士の干渉をさせないように動きを完全にとめる。）
- アニメーションの削除。
- CodePenのコードに光の設定を入れる

これでなんとかチラつかずに見れるようになりました。
