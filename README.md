# 1Roll Javascript Player API Demo

## 使用方法

```HTML:index.html

<!DOCTYPE html>
<html>
  <head>
    <script>
      <!-- scriptタグを作成し、apiを読み込み -->
      var tag = document.createElement('script');
      tag.src = "https://cloud-player.1roll.jp/iframe-api";
      var firstScriptTag = document.getElementsByTagName('script')[0];
      firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
  
      <!-- OnerollPlayerManger, OnerollPlayerインスタンスを格納する変数を用意 -->
      var playerManager;
      var player;
  
      <!-- apiの読込が完了したら、onOnerollIframeAPIReadyが自動的に実行されます -->
      function onOnerollIframeAPIReady() {
        
        <!-- OnerollPlayerManagerを初期化 -->
        playerManager = new OnerollPlayerManager();
  
        <!-- OnerollPlayerManagerクラスからOnerollPlayerインスタンスを作成 -->
        player = playerManager.create(
          'player', <!-- プレイヤー配置先のHTML要素のidを指定 -->
          {
            videoId: 'XXXXXXXXXXXX',
            autostart: true,
            autostartSoundOn: true,
            allowfullscreen: true,
            jsonld: true,
            onPlayerReady: onPlayerReady,
            onPlayerStateChange: onPlayerStateChange,
            onPlayerTime: onPlayerTime,
            attributes: {
                class: 'player'
            }
          }
        );
      }
  
      function onPlayerReady(event) {
        console.log(event);        
      }
      
      function onPlayerStateChange(event) {
        console.log(event);
      }

      function onPlayerTime(event) {
        console.log(event);        
      }
    </script>
  </head>
  <body>
    <div id="player"></div> <!-- プレイヤー配置先 -->
  </body>
</html>

```
### 動画プレーヤーの読み込み

API の JavaScript コードが読み込まれると、API によって onOnerollIframeAPIReady 関数が呼び出されます。
この時点で、動画プレーヤーを管理する OnerollPlayerManager オブジェクトを作成できます。
以下の HTML（抜粋）は、上記の例の onOnerollIframeAPIReady 関数を示しています。

```js
var playerManager;
var player;
function onOnerollIframeAPIReady() {
  playerManager = new OnerollPlayerManager();
  player = playerManager.create('player', {
    videoId: 'XXXXXXXXXXXX',
    autostart: true,
    autostartSoundOn: true,
    allowfullscreen: true,
    jsonld: true,
    events{
      onPlayerReady: onPlayerReady,
      onPlayerStateChange: onPlayerStateChange,
      onPlayerTime: onPlayerTime
    }
    attributes: {
        class: 'player'
    }
  });
}
```

  
1. OnerollPlayerManager.createメソッドで、OnerollPlayerオブジェクトを作成します。
`OnerollPlayerManager.create(playerId:String, attrs:Object):OnerollPlayer`
  
2. 最初のパラメータのplayerIdは、DOM要素、または HTML要素のidを指定しています。ここに、プレーヤーが含まれる <iframe> タグが API によって挿入されます。

3. ２つ目のパラメータのattrsは、プレーヤーのオプションを指定するオブジェクトです。このオブジェクトには次のプロパティが含まれています。
* プロパティ一覧
  * `videoId (String) [required]` - プレーヤーが読み込む動画を識別する1RollのVideoID。
  * `width (Number) [option]` - プレイヤーの横幅を設定します。
  * `height (Number) [option]` - プレイヤーの縦幅を設定します。
  * `autostart (Boolean) [option]` - 自動再生させるかどうか。（デフォルトはfalse）　※ブラウザにより自動再生できない場合があります
  * `autostartSoundOn (Boolean) [option]` - 自動再生時に音声を出すかどうか。（デフォルトはfalse）　※ブラウザにより音声が出ない場合があります
  * `allowfullscreen (Boolean) [option]` - フルスクリーンを許可（デフォルトはtrue）
  * `jsonld (Boolean) [option]` - 動画情報タグ（JsonLDオブジェクト）を出力するかどうか。 許可すると挿入するためのscriptタグがiframeタグの次の行に挿入されます。
  * `events (Object) [option]` – APIが呼び出す関数（イベント リスナー）を識別します。
    * `onPlayerReady (Function) [option]` - OnerollPlayerが再生可能状態になったときに、値として渡した関数"onPlayerReady"が実行されます。
    * `onPlayerStateChange (Function) [option]` - OnerollPlayerの状態が変わった時に、値として渡した関数"onPlayerStateChange"が実行されます。
    * `onPlayerTime (Function) [option]` - OnerollPlayerで再生されている時に定期的に、値として渡した関数"onPlayerTime"実行されます。
  * `attributes (Object) [option]` - iframeタグに属性を設定します。この例では、class名を渡しています。


### クラスとメソッド

```
Class OnerollPlayerManager
```

OnerollPlayerMangerクラスは、OnerollPlayerオブジェクトを管理し、作成や削除を行います。
内部でOnerollPlayerオブジェクトを複数持つことができ、動画プレイヤーを複数埋め込みたいページにも対応します。

* `playerManager.create(playerId:String, attrs:Object):OnerollPlayer`  
OnerollPlayerオブジェクトを作成します。

* `playerManager.remove(player:OnerollPlayer):Void`  
OnerollPlayerオブジェクトをOnerollPlayerManagerの管理対象から削除します。

* `playerManager.getPlayers():Array<OnerollPlayer>`  
OnerollPlayerManagerクラスが管理しているOnerollPlayerオブジェクトリストを返します。

```
Class OnerollPlayer
```

* `player.play():Void`  
動画を再生します。

* `player.pause():Void`  
動画を停止します。

* `player.seek(time:Number):Void`  
動画を再生位置を変更します。timeは秒数を指定します。

### イベント

```
onPlayerReady(event)
```
  
* `event.target` - イベント対象となるOnerollPlayerオブジェクト
* `event.data.duration` - このtargetの動画の再生長さ

```
onPlayerStateChange(event)
```
  
* `event.target` - イベント対象となるOnerollPlayerオブジェクト
* `event.data.state` - このtargetの状態
  * ready - 再生準備完了
  * play - 再生開始
  * pause - 一時停止
  * complete - 再生完了

```
onPlayerTime(event)
```
  
* `event.target` - イベント対象となるOnerollPlayerオブジェクト
* `event.data.position` - このtargetの動画の現在の再生位置
* `event.data.duration` - このtargetの動画の再生長さ
