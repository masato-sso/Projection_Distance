# Projection_Distance
## 改良投影距離法による手書き数字認識

パターン認識のための学習ー基礎と応用ー論文小特集に取り上げられた論文「ベイズアプローチによる最適識別系の有限標本効果に関する考察ーー学習標本の大きさがクラス間で異なる場合ーー」を参考にした. また, 手書き数字のデータはIPTP CDROM1Bを使用した.

### IPTP CDROM1Bデータ
各画像データは1サンプルの手書き数字の2値画像を格納している. 画像サイズは縦120,
横 80 である. バイナリ pgm 形式の場合, 各画像の濃淡値 0~255 は 8 ビットの 2 進数で表 現されるため, 1 バイト/画素となる. 今回の手書き数字の画像は 2 値画像とし, 文字部分が 値 0(黒), 背景部分が値 255(白)となる.

### 特徴抽出
文字認識研究の分野では, 多くの特徴抽出が存在する. 今回の特徴抽出は, 文字線の方向
に有力な「輪郭方向分布特徴抽出」と「濃淡画像変換」を扱う. この特徴抽出の発展形とし て, 2 値画像ではなく, 濃淡画像に適用される「濃淡勾配方向分布特徴抽出」がある.
  
・輪郭方向分布特徴抽出
手書き数字画像を複数のブロック領域に分割し, 各ブロック領域に含まれる黒連結領域
の輪郭画素の方向を 4 方向(H:水平, R:右 45 度, V:垂直, L:左 45 度)に量子化した後 に計数し, ブロック毎に4方向別の比率を算出する. この黒画素輪郭画素の比率をブロッ ク数分だけ並べたベクトルを輪郭方向分布特徴と呼ぶ.具体的には, 縦120画素×横80画素の手書き数字画像を10×10画素の正方ブロック領 域に分割することにより, 縦12×横8の総数96個のブロック領域を生成する. 各ブロッ クの黒画素輪郭の 4 方向別の比率を並べた特徴ベクトルの次元数は 12×8×4=384 となる.
  
・濃淡画像変換
白黒の 2 値だけで表されていた画像に対して, 灰色も含めて表す画像を濃淡画像と呼ぶ.具体的には, 色の連続した変化(濃度値)を8bitの0〜255の256レベルに量子化したも のである. また, 各濃度値に対応した濃淡度合いを表し, 2値画像に比べて豊かに画像を 表現することができる.
  
### 改良投影距離距離法
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

