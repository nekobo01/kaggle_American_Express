# kaggle_American_Express
- Title American Express - Default Prediction
URL	  https://www.kaggle.com/competitions/amex-default-prediction
Timeline	2022/5/25 ⇒ 2022/8/24

- Description
カード発行会社は、私たちが請求した金額をきちんと返済してくれることをどうやって確認するのでしょうか？
貸し倒れ予測は、消費者金融ビジネスのリスク管理の中心的存在です。
貸し倒れを予測することで、貸し出しの決定を最適化し、より良い顧客体験と健全なビジネス経済を実現することができます。

- Evaluation
このコンペティションの評価指標である , Mは、順位付けのための2つの指標の平均値である。
正規化ジニ係数（Normalized Gini Coefficient) ,G および4%時点のデフォルト率（default rate captured at 4%,D.
4%で捕捉されるデフォルト率は，予測値の最高位である4%以内に捕捉された正のラベル（デフォルト）の割合であり，感度/想起統計量を表している．
サブメトリクスのG,Dの両方と 、ネガティブラベルには、ダウンサンプリングを調整するために20の重みが与えられます。

- Data
このコンペティションの目的は、毎月の顧客プロファイルに基づいて、ある顧客が将来クレジットカードの残高を返さない確率を予測することです。
対象の二項変数は、最新のクレジットカード明細書から18ヶ月間のパフォーマンスウィンドウを観察することによって計算され、
もし顧客が最新の明細書の日付から120日以内に返済額を支払わない場合は、デフォルトイベントとみなされます。
データセットには、各顧客の各明細書日付における集約されたプロファイル特徴が含まれています。特徴は匿名化、正規化されており、以下の一般的なカテゴリに分類されます。
	
D_* = 延滞変数　　96カラム  
S_* = 支出変数　　22カラム
P_* = 支払い変数　 3カラム	P_2,P_3,P_4
B_* = 残高変数　　40カラム
R_* = リスク変数　28カラム (合計189カラム)
であり、以下の特徴はカテゴリ的である。  
	
