## .plyファイルの活用法検討

### ①meshlabから出力し、HWで読み込む場合
meshlabから各拡張子で出力し、HWで読み込んだ結果を下表に示す。

||meshlabの出力|HWでの読み込み|
|--|----|--|
|stl|可能|可能|
|obj|可能|3dsprintでstlに変換する必要あり|
|wrl|可能|3dsprintでstlに変換する必要あり|
|3ds|メッシュ数制限のため不可|-|
|dxf|可能|可能だがライン形状のみ|

### ②meshlabから出力し、HMで変換してHWで読み込む場合
HMで変換することでHWで読み込めたものを下表に示す。
HMでのサーフェス作成方法は　geom→surface→from FEでエレメントからサーフェス作成する。

||HMの出力|HWでの読み込み|
|--|--|--|
|iges|サーフェスを作成すれば可能|可能|
|jt|メッシュ・サーフェス共に可|メッシュ・サーフェス共に可|
|step|サーフェスを作成すれば可能|可能|
|x_t|サーフェスを作成すれば可能|可能（他と比べサーフェス形状がいびつだったように思う）|

### ③meshlabの機能

#### ・外れ値の除去
下記2通りの手順で外れ値を除去可能。各手順におけるパラメータの効果を下表に示す。

手順①：filters→cleaning and repairing→remove isolated pieces(wrt diameter)
手順②：filters→cleaning and reairing→remove isolated pieces(wrt face num.)

|機能|パラメータ|効果|
|--|--|--|
|remove isolated pieces(wrt diameter)|enter max diameter of isolated pieces(abs and %)|独立したメッシュ群の直径がパラメータ以下の場合削除する。|
|remove isolated pieces(wrt face num.)|enter minimum conn. comp size|独立したメッシュ群の要素数がパラメータ以下の場合削除する。|

#### ・スムージング
filters→smoothing, fairing and deformation→depth smooth
filters→smoothing, fairing and deformation→laplacian smooth

#### ・背景を白にする
下記の手順でglobal parameter windowを立ち上げ、下表のVariable Nameに対応するVariable Valueの枠をダブルクリックする。
meshlab_64bit_fpが立ち上がるのでmeshlab_64bit_fp上の色枠をクリックする。
pick a colorが立ち上がるのでpick a color上の変数htmlを下表のとおり変更することで背景色を白色に変更可能。

手順：tool→options

|Variable Name|html初期値|html変更後|
|--|--|--|
|Meshlab::Appearance::backgroundBotColor|#8080ff|#ffffff|
|Meshlab::Appearance::backgroundTopColor|#000000|#ffffff|

#### ・計測データのゼロ点補正（原点の移動）
下記手順①で座標軸を表示する。（原点位置を表示するためなのでやらなくても原点の移動は可能）
下記手順②でtransform:translate, center, set originを立ち上げ、下表の通りパラメータを変更し、applyすることで原点が移動する。
※下表new originの各入力欄は左からX方向移動量、Y方向移動量、Z方向移動量となる。
※下表new originの各入力欄の入力値は下記手順①で表示した座標軸の方向は負の値となる。

手順①：render→show axis
手順②：filter→normals, curvatures and orientation→transform:translate, center, set origin 

|変数名|初期値|変更後|
|--|--|--|
|transformation|xyz translation|set new origin|
|new origin|0,0,0|任意の値,任意の値,任意の値|

#### ・計測データのｍｍスケールへの変更
下記の手順でtransform:scale, normalizeを立ち上げ、下表の通りパラメータを変更し、applyすることでmからmmに変更できる。
※uniform scalingのトグルにチェックが入っている場合「X Axis」の値を参照して全方向に一様に拡大縮小を行なう。

手順：filter→normals, curvatures and orientation→transform:scale, normalize

|変数名|初期値|変更後|
|--|--|--|
|X Axis|1|1000|
|Y Axis|1|1000|
|Z Axis|1|1000|

#### ・寸法計測の記録方法
下記手順①でmeasuring toolを有効にし、2点をクリックして選択すると2点間の距離が表示される。この時Pを押すと画面右側のlayer dialog（表示されてなければ手順②で表示される）下側のログに今まで計測した距離の一覧が表示されるのでこれをコピーしてテキストファイル等に貼り付けて保存できる。
※PのほかにCで計測データのクリアや、Sで計測データのセーブができると書かれていたがセーブは上手くいかなかった。

手順①：edit→measuring tool
手順②：view→show layer dialog

#### ・寸法計測の表示改善
下記手順でクイックヘルプを有効にした状態でmeasuring toolを有効にすると計測した際に距離が左上に計測した順に表示される。

手順①：help→on screen quick help

#### ・ディストーションの悪いデータの削除
下記手順①でselect 'problematic' facesを立ち上げ、下表の機能を用いてディストーションの悪いデータを選択する。その後、下記手順②で選択したデータを削除する。

手順①：filter→selection→select 'problematic' faces
手順②：filter→selection→delete selected faces and vertices

|機能|変数名|要否|効果|
|--|--|--|--|
|select by aspect ratio|aspect ratio|必須|変数値以下のアスペクト比を持つ要素を全選択|
|select by normal angle|angle flip|任意|変数値以下の法線角度を持つ要素を全選択|

#### ・魚眼表示の解除
下記手順で魚眼表示の切り替えが可能。

手順：view→toggle orthographic camera

OpenViewの操作
![Openview](Openview.bmp)

![OpenVino2](OpenVino2.bmp)

MeshLabの操作

![MeshLab](MeshLab.bmp)

![MeshLab2](MeshLab2.bmp)
