<link href="style.css" rel="stylesheet"></link>


# 平成27年度 TSUBAME産業利用トライアルユース 成果報告書

https://www.preferred-networks.jp/
</center>

## 邦文抄録

 本プロジェクトでは、深層学習による表現学習の創薬への応用として、化合物のFinger Printを学習ベースで獲得するNeural Finger Print（NFP）を再現し、さらにその乱択アルゴリズム化であるDrop Edgeを提案した。さらに、両者の化合物活性予測のタスクをTSUBAME上で実施し、予測精度と速度を比較した。72アッセイの化合物活性予測に対してNFPとDrop Edgeを適用し、特に半径（アルゴリズム中のイテレーション回数）が大きい場合に、1割程度の計算時間削減を確認した。一方で、予測精度はDrop Edgeの方が1割程度低い（Accuracyで比較）という結果も得られた。計算量と予測精度のトレードオフの解消は今度の課題である。

## 英文抄録

This project explores an application of representation learning by deep learning toward drug discovery. Specifically, we reproduce Neural Finger Print (NFP), which learns the finger print of compounds from data, and propose the randomized algorithm of NFP, called Drop Edge. We apply these two methods to the activity prediction task of 72 assays and find that Drop Edge can reduce the time complexity, especially when the radius is large, by approximately 10%. On the other hand, prediction accuracy is also reduced by approximately 10%. So there remains the trade-off between time complexity and accuracy.

## Keywords
Neural Finger Print、機械学習、深層学習、QSAR

## 概要

## 背景と目的

　定量的構造活性相関(Quantitative Structure-Activity, QSAR)は化合物の化学的な性質と毒性や体内の酵素に対する代謝の度合いなど、生化学的性質間の量的な関連を指す。QSARは創薬での候補化合物探索おいて、初期のスクリーニングに利用される。精度の良いQSARは、スクリーニング精度向上に貢献し、創薬コストの削減、化合物のヒット率上昇につながる。

## 手法

　まず、ECFP, NFP, Drop Edgeの手法を解説する。

![ecfp-nfp](image/ecfp_nfp.png)
<center>
<figcaption>図1：ECFP（左）とNFP（右）の比較（[Duvenaud+15]より引用）
</center>

</figure>


　化合物の最大次数をNとする。NFPでは、予め、長さfのベクトルを長さdのベクトルに変換するニューラルネットHと、各次数n (n=1,...N)に対し、長さfのベクトルを同じ長さfのベクトルに変換するニューラルネットWnを用意する。Hは各要素が0以上で要素の合計が1になるように正規化され、出力が確率として解釈できるようにする。典型的には、Wnは1層の全結合層 + 活性化関数、Hは1層の全結合層 + Softmax関数で実現される。

![ecfp-nfp](image/nfp.png)
<center>
<figcaption>図2：NFPでのベクトル更新</figcaption>
</center>

</figure>

　最後に、NFPの乱択化であるDrop Edgeを解説する。NFPでは、ある原子に割り振られた長さfの実数値ベクトルを更新する際、その原子に隣接するすべての原子のベクトルを用いた。Drop Edgeでは、隣接する全ての原子を用いる代わりに、ランダムに1個の原子を選びそれのみを利用する。化合物をグラフと見た時の最大次数をNとすると、NFPでは、N+1 種類のニューラルネット(W1, …, WN, H)を用意する必要があった。それに対して、Drop Edgeで用意すべきニューラルネットは2つのみである。これにより学習すべきパラメータ及びメモリ使用量が減少する。また、NFPに比べ行列計算の回数が減るので、所要時間が削減されることが期待出来る。もちろん、乱択化により訓練時に利用する情報は減少するため、予測モデルの精度が落ちる可能性がある。実験では乱択化による精度劣化の度合いを検証した
 
## 実験

　72アッセイに対して、化合物の活性の有無を予測する2値分類問題をNFPとDrop Edgeを用いて解き、その精度と実行速度を比較した。
　

　実験は以下のようなパラメータで行った。後述の結果の通り、この実験設定で、1つのアッセイの実験に10-20時間程度要する。

![acc -result](image/acc.png)
<center>
<figcaption>図3：Accuracyの最大値のbox plot</figcaption>
</center>
</figure>

　図4は72アッセイに対する計算時間をプロットしたものである。本手法の計算量は半径Rに比例するものであり、実際の所要時間でもR=6の所要時間は、R=3の倍程度時間がかかっている。NFPとDrop Edgeの所要時間を比較すると、R=3では大きな改善は見られなかったが、R=6では、1割程度の所要時間削減がなされていることがわかる。
<figure>

![time -result](image/time.png)
<center>
<figcaption>図4：実行時間のbox plot（横軸の単位は秒）</figcaption>
</center>
</figure>

## まとめ、今後の課題

　本プロジェクトでは、深層学習による表現学習の創薬への応用として、化合物のFinger Printを学習ベースで獲得するNeural Finger Print (NFP)を再現し、さらにその乱択アルゴリズム化であるDrop Edgeを提案した。さらに、両者の化合物活性予測のタスクをTSUBAMEで実験し、その予測精度と速度を比較した。

## 参考文献

[Bohacek+96] "The art and practice of structure-based drug design: a molecular modeling perspective". Med. Res. Rev. 16: 3–50.

[Dahl+14] Dahl, G. E., Jaitly, N., & Salakhutdinov, R. (2014). Multi-task neural networks for QSAR predictions. arXiv preprint arXiv:1406.1231.

[Duvenaud+15] Duvenaud, D. K., Maclaurin, D., Iparraguirre, J., Bombarell, R., Hirzel, T., Aspuru-Guzik, A., & Adams, R. P. (2015). Convolutional Networks on Graphs for Learning Molecular Finger Prints. In Advances in Neural Information Processing Systems (pp. 2215-2223).

[Thomas+14] Thomas Unterthiner, Andreas Mayr, Günter Klambauer, Marvin Steijaert, Jörg Kurt Wegner, Hugo Ceulemans, Sepp Hochreiter. Deep Learning as an Opportunity in Virtual Screening. Deep Learning and Representation Learning Workshop: NIPS 2014