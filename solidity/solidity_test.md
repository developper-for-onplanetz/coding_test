### Solidityエンジニア向け　コーディングテスト

### 概要
- 今回のテストでは、以下仕様に沿ったコントラクトコードの実装及び、goerliテストネットへのデプロイを行っていただきます。
  - ERC721規格
  - buy機能（NFTのmintと支払い通貨のtransferを実行）あり
  - buy機能内に購入制限（１アドレスあたりの発行上限及び購入時間の制限）機能あり

- 問題1に記載されているコントラクトコード、問題2に記載されているmigration用コードを実装し、truffle(or hardhat)パッケージを使って、デプロイをお願いします。
- デプロイにあたり、必要なパッケージはtestNFTディレクトリ内のpackage.jsonに記載されているので、適宜インストールをお願いします。
- テストが終わったら、問題1で作成したsolファイル、問題2で作成したデプロイ用コードの提出をお願いします。

#### 問題1
- 以下のsolidityファイルの中身を設問(1)~(5)に沿って実装せよ。

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

//変更箇所11/24
contract testNFT is ERC721URIStorage,Ownable,ReentrancyGuard
{
  //設問(1)
  //constantとして以下定数を設定せよ。
  // 1. 購入価格(設定値:500)
  // 2. 1アドレスあたりの発行上限(設定値:3)
  // 3. 総発行上限(設定値:20)


  //設問(2)
  //variable(状態変数)として以下変数を設定せよ。
  // 1. 発行済みトークンのindex
  // 2. mint可能時刻
  // 3. 支払い通貨のinterface(IERC20)
  // 4. 発行対象のtokenid(uint)のリスト
  // 5. 発行対象のtokenmeta情報(string)のリスト
  // 6. 支払い通貨の受け取りアドレス

  uint256 public index_;



  //設問(3)
  //constructorとして、
  // 1. tokenのname
  // 2. tokenのsymbol
  // 3. 設問(2)の1~6
  //の8つを引数として、設問(2)の各variableに設定せよ。
  //(但し、設問(2)の3については、引数はaddress型で渡し、constructor内でinterfaceを初期化すること)



  //設問(4)
  //以下仕様に基づき、アドレスを引数として、購入者の発行上限を取得する関数を作成せよ。
  //[前提]
  // 1.引数のアドレスが、0アドレスではないこと
  //[処理フロー]
  // 1.引数のアドレスが現在所有しているNFT数を取得
  // 2.NFT数が1アドレスあたりの発行上限を上回っている場合は0を戻す
  // 3.2.の条件に合致しない場合は、1アドレスあたりの発行上限からNFT数を差し引いた値を戻す



  //設問(5)
  //以下仕様に基づき、購入数量を引数として、購入処理を行う関数を作成せよ。
  //[前提]
  // 1.関数実行時の時刻がmint可能時刻を超えていること
  // 2.関数実行者の支払い通貨残高が、NFTの購入に必要な金額以上であること
  // 3.関数実行者の購入数量が、1アドレスあたりの上限を超えていないこと(設問4で作成した関数を用いる)
  // 4.関数実行者の購入数量+今までの総発行数が、発行上限を超えていないこと
  //[処理フロー]
  // --1~4は、引数で指定した購入数量の数だけ、ループ処理--
  // 1.発行対象のtokenidとメタ情報をリストから取得
  // 2.idを指定して、トークンをmint
  // 3.tokenURIにメタ情報をセット
  // 4.発行済みトークンのindexを加算
  // 5.支払い通貨の受け取りアドレス宛に、購入金額分の通貨を送付


}

```

#### 問題2
- 以下のmigration用jsファイルの中身を設問に沿って実装せよ。

```
const fs = require('fs');
//設問(1)
//artifactsを用いて、問題1で作成したtestNFTの呼び出しを記述せよ。

var token_ids = new Array();
var ipfs = new Array();
//設問(2)
// testNFT/data内にあるdata.jsonを読み込み、token_ids、ipfsに値を格納せよ。

//設問(3)
//コントラクトのデプロイ時の初期値を設定せよ。
//なお、初期値は以下の通り
// tokenのname:testNFT
// tokenのsymbol:testNFT
// 発行済みトークンのindex:0
// mint可能時刻:テスト開始時刻の3時間後
// 支払い通貨のaddress: 0xc5b4496C59818C34ff65d32b11824E4d425aDDb2
//

```

#### 問題3
- 問題1で作成したsolファイル、問題2で作成したmigration用jsファイルをtruffle(or hardhat) を用いて、compileした後、goerliテストネットにdeployせよ。