[B_30', 'B_38', 'd_114', 'd_116', 'd_117', 'd_120', 'd_126', 'd_63', 'd_64', 'd_66', 'd_68'].
あなたのタスクは、各顧客IDについて、将来の支払い不履行の確率を予測することです（ターゲット=1）。
このデータセットでは、ネガティブ・クラスは5%でサブサンプルされているので、スコアリング・メトリックでは20倍の重み付けを受けることに注意してください。

### 特徴量の特徴
全191カラム
| dtype_name | counts | samples |
| --- | --- | --- |
| int8 | 2 |B_31, target|
| float16 | 185 | --- |
| object | 4 |customer_ID, S_2(日付),D_63,D_64 |

<int8>  
B_31 0/1 ほとんど1が立っている。平均0.99  
  
<object>  
S_2	日付列
D_63	CL,CO,CR,XL,XM,XZ
D_64	-1,O,R,U

欠損が多い  
D_87 D_88 D_108	D_110 D_111 B_39 D_73 B_42 D_134 D_137 D_138 D_135 D_136 R_9 B_29 D_106 D_132 D_49 R_26 D_76  

カテゴリ的  
B_30, B_38, d_114, d_116, d_117, d_120, d_126, d_63, d_64, d_66, d_68  

### 読んだkernel	
Understanding NA values in AMEX competition  
https://www.kaggle.com/code/raddar/understanding-na-values-in-amex-competition	
サマリー：多くのcustomer_idで欠損値の位置が共通するパターンを3つ見つけた。初回入金時に残高0円NAが発生していると思われる。
ネクスト：欠損値の処理の際、0で埋めるのは悪手ぽい。初回入金時の残高0円パターンがあり、そのフラグを入れておきたい。

AMEX EDA: Even more insane time patterns revealed  
https://www.kaggle.com/code/pavelvod/amex-eda-even-more-insane-time-patterns-revealed  	
サマリー：大量のデータにおいて季節性など一定の法則が見られた。
ネクスト：時系列情報の特徴量を追加?日時で正規化を事前に掛けるとよいかもとのこと。 
	
Amex EDA - Iso Forest,Down Sampling & PCA  
https://www.kaggle.com/code/krishm/amex-eda-iso-forest-down-sampling-pca  
サマリー：カテゴリ変数がターゲットに与える影響。  
ネクスト：相関の高いデータはどうせ主成分分析するから…と削除を怠ったが、やったほうが良さそう。  
年、月、日 といった特徴量も追加したほうが良さそうだね。  

Feather & Parquet Files : AMEX-Default Prediction  
https://www.kaggle.com/datasets/ruchi798/parquet-files-amexdefault-prediction?select=test_data.ftr  
サマリー：このデータを使ってtest,trainが一気に軽くなるみたい。ad_dataより追加可能だったので直接追加しました。  
	 ダウンロードしなくていいの助かる。  

Lag Features Are All You Need  
https://www.kaggle.com/code/thedevastator/lag-features-are-all-you-need  
サマリー：Last - FirstとLast / First列を追加するのがオススメ  

Amex LGBM Dart CV 0.7977  
https://www.kaggle.com/code/ragnar123/amex-lgbm-dart-cv-0-7977  
サマリー：seed 42 , 最終地 - 平均値列を追加するのがオススメ
	
### idea_list
・曜日によってデータが異なる⇒週末支払傾向のある人は返しにくいなど  
・特徴量重要度の確認⇒その内容を確認してみる  
・主成分分析の効果がありそうか検討  
・customer_ID別に最終明細書時点のデータのみ参考にしているので、それ以前のデータを使う方法を検討する  
・累計明細書数、各数値の初回からの増減率、平均値、最頻値など
・そもそも0.1%の抽出したデータでモデル作っていいの? 全数で回るか、最新結果だけにしたときに回るかの2段階チェック  
⇒全数を対象に標準化した時点でメモリ不足エラーが発生。customer_idの最新に限定したほうがいいかも。  
その上で「平均〇〇数」など過去を考慮した列を追加して学習させる。
・精度爆落ちした。恐らく0で埋めたことが大きそうだが、主成分分析で削り過ぎたか？あとは列がずれている？
・fillnaは動くが、dropがとにかく処理落ちる…。なぜ
・欠損値の処理の際、0で埋めるのは悪手ぽい。初回入金時の残高0円パターンがあり、そのフラグを入れておきたい。
	
### log_date
| 日付| 説明|
| ---------------------------------- | ----------------------------------------------- |
|2022-06-09|データ量が多くて主成分分析が回っていない<br>読む⇒https://www.kaggle.com/code/aliphya/who-would-default-next|
|2022-06-10|コードを日本語に訳しつつ1行ずつ実行して内容理解を進めた。<br>EDAは概ねOK！明日からはモデリングの確認|
|2022-06-11|久しぶりにlightgbmの実装。メモリエラー対策を進めた。<br>ひとまずスコアは出た。明日からは特徴量の精査|
|2022-06-12|とりあえず特徴量の詳細確認と特徴量重要度の算出をした。<br>明日は新特徴量を追加してみる|
|2022-06-13|特徴量を追加したがメモリ不足でエラーが発生。対策を検討する。<br>多変量解析か？|
|2022-06-14|mergeよりjoinに変更し処理軽量化を図る。GPUオフだとモデル落ちない。<br>モデルのinput179とtest189の特徴量が異なるというエラー|
|2022-06-15|欠損値があまりにも多いカラムは削除し、一部欠損しているデータは0で埋めた<br>主成分分析の必要コードを実装し、データ量削減した|
|2022-06-17|数値カラムのみを使って主成分分析を実施した後、カテゴリ変数やTargetを再び戻す処理を追加。<br>コードに関しても見やすいよう一部書き換え。処理落ち減ったように感じる|
|2022-06-18|エンコーディングを実装<br>主成分の個数指定もできた。joinへの書き換えも進めた|
|2022-06-19|そもそものデータ処理の順序がcutomer_ID別にしてからじゃないとおかしくなると気付いたので全体修正。<br>pickleの導入で楽になった。ただ出力がうまくいかないの困る|	
|2022-06-24|aggを使った特徴量抽出。だいぶスムーズになった。欠損値の0埋めや主成分分析はなしに変更<br>pickle形式で加工済のtrainが落とせるようにした。スムーズ！|  
|2022-06-24|cudf,DASKに関して調査したが使えなさそう。。。不便…。<br>アカウントID列の軽量化は成功。とはいえまだメモリ不足にはなる。Google colablatoryか？？|  
|2022-07-06|プロ契約したら回った。モデルをdartに変更<br>dartにはearly_stoppingが効かないので要注意。学習中に落ちないようにPCの設定を変更しました。|  
|2022-07-07|相関係数が高い変数の削除をしておきたい<br>あとは|  
|2022-07-10|変数の削除したら精度下がったので相関係数は触らない<br>seed値を変更、最終値 - 平均値 という特徴量を追加する|  

### 参考にした記事一覧
lightGBMの使い方とハイパーパラメータについて  
https://datadriven-rnd.com/lightgbm/  
LightGBMのパラメータチューニングまとめ  
https://qiita.com/c60evaporator/items/351188110f328ff921b9  
【初心者向け】特徴量重要度の算出 (LightGBM) 【Python】【機械学習】  
https://mathmatical22.xyz/2020/04/12/%e3%80%90%e5%88%9d%e5%bf%83%e8%80%85%e5%90%91%e3%81%91%e3%80%91%e7%89%b9%e5%be%b4%e9%87%8f%e9%87%8d%e8%a6%81%e5%ba%a6%e3%81%ae%e7%ae%97%e5%87%ba-lightgbm-%e3%80%90python%e3%80%91%e3%80%90%e6%a9%9f/  
light GBMでimportanceを出す  
https://qiita.com/studio_haneya/items/e70e835c26524d506e19  
【Python】Kaggleで引っ張りだこ！lightgbmの２種類の使い方！Training APIとScikit-learn API！【lightgbm】  
https://shiokoji11235.com/two_interface_of_lightgbm  
【Python】Pandasのメモリ使用量の削減方法のまとめ  
https://www.st-hakky-blog.com/entry/2020/06/10/093016  
めっちゃ使えるpandasのメモリサイズをグッと抑える汎用的な関数  
https://qiita.com/hiroyuki_kageyama/items/02865616811022f79754  
Jupyter(IPython)上でメモリ食っている変数を探し出して削除する
https://qiita.com/AnchorBlues/items/883790e43417640140aa  
Kaggle モデル保存と読み出し
https://qiita.com/greco_guitar/items/30e3f1bdcfa6acb2febb  
pickle を使った、学習済みモデルの保存・読み出し方法  
https://www.sairablog.com/article/pickle-trained-model-save-read.html#.Yq18M1S4pBE.twitter  	
Memory Trick - Reduce Memory 8x or 16x!(メモリ使用量を1/8にする方法)  
https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations/discussion/308635  	

### 分析ステップ  
▼▼▼▼▼モデルの構築(train)▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼  
必要なライブラリの準備
trainデータの読み込み-- 5,532,451行 191列  
(データ概要の確認)-- あくまで確認なのでスキップしてOK  
エンコーディング  
customer_idごとの最新データで抽出-- 458,913行 191列  
(欠損値の多い列の削除)-- 意味がありそう。削除はしない。  
欠損値を0で埋める  
(a)int型のみのdf , (b)str型のみのdf に分割  
	(上記aを対象に)標準化  
	(上記aを対象に)主成分分析 188カラム⇒100カラムに圧縮  
(a)(b)の結合-- 458,913行 103列(日付とtargetとcustomer_IDの分+3)  
(相関の高いカラム同士を削除) -- このステップは主成分分析済なので不要かも  
特徴量エンジニアリング-- 458913行 104列  
モデリング -- LightGBM  
(特徴量重要度の確認) -- 主成分分析前にやっていた作業  
メモリ解放  
▼▼▼▼▼予測値の計算(test)▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼  
testデータの読み込み-- 11,363,762行 190列(target列がないので)
エンコーディング  
customer_idごとの最新データで抽出-- 924,621行 190列  
欠損値を0で埋める
(a)int型のみのdf , (b)str型のみのdf に分割  
	(上記aを対象に)標準化  
	(上記aを対象に)主成分分析 188カラム⇒100カラムに圧縮  
(a)(b)の結合-- 924,621行 102列(日付とcustomer_IDの分+2)  
特徴量エンジニアリング-- 924,621行 103列  
モデル適用  
予測結果の算出  

924621
	
