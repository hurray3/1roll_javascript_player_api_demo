
# 1Roll Javascript Player API Demo

## サンプルページ
https://hurray3.github.io/1roll_javascript_player_api_demo/0.1.0/index.html


## 使用方法

```HTML:index.html

<!DOCTYPE html>
<html>
    <head>
        <title>demo</title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">

        <style>
        .player-wrapper{
            width:360px;
        }
        .player{
            width:100%;
            padding-bottom: 56.25%;
            height:0px;
            position: relative;
        }
        .player iframe{
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        </style>

        <script>
        var tag = document.createElement('script');
        tag.src = "https://cloud-player.1roll.jp/iframe-api?version=0.1.0";
        var firstScriptTag = document.getElementsByTagName('script')[0];
        firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);            

        var playerManager;
        var player;

        function onOnerollIframeAPIReady(){

            playerManager = new OnerollPlayerManager();

            player = playerManager.create(
                'player-demo',
                {
                    videoId: 'reUuC88AE0PD',
                    allowfullscreen: true,
                    jsonld: true,
                    events:{
                        onPlayerReady:       onPlayerReady,
                        onPlayerStateChange: onPlayerStateChange,
                        onPlayerTime:        onPlayerTime,
                        onPlayed25P:         onPlayed25P,
                        onPlayed50P:         onPlayed50P,
                        onPlayed75P:         onPlayed75P,
                        onPlayed90P:         onPlayed90P,
                        onPlayed99P:         onPlayed99P,
                        onPlayed100P:        onPlayed100P
                    },
                    attributes: {
                        class: 'player-iframe'
                    }
                }
            );
        }

        function onPlayerReady(event) {
            console.log(event.target.getDuration());
        }

        function onPlayerStateChange(event) {
            console.log(event);
        }

        function onPlayerTime(event) {
            console.log(event);
        }

        function onPlayed25P(event){
            console.log("25% played");
        }
        function onPlayed50P(event){
            console.log("50% played");
        }
        function onPlayed75P(event){
            console.log("75% played");
        }
        function onPlayed90P(event){
            console.log("90% played");
        }
        function onPlayed99P(event){
            console.log("99% played");
        }
        function onPlayed100P(event){
            console.log("100% played");
        }
        </script>
    </head>
    <body>
        <div class="player-wrapper">
            <div class="player" id="player-demo"></div>
        </div>
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
    player = playerManager.create(
        'player-demo',
        {
            videoId: 'xxxxxxxxx',
            allowfullscreen: true,
            jsonld: true,
            events:{
                onPlayerReady:       onPlayerReady,
                onPlayerStateChange: onPlayerStateChange,
                onPlayerTime:        onPlayerTime,
                onPlayed25P:         onPlayed25P,
                onPlayed50P:         onPlayed50P,
                onPlayed75P:         onPlayed75P,
                onPlayed90P:         onPlayed90P,
                onPlayed99P:         onPlayed99P,
                onPlayed100P:        onPlayed100P
            },
            attributes: {
                class: 'player-iframe'
            }
        }
    );
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
    * `onPlayerTime (Function) [option]` - OnerollPlayerで再生されている時に定期的に、値として渡した関数"onPlayerTime"が実行されます。
    * `onPlayed25P (Function) [option]` - OnerollPlayerで動画の25%以上再生された時に値として渡した関数"onPlayed25P"が実行されます。※一度のみ実行されます
    * `onPlayed50P (Function) [option]` - OnerollPlayerで動画の50%以上再生された時に値として渡した関数"onPlayed50P"が実行されます。※一度のみ実行されます
    * `onPlayed75P (Function) [option]` - OnerollPlayerで動画の75%以上再生された時に値として渡した関数"onPlayed75P"が実行されます。※一度のみ実行されます
    * `onPlayed90P (Function) [option]` - OnerollPlayerで動画の90%以上再生された時に値として渡した関数"onPlayed90P"が実行されます。※一度のみ実行されます
    * `onPlayed99P (Function) [option]` - OnerollPlayerで動画の99%以上再生された時に値として渡した関数"onPlayed99P"が実行されます。※一度のみ実行されます
    * `onPlayed100P (Function) [option]` - OnerollPlayerで動画の100%再生された時に値として渡した関数"onPlayed100P"が実行されます。※一度のみ実行されます
  * `attributes (Object) [option]` - iframeタグに属性を設定します。この例では、class名を渡しています。


### クラス

```
Class OnerollPlayerManager
```
OnerollPlayerMangerクラスは、OnerollPlayerオブジェクトを管理し、作成や削除を行います。
内部でOnerollPlayerオブジェクトを複数持つことができ、動画プレイヤーを複数埋め込みたいページにも対応します。

#### メソッド

* `playerManager.create(playerId:String, attrs:Object):OnerollPlayer`  
OnerollPlayerオブジェクトを作成します。

* `playerManager.remove(player:OnerollPlayer):Void`  
OnerollPlayerオブジェクトをOnerollPlayerManagerの管理対象から削除します。

* `playerManager.getPlayers():Array<OnerollPlayer>`  
OnerollPlayerManagerクラスが管理しているOnerollPlayerオブジェクトリストを返します。

```
Class OnerollPlayer
```
#### メソッド

* `player.getDuration():Number`  
動画の再生時間を取得します。

* `player.play():Void`  
動画を再生します。

* `player.pause():Void`  
動画を停止します。

* `player.seek(time:Number):Void`  
動画を再生位置を変更します。timeは秒数を指定します。

#### イベント

* `onPlayerReady(event)`
再生の準備ができたタイミングで呼び出されます。  

	**parmas**
	* `event.target` - イベント対象となるOnerollPlayerオブジェクト
	* `event.data.duration` - このtargetの動画の再生長さ
	___
* `onPlayerStateChange(event)`
プレイヤーの再生状態が変更されたときに呼び出されます。  

	**parmas**
	* `event.target` - イベント対象となるOnerollPlayerオブジェクト
	* `event.data.state` - このtargetの状態
	  * ready - 再生準備完了
	  * play - 再生開始
	  * pause - 一時停止
	  * complete - 再生完了
	___

* `onPlayerTime(event)`
再生中に定期的に呼び出されます。  

	**parmas**
	* `event.target` - イベント対象となるOnerollPlayerオブジェクト
	* `event.data.position` - このtargetの動画の現在の再生位置
	* `event.data.duration` - このtargetの動画の再生長さ
	___

* `onPlayed25P(event)`
  再生時間が動画の長さの25%以上に達した時に1度だけ呼び出されます。
  
	**parmas**
	* `event.target` - イベント対象となるOnerollPlayerオブジェクト
	___

* `onPlayed50P(event)`
	再生時間が動画の長さの50%以上に達した時に1度だけ呼び出されます。
	
	**parmas**
	* `event.target` - イベント対象となるOnerollPlayerオブジェクト
	___

* `onPlayed75P(event)`
  再生時間が動画の長さの75%以上に達した時に1度だけ呼び出されます。
  
	**parmas**
	* `event.target` - イベント対象となるOnerollPlayerオブジェクト
	___

* `onPlayed90P(event)`
  再生時間が動画の長さの90%以上に達した時に1度だけ呼び出されます。
  
	**parmas**
	* `event.target` - イベント対象となるOnerollPlayerオブジェクト
	___

* `onPlayed99P(event)`
  再生時間が動画の長さの99%以上に達した時に1度だけ呼び出されます。
  
	**parmas**
	* `event.target` - イベント対象となるOnerollPlayerオブジェクト

	___

* `onPlayed100P(event)`
 再生時間が動画の長さの100%に達した時に1度だけ呼び出されます。
 
	**parmas**
	* `event.target` - イベント対象となるOnerollPlayerオブジェクト
	___
