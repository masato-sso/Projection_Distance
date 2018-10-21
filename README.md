# Projection_Distance
## 改良投影距離法による手書き数字認識

パターン認識のための学習ー基礎と応用ー論文小特集の論文「ベイズアプローチによる最適識別系の有限標本効果に関する考察ーー学習標本の大きさがクラス間で異なる場合ーー」（ダウンロードリンク：https://ci.nii.ac.jp/naid/110003183471 ）を参考にした. また, 手書き数字のデータはIPTP CDROM1Bを使用した.

### IPTP CDROM1Bデータ
各画像データは1サンプルの手書き数字の2値画像を格納している. 画像サイズは縦120,横80である. バイナリpgm形式の場合, 各画像の濃淡値0~255は8ビットの2進数で表現されるため, 1バイト/画素となる. 今回の手書き数字の画像は 2値画像とし, 文字部分が値0(黒), 背景部分が値255(白)となる.

### 特徴抽出
文字認識研究の分野では, 多くの特徴抽出が存在する. 今回の特徴抽出は, 文字線の方向に有力な「輪郭方向分布特徴抽出」と「濃淡画像変換」を扱う. この特徴抽出の発展形として, 2値画像ではなく, 濃淡画像に適用される「濃淡勾配方向分布特徴抽出」がある.
  
● 輪郭方向分布特徴抽出  
手書き数字画像を複数のブロック領域に分割し, 各ブロック領域に含まれる黒連結領域の輪郭画素の方向を4方向(H:水平, R:右 45 度, V:垂直, L:左 45 度)に量子化した後 に計数し, ブロック毎に4方向別の比率を算出する. この黒画素輪郭画素の比率をブロック数分だけ並べたベクトルを輪郭方向分布特徴と呼ぶ.具体的には, 縦120画素×横80画素の手書き数字画像を10×10画素の正方ブロック領域に分割することにより, 縦12×横8の総数96個のブロック領域を生成する. 各ブロックの黒画素輪郭の4方向別の比率を並べた特徴ベクトルの次元数は12×8×4=384となる.
  
● 濃淡画像変換  
白黒の 2 値だけで表されていた画像に対して, 灰色も含めて表す画像を濃淡画像と呼ぶ.具体的には, 色の連続した変化(濃度値)を8bitの0〜255の256レベルに量子化したものである. また, 各濃度値に対応した濃淡度合いを表し, 2値画像に比べて豊かに画像を表現することができる.
  
### 改良投影距離法
改良投影距離法は全てのクラスで
<img src="https://latex.codecogs.com/gif.latex?\inline&space;\left|\Sigma_{N}\right|"/>
,
<img src="https://latex.codecogs.com/gif.latex?\inline&space;P(\omega)"/>
,
<img src="https://latex.codecogs.com/gif.latex?\inline&space;N"/>
,
<img src="https://latex.codecogs.com/gif.latex?\inline&space;N_{0}"/>
が等しいと仮定すると, 次式で定義される改良投影距離によって, 擬似ベイズ識別関数を近似することができる.
  
<img src="https://latex.codecogs.com/gif.latex?\;g(x)=\|X-M\|^{2}-\sum_{i=1}^{k}\frac{(1-\alpha)\lambda_{i}}{(1-\alpha)\lambda_{i}+\alpha\sigma^{2}}\left\{\Phi_{i}^{T}(X-M)\right\}^{2}"/>
  
投影距離による分類において, 未知パターンXは各々のクラスの長軸までの距離の短 い方のクラスに分類される. 改良投影距離の決定境界は固有値によって決まる双曲線になり, 超平面の交差部分(共通部分空間)とその近傍における誤認識を避けることができる.
