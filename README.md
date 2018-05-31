# how-to-win-Numbers3-lottery
これはナンバーズ3でどのように買うか（買わないか）を調べるためのプログラム（エクセルvba）です。当てたいという方はぜひ使ってみてください（数字は出せますが、処理に30分以上かかる可能性があります。現在この処理を早くするために開発を続けています。よってこれはまだ完成品ではありません）。

---------------------------------------------------How to use---------------------------------------------------------------------------

"Book1_20180505.xlsm"のSheet5のセルのaw12、aw13に数字(1から210まで、aw12 =< aw13)を入力して、実行ボタンを押してみましょう。ボックス番号の最適なラグとブランクが求まります。

"yosokuhenkan.xlsm"で前回から50回前までの当選番号をb列の3行目から貼り付けます。それでAUF列に移動して4行目を右へと参照していきます。数字があれば、それが次回購入すべきボックス番号です。

----------------------------------------------------------------------------------------------------------------------------------------

--------------------------------------------------------------原理----------------------------------------------------------------------

0.データはここからcsvをダウンロード　http://www.toe.jp/numbers34/download/

1.基本は全部買います。私が求めるのは例外、つまり買わないタイミングです。

2.まず、番号を3つ考えて結合します。

3.次に、小さい順に並べます（154 → 145）。それをボックス番号と呼びます。

4.それぞれの期待リターンを求めます。求め方は当選番号（ボックス番号）と同じボックス番号であればその回のリターンは((当選金額/200)-1)、そうでなければ-1としてください。それですべての回のリターンが求まったなら、その平均を求めます。それが期待リターンです。期待リターンが求まったなら、それが高い順になるようにボックス番号を並び替えます（エクセルにあるリターンが以上に高いのは、当選金額をそのままリターンに計上しているだけで期待リターンを並び替えるのに問題ありません）。

5.さて、皆様お気づきでしょうが、ボックス番号の期待リターンがマイナス(ここでは当選金額となっているので200未満)になっています。これは不必要なときに不必要なボックス番号を購入しているからです。そこで重要なのは、どのぐらい前に当選したら(ラグ)、どのぐらいの期間買わないか(ブランク)を求めることです。つまり期待リターンが最大になるボックス番号のそれぞれの桁の、ラグとブランクを求めること、それがこのプログラムの利用目的です。

6.例として、"Book1_20180505.xlsm"の、portfolioのシートにある、ボックス番号066の最適なラグは、1桁目(0)は25、2桁目は28、3桁目は30となります。また、最適なブランクは、1、2、1です。これはつまり、1桁目は25回前に当選したことがあれば1回だけ買わない、2桁目は28回前に当選したことがあれば2回だけ買わない、3桁目は30回前に当選したことがあれば1回だけ買わないということになります。さて、このボックス番号の期待リターンはというと、なんと驚きの37％！　ヤッター！　しかしちょっと待ってください。調べると、当選したのはたったの25回。偶然なんじゃないですかと思われた方も多いはず。実際標準偏差は1218％もあります。大丈夫なのか？　そこで私はt分布95％信頼区間の下限を求めることにしました。やはりゼロより大きいです。ここで必勝法が見つかったとしてもいいのですが、もっとリターンを高めることができます。

7.ここで、基本とは何かを振り返ります。基本とは全部買うことです。例外とは基本の”条件”から外れたボックス番号のことです。その条件とは、次のことです。sheet5の同じ行にある(lag1)列、(lag2)列、(lag3)列それぞれの数字が0ならば、その条件を満たすボックス数字を買うこと、それが基本です。ではその基本の条件を満たすボックス番号はどれぐらいあるのか。平均としては10個です。残りの200個は例外です。要するに、基本よりも例外のほうが多いということです。

8.さて、210個のボックス番号のそれぞれのラグやブランク、それぞれの個別のリターンが求まりましたでしょうか。それでは、sumproductで回ごとの買う数を求め、それをnとします。a=1/nとしますと、sumproductでそれぞれの回の期待リターンが求まります。それをすべての回で求め、平均を取ります。すると、最終的な期待リターンが求まります。"Book1_20180505.xlsm"にある期待リターンは118%、標準偏差は587%、95%信頼区間の下限は102%です。つまり、1賭けたらかなりの高い確率で2倍以上になって還ってくることになります。

9.それでは、"yosokuhenkan.xlsm"を用いて、次の当選番号を予測しましょう。前回から50回前までの当選番号と、先ほど求めたラグ、ブランクを用いて予測します。ボックス番号におけるlag1、lag2、lag3それぞれの数字が0なら、それが次に買う番号です。

----------------------------------------------------------------------------------------------------------------------------------------

最後に、これは未完成品です。今後改良して早く処理できるようにしていきます。予想としては1ボックス番号5分です。よろしくお願いいたします。

p.s.　5月の始めに1ボックス3分の処理に成功しました。成功した理由は、各回のリターンに余計な計算をさせていたことを改善したことにありました。今後の方針としましては、実数値を用いて分析するのではなく、理論的な手法で分析したいと考えています。具体的には、自然数の乱数を大量発生させて、それを当選番号とします。当選金額のリターンも理論値を用います。
