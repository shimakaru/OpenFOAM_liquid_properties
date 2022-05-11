## OpenFOAM_liquid_properties ##

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
    sed -i -e s/'IC8H18'/'gasoline'/g gasoline.C
    sed -i -e s/'IC8H18'/'gasoline'/g gasoline.H
    sed -i -e s/'IC8H18'/'gasoline'/g gasolineI.H
    rm IC*
    ls

これにより，コメントも含めたテキストファイルの中身の「IC8H18」という文字列が「gasoline」に置換されたファイルが3つ出来上がる．
これをもとに改造を行っていくこととなる．

## 2.ソースを編集

ここからは各ファイルをテキストエディターで開き編集していく．編集内容は省略．
細かい方はdescription等も弄った方がよいが面倒なので必要箇所だけ，「gasoline.C」を編集する．

## 3.コンパイル

コンパイル準備として以下の編集を行う．「$WM_PROJECT_USER_DIR/src/thermophysicalModels/thermophysicalProperties/Make/files」内の

    LIB = $(FOAM_LIBBIN)/libthermophysicalProperties
    
↓

    LIB = $(FOAM_USER_LIBBIN)/libthermophysicalProperties
    
に修正．

さらに同ファイル内でコンパイルするファイルを追記する．今回であれば，

    liquidProperties/gasoline/gasoline.C

を追記．そしてコンパイルする．

    cd $WM_PROJECT_USER_DIR/src/thermophysicalModels/thermophysicalProperties
    wmake

以上．

    

