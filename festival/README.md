># 日専祭アプリ：（音描、何食べようかな）

> **チーム：** みつぼし（26CM0128 古田裕稀、26CM0129 封怡璇、26CM0139 李安琪）
> 
> **AI利用レベル：** 2（生成コード可）
> **最終更新：** 2026-11-07

> 💡 制作の進め方は担任の先生から案内があります。このファイルの項目は、担任の先生の指示で増えることがあります。

---

## 何ができるアプリか（3行）
【音描-ongaku-】イラストを描く、絵を音に変換する、ダウンロードする

【何食べようかな】気分を選択、スマホを振ってランダムで食べ物を表示(3km範囲のお店)、地図を表示


## 画面
[音描]

（スクリーンショットを貼る。GitHub の編集画面に画像をドラッグ＆ドロップすると貼れます。）
<img width="1470" height="956" alt="スクリーンショット 2026-09-28 14 12 05" src="https://github.com/user-attachments/assets/d96e9deb-0574-4233-9907-3b38b95a4ba0" />

[何食べようかな]

<img width="300" height="600" alt="IMG_2153 (1)" src="https://github.com/user-attachments/assets/b018cd58-c745-41b9-be07-d7e1337c5929" />


<img width="300" height="600" alt="IMG_2154" src="https://github.com/user-attachments/assets/7def94c6-c537-49a9-b1a7-06d9f93ba384" />





## 使った文法・技術

（例：`@State`、`List`、`ForEach`、構造体、配列の `append` と `remove`）
[音描]
- 画面づくり:`VStack` / `HStack` / `ZStack`、`GeometryReader`(画面サイズでレイアウトを切り替え)、`.padding` `.frame` `.background` `.overlay`
- データの管理:`struct` / `class` / `enum`、`@State`、`@StateObject`、`@Published`、`ObservableObject`、配列(`[UInt8]` で絵を保存)
- お絵かき:`Canvas`、`DragGesture`(指の位置をマスの番号に変換)、2点をつなぐ線の計算
- もどす/やり直し:配列に絵のコピーを保存(`append` / `removeLast`)
- 音:`AVAudioEngine` で音の波形を自作して再生、`AVAudioPlayer` で効果音とBGM
- 同時に処理する仕組み:`Task` / `async` / `await`(曲づくりを裏で行い、画面が固まらないようにした)
- その他:`Timer`(放置の検知)、`UserDefaults`(背景の記憶)、`CIFilter`(QRコード作成)、`ShareLink`(共有)、`ViewModifier`(`.card` など見た目の使いまわし)




[何食べようかな]
- 画面づくり:`ZStack` / `VStack` / `HStack`、`GeometryReader`、`.padding` `.frame` `.background` `.overlay`
- データ:`struct`(`FoodItem` / `MoodTagItem`)、配列、`enum`、`extension`、オプショナル(`?` と `??`)
- 状態管理:`@State`、`@StateObject`、`@Published`、`@AppStorage`、`@Environment`、`@Namespace`
- 繰り返しと分岐:`ForEach`、`filter`、`randomElement()`、`switch`、`guard`
- 操作:`DragGesture`(ダイヤルを指で回す)、`onTapGesture`、端末を振る検知(`UIWindow` の `motionEnded` を通知に変換して `.onShake` を自作)
- アニメーション:`withAnimation`、`spring`、`matchedGeometryEffect`(起動画面のロゴがヘッダーへ飛ぶ)、`matchedTransitionSource` + `.zoom`(お皿からマップへズーム)、`rotation3DEffect`、`TimelineView` + `Canvas`(粒子の演出)
- 時間の制御:`DispatchQueue.main.asyncAfter`(ルーレットの減速や演出を、何秒後に何を変えるかで組み立てた)
- センサー:`CoreMotion`(端末の傾きと、振りの強さ)
- 位置情報とマップ:`CoreLocation`、`MapKit`(`MKLocalSearch`、`Map`、`Marker`)
- 音と触覚:`AVAudioPlayer`(BGM・効果音)、`AudioToolbox`、`UIImpactFeedbackGenerator`
- 設定画面:`Form`、`Toggle`、`.sheet`、`presentationDetents`



## 生成AIの使い方（どの場面で、どう使ったか）

（例：画面の骨組みは ChatGPT に書いてもらった。削除ボタンが効かなかったので、原因を質問して直した。）

## 展示で工夫した点

（例：文字を大きくして、来場者が操作しやすいようにした。）
[音描]
- 5分操作がないと「つづけますか?」を出し、さらに60秒でトップに戻る。放置されても次の来場者に、まっさらな状態で渡せる機能
- 初めての人向けに、光る枠と説明カードで進む10ステップのあそび方を用意（チュートリアル）
- 絵から曲調とテンポを自動で選ぶ「おまかせ設定」(ただし自分で選んだ場合は上書きしない)
- iPhone/iPadの縦横4パターンのレイアウトに対応（どの向きでも使用できる）
- お菓子の国をテーマにした、さわると動くかわいいデザイン（リアクション機能）
- 音がぶつっと切れてノイズが出ないよう、フェードやクロスフェードを入れて調整
- QRで描いた絵と曲を持ち帰れる（ずっと残せる）

  

[何食べようかな]
- ダイヤルを回す時のカチッという音と振動、惑星のぽんという動きなど、操作の手応えを細かく作った
- 気分ごとに、待機中の演出と結果発表の演出(金のキラキラ、湯気、炎、コインなど)を作成
- 強く振るほど、紙吹雪が多く、震えも大きくなる（連動）
- 振りが苦手な人でも遊べるよう、ボタンでも抽選できる（シェイプ機能のカバー）
- 設定画面で、BGM・効果音・触覚・エフェクトを個別にOFFにできる。端末の「視差効果を減らす」設定にも対応している
- BGMは抽選中に小さくなり、効果音を目立たせる。他のアプリの音楽が流れている時は邪魔をしない配慮
- iPhone と iPad の両方で、同じ見た目になるように大きさを自動で調整
- お店のピンを、いっせいにではなく、1つずつぽんと出す演出にした



