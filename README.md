# SENet
## ポイント
- SENetとは、SEブロックを組み込んだネットワークアーキテクチャの総称
- SEブロックとは、特徴量マップのチャネル間の相互依存性を明示的にモデル化するための構造
- 特徴量マップに、重要度に応じ**アテンション**をかける働きをする
- 既存のアーキテクチャにSEブロックを組み込むことで学習効率、精度が向上する<br>
<img alt="SE result" src="./image/se_result_train.png"></img>
- 上記図はSEブロックを組み込んだ場合のResNet, ResNeXt, Inceptionの学習効率改善結果を示している。オリジナルのものより学習収束が早くなっている<br><br>
<img alt="SE result" src="./image/se_result_val.png"></img>
- 上記図はSEブロックを組み込んだ場合の精度改善結果を示している。1%前後、オリジナルのものより精度が良くなっている
## SEブロック
- SEブロックは、チャネル間の相互依存性を明示的にモデル化し、特徴量マップの学習を強化することが狙い<br>
<img slt="SEBlock" src="./image/seblock.png"></img>
- SqueezeとExcitationとの 2つ のステップで次の畳み込みフィルター応答を再調整する
- **Squeezeステップ**：グローバル平均プーリングを使用してチャネル毎の統計を生成
- グローバル平均プーリングを用いる根拠：特徴量マップの空間情報(H×W)をチャネル記述子に圧縮するため。（特徴量マップ1ピクセル毎の受容野情報を集約）
- **Excitationステップ**：2つの全結合層によって、ゲート機構をモデル化。inputはSqueezeステップの圧縮特徴量マップ
- ゲート機構式： $s=Fex(z,W)=σ(g(z,W))=σ(W2δ(W1z))$ 。 $σ$ はsigmoid, $δ$ はReLU, $W1 ∈ R^{C/r \times C}$ および $W2 ∈ R^{C \times C/r}$ を表す
- $r$ は圧縮率という（FC層パラメータを調整するためのハイパーパラメータ）
- 最後にExcitationステップのアウトプットを入力特徴量マップに乗算する。つまり、各チャンネル依存関係を捉えた重みを元の特徴量マップに乗算し、セルフアテンションを行う<br>
<img alt="SEBlock 組み込み" src="./image/seblock_combin.png"></img>
- 上記は最終的なSEブロックはどのような構成になるか、またInceptionとResidualブロックに追加する方法を表している<br>
<img alt="SENet Resseries" src="./image/senet_resseriese.png"></img>
- 上記はSEブロックを組み込んだResNet, ResNeXtの構造を表している
