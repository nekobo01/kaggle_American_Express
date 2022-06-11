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
	
D_* = 延滞変数  
S_* = 支出変数  
P_* = 支払い変数  
B_* = 残高変数  
R_* = リスク変数  
であり、以下の特徴はカテゴリ的である。  
	
[B_30', 'B_38', 'd_114', 'd_116', 'd_117', 'd_120', 'd_126', 'd_63', 'd_64', 'd_66', 'd_68'].
あなたのタスクは、各顧客IDについて、将来の支払い不履行の確率を予測することです（ターゲット=1）。
このデータセットでは、ネガティブ・クラスは5%でサブサンプルされているので、スコアリング・メトリックでは20倍の重み付けを受けることに注意してください。
	

| ファイル名                          | 説明|
| ---------------------------------- | ----------------------------------------------- |
|train_data.csv|customer_IDごとに複数のstatement dateを持つトレーニングデータ。|
|train_labels.csv|各 customer_ID のターゲットラベル。|
|test_data.csv|対応するテストデータ、あなたの目的は、各 customer_ID のターゲット ラベルを予測することです。|
|sample_submission.csv|正しいフォーマットで作成されたサンプルファイル|

- directory tree
```
/kaggle/input/amex-default-prediction/
├── README.md
├── sample_submission.csv	<---- 
├── train_data.csv		<---- 110万行×190特徴量がcustomer_ID別に掲載
├── test_data.csv		<---- 
└── train_labels.csv		<---- customer_ID別に離反結果が掲載(目的変数)
```
### idea_list
|カテゴリ|アイデア詳細|実装済|
|---|---|---|
|新しい特徴量の検討|曜日によってデータが異なる⇒週末支払傾向のある人は返しにくいなど|未実装|
|特徴量について知る|特徴量重要度の確認|未実装|
|新しい特徴量の検討|主成分分析の効果がありそうか検討|未実装|



### log_date
| 日付| 説明|
| ---------------------------------- | ----------------------------------------------- |
|2022-06-09|データ量が多くて主成分分析が回っていない<br>読む⇒https://www.kaggle.com/code/aliphya/who-would-default-next|
|2022-06-10|コードを日本語に訳しつつ1行ずつ実行して内容理解を進めた。<br>EDAは概ねOK！明日からはモデリングの確認|
|2022-06-11|久しぶりにlightgbmの実装。メモリエラー対策を進めた。<br>ひとまずスコアは出た。明日からは特徴量の精査|
|2022-06-12|久しぶりにlightgbmの実装。メモリエラー対策を進めた。<br>ひとまずスコアは出た。明日からは特徴量の精査|


### 参考にした記事一覧
LightGBMのパラメータチューニングまとめ  
https://qiita.com/c60evaporator/items/351188110f328ff921b9  
【初心者向け】特徴量重要度の算出 (LightGBM) 【Python】【機械学習】  
https://mathmatical22.xyz/2020/04/12/%e3%80%90%e5%88%9d%e5%bf%83%e8%80%85%e5%90%91%e3%81%91%e3%80%91%e7%89%b9%e5%be%b4%e9%87%8f%e9%87%8d%e8%a6%81%e5%ba%a6%e3%81%ae%e7%ae%97%e5%87%ba-lightgbm-%e3%80%90python%e3%80%91%e3%80%90%e6%a9%9f/  
【Python】Pandasのメモリ使用量の削減方法のまとめ  
https://www.st-hakky-blog.com/entry/2020/06/10/093016  
めっちゃ使えるpandasのメモリサイズをグッと抑える汎用的な関数  
https://qiita.com/hiroyuki_kageyama/items/02865616811022f79754  
