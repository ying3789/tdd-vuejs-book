# overview

2021/4/9
目的など：

参考書：[Vue CLIがわかる！使える！TDDでつくるアプリ開発入門 (技術の泉シリーズ（NextPublishing）)](https://www.amazon.co.jp/dp/B088LX1NMN/)

端末：imac

localpath：/Desktop/homework/210505_tdd-vuejs-book

git ：[hoshimado/tdd-vuejs-book](https://github.com/hoshimado/tdd-vuejs-book/)
forkした

## 開発環境
```markdown
動作確認環境
・OS:Windows10
・Node.js:v8.11.3/v10.16.0
・fontawesome:v5.6.1
・express@4.17.1
・expressgenerator@4.16.1
・@vue/cli@3.11.0
・axios@0.19.0
・sqlite3@4.0.9
・mocha@6.2.0
・chai@4.2.0
・sinon@7.4.2
・promisetesthelper@0.2.1
```

## 感想
本は　2019/12/01 ごろのもの。かつwindows
古いかもしれないので、何をやっているかわかれば良い。あとから参考にする。
わかっていることを前提にすすめている
2019なので多分古い
ほかの初歩的なほうを先にやる

> Vue.jsのCLI版を利用することで、「Node.js環境でテストして、ブラウザー実行時にはブラウザー環境向けにトランスパイルして出力する」ことが可能となります。Node.js環境であれば、ファイル単位のスコープ機能があり、テスト駆動開発を支援する機能も利用し易いです。

> 「Vue.jsでもMochaでのテスト駆動開発ができた！」、「トランスパイルも怖くなくなった！（気がする）」と思ってもらえたなら嬉しく思います。 



## 再挑戦 2021/6/6 

CDNを用いた作成。
```
210505_tdd-vuejs-book/git/tdd-vuejs-book/chapter01/section1-1/index.html
```

ファイル単位でのスコープ機能がない。  
被テスト対象を限定してテストすることが困難。  

### VueCli版へ移行する際のポイント  

- htmlファイルでdiv id="id_app1"としていたdivを、vueファイルのtemplateタグ直下へ移動（div自体は消さない）
- js部分はvueファイルのscriptタグ配下へ移動
- vueインスタンス化するdivブロックとして指定したidは削除（id="id_app1"）
-  elタグに変わってnameタグを用いる。nameはファイル名と同じ(決まり？)（js）
- data:{}の部分を data:function(){return:{};}のように、関数の戻り値としてオブジェクトを返却するように変更
- cssはstyleタグ配下へ移動
- CLI版への具体的な移行手順

### Vue CLI環境をNode.jsに導入
- CDNで作成した3ファイルをMyClient.vue（任意の名前）にまとめる
- Vue CLIのスタートファイルとなるApp.vueに、MyCLient.vueを表示して、と記述
- devトランスパイルして（開発時の動作環境用）、ローカルのブラウザーで表示を確認
- buildトランスパイルして（公開用）、実際にHTTPサーバーに配置して、ブラウザー表示を確認


### vueCLIの導入から（p.216 14%の位置）

```
npm install @vue/cli -d
```

VueCLIのスケルトンを作成。「プロジェクト」とも呼ばれる。  
Vueフレームワークの構成ファイル一式。  
プロジェクト名を、cli-vueとする。
```
node_modules/.bin/vue create cli-vue
```

質問
manually select featres  

choose Vue version  
Babel  
Unit Testing   
あとはプロジェクトにあわせて任意でやる形。  
  

インストール完了をまつ。  
cli-vueの中に、プロジェクト（vueトランスパイル用の環境）が作成される。  
  

```
 cd cli-vue
 $ npm run serve
 ```

 - pulic/index.html：ブラウザー表示のベースファイル。
   - index.htmlにはsrc/main.jsが埋め込まれる
 - src/main.jsは、src/App.vueにしたがって表示するようにコードがかかれている。
 - src/App.vueは、src/components/HelloWorld.vueを読み込んで表示する

### fontawesomeを入れる時  
（public/index.htmlにstylesheetを読み込ませることもできるが、下記の方法が推奨されている）
(付録A)  
main.jsに記述する  

fontawesome公式  
https://fontawesome.com/v5.15/how-to-use/on-the-web/setup/using-package-managers  



install 
```
npm install --save @fortawesome/fontawesome-svg-core
npm install --save @fortawesome/free-solid-svg-icons
npm install --save @fortawesome/vue-fontawesome
```

main.jsで読みこみ
```
import { library } from '@fortawesome/fontawesome-svg-core'  //使う分だけ定義が必要
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { fas } from '@fortawesome/free-solid-svg-icons' //最終的には絞り込むべき
library.add(fas)
```

使う場所（MyClient.vue) に入れる  

```
<font-awesome-icon icon="pen" class="fa-2x"></font-awesome-icon>
```