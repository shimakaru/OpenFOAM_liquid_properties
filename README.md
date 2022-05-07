## OpenFOAM_liquid_properties ##

### （未完） ###

OpenFOAMにliquidPropertieを追加する方法
(一応バージョン違いも考慮して作るが、ベースはv9)

この資料は、「
http://www.tfd.chalmers.se/~hani/kurser/OS_CFD_2009/ChenHuang/OFProject0122.pdf
,
https://github.com/snaka-dev/Training_begineer_OpenFOAM_Customize/blob/master/Text.md
」を参考に作成された。

一度もOpenFOAMの改造したことがない方は、「
https://github.com/snaka-dev/Training_begineer_OpenFOAM_Customize/blob/master/Text.md
」を先に読んで実行しておいてください。

恐らく役に立つ対象となる方は、噴霧燃焼のシミュレーションをしようとしている方でレアな液体燃料を噴霧しようとしている方。つまり、ほぼ日本には居ない。

## 1.ソースをコピー

「OpenFOAM改造あるある」ではあるのだが、ソースファイルのincludeにより、どれとどれが絡み合ってるのかを確かめることが面倒なので、srcファイルを丸ごと$WM_PROJECT_USER_DIRにコピーしておく。

    cp -r $FOAM_SRC $WM_PROJECT_USER_DIR/.

次に今回、改造の対象となる「liquidProperties」を見に行く（ここはバージョンによってコマンドが違うかもしれない）。

    cd $WM_PROJECT_USER_DIR/src/thermophysicalModels/thermophysicalProperties/liquidProperties
    ls

これによって，定義されている液体の物性値の名前一覧が取得できる．
次に「IC8H18」をディレクトリごとコピーし「gasoline」という名前にし，ディレクトリを移動する．

     cp -r IC8H18 gasoline
     cd gasoline
     ls

「IC8H18.C  IC8H18.H  IC8H18I.H」の3ファイルをgasoline用に改造する．

    cp IC8H18.C gasoline.C
    cp IC8H18.H gasoline.H
    cp IC8H18I.H gasolineI.H
    rm -r IC*
    ls


    

