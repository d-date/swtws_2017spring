みなさんこんばんは、上手くいっていれば僕は #yidev の懇親会で中華を食べ終わってコーヒー☕でも飲みながらタイムラインを眺めているはずです。コーヒー飲みながら登壇するのは初めてです。自己紹介はプロフィールを見てくださいね。今日はSwift4の話をします。
![](./imgs/swtws/swtws.001.jpeg)

---

Swift4に関しては、Swift.orgやswift-evolutionで毎日情報が流れていますが、僕自身ちゃんとキャッチアップできていませんでした。今日は出来るだけリソースを提示しながらお話したいと思います。

---

内容を取り違えているかもしれないので、遠慮なくご指摘いただければ幸いです。僕が議題を提供し、みなさんと議論できればと思います。さて、早速始めましょう。Swift4が何を目指しているのかはここに記載されています。https://goo.gl/5XSu7M

---

Swift4は今年2017年の下期のリリースを目標に開発が進んでいます。Swift4には２つ目標があります。一つはSwift3のソースコードの安定性の提供、もう一つは、標準ライブラリのABI安定性の提供です。この２つの目標達成のために、開発を2段階に分けています。
![](./imgs/swtws/swtws.002.jpeg)

---

Stage1: ソー​​ス安定性とABI安定性に必要なことにフォーカスしています。既存の言語機能のABIを根本的に変更しない機能や、標準ライブラリに対するAPIの破壊的な変更を意味する変更は、この段階では考慮していません。Stage1の主だった変更をご紹介します。
![](./imgs/swtws/swtws.003.jpeg)

---

ソースの安定化：異なるSwiftの言語バージョンが同じソースコードに共存できます。Swift3.1ですでに@ available にSwiftのバージョン指定ができるようになっています。https://goo.gl/ZXcAhi
![](./imgs/swtws/swtws.005.jpeg)

---

ABIの安定化：コード生成に関して、Swiftランタイムとのやりとりを含めた改善です。SwiftのパフォーマンスやSwiftの今後の方向性に影響することがあります(詳細は後述)。

---

ジェネリクスの改善：条件付き適合、再帰的なプロトコルの適合、Where節を使った条件付き関連型など、ジェネリクスの機能が強化されます。Genericsに関するプロポーサルを紹介します。ここにも記載されています。https://goo.gl/BPnfmK
![](./imgs/swtws/swtws.006.jpeg)

---

[SE-0142]関連型が適合するプロトコルにwhere節で条件を付けることができます。
![](./imgs/swtws/swtws.007.jpeg)

---

[SE-0143]条件付き適合(Conditional Confermance):
特定の条件にマッチしたときだけ、ジェネリクスをそのプロトコルに適合するようにできます。
![](./imgs/swtws/swtws.008.jpeg)

---

[SE-0157]あるプロトコルの関連型がプロトコル自身に適合できるようになります。例では、先程のwhereを使って条件をつけています。
![](./imgs/swtws/swtws.009.jpeg)

---

[SE-0148]subscriptに型変数が使えるようになりました。
![](./imgs/swtws/swtws.010.jpeg)

---

[SE-0104]Genericsの改善とは直接関係しませんが、整数型をプロトコルで再構成して、Generic Programingできるようにします。現在だとこのようにIntとInt8でOperatorの適応はできません。
![](./imgs/swtws/swtws.011.jpeg)

---

このようなプロトコルで再構成して、Generic Programingを可能にします。各プロトコルの詳細はこちらをご覧ください。
https://github.com/apple/swift-evolution/blob/master/proposals/0104-improved-integers.md
![](./imgs/swtws/swtws.012.jpeg)

---

Stringの再評価：Swift4では、Unicodeの正確さを維持しながら、よりパワフルで使いやすいStringにしようとしています。今日の後半でも少しStringのお話をします。

---

Memory Ownership Model：CycloneやRustをインスパイアしたモデルになるそうです。詳しくはここにありますが、難しくて追いきれていません😇 https://goo.gl/rBjn6E

---

以上がSwift4 Stage1における変更です。Stage 2に持ち越されているものもありますが、すべてSwift4で実現されるとうれしいですね！🚩。

---

残念なことに、ABIの安定化は延期されました😇 2/16にその旨がメーリスに流れています。コンパイル時間の改善やコンパイラの安定化を優先したということです。https://goo.gl/3KBvrX

---

そもそも”ABIの安定化”とはどういうことでしょう。具体的には、ABI Manifestoに書かれています。簡単に言うと、違うバージョンのSwiftでコンパイルされたフレームワークをそのまま使えるぜ！ということです。https://goo.gl/PZd6j9

---

Swiftのバージョンは統一する必要があるので、Carthageなどで導入したフレームワークは、現状ではVer.upの度にビルドし直す必要があります。ABIの安定化によって、Swiftのバージョンが先に進んでも、古いバージョンのフレームワークはそのまま使えます。

---

このままSwift4がリリースされても、先述のソース安定化によって、同じソースコードにSwift3.xとSwift 4を同居させることは可能です。ですが、Carthageなどで導入しているライブラリがSwift4対応していない場合はどうなるでしょう。

---

どんなに自分のソースコードをSwift4対応しても、フレームワークがSwift4でビルドされていなければコンパイルが通りません。OSS開発者のみなさま、ご対応よろしくお願いします🙏

---

そのままSwift3を使い続けることもできます。Swift4がリリースされると、`-swift-version 3`と`-swift-version 4`の2つのフラグが提供されます。https://goo.gl/zwlT0Z

---

Swift Version 3モードは、Swift 4のコンパイラで、Swift3.1のソースコードのビルドが可能になるモードです。ところで今日お集まりのみなさんはSwift3への移行はお済ですよね？

---

Swift Version 4は、Swift 4の破壊的変更を許容します。主に書き換えが必要なのは、String周辺です。Swift2.2からSwift3への変更に比べたらそんなに大したことないとのことですが、実際のところ試してみないとわかりません😇

---

ちなみにStage2で何が変わるのかも見ておきましょう。Stage2ではABIの変更に影響を与えない程度の既存機能の変更が含まれます。
![](./imgs/swtws/swtws.013.jpeg)

---

ソースの破壊的変更：Swift4モードの破壊的変更です。下記の方針で行われます。破壊的変更は続きますが、swift3モードがあるのでしばらくは大丈夫でしょうか。いつまで延命できるのか気になります。
![](./imgs/swtws/swtws.014.jpeg)

---

既存の標準ライブラリの改善：コレクションアルゴリズムや、Dictionaryの改善などが含まれます。これに関連するものとして次のプロポーサルを見てみます。
![](./imgs/swtws/swtws.015.jpeg)

---

[SE-0154] Dictionaryのキーの探索と、mutatingが非効率なので、カスタムコレクションが提供されます。
![](./imgs/swtws/swtws.016.jpeg)

---

現状ではLazyMapCollectionですが、keysもValueも独自のCollection型になります。
![](./imgs/swtws/swtws.017.jpeg)

---

Foundationの改善：SwiftではCocoa SDKをシームレスに動作させるために、Foundation APIの改良を進めています。

---

以上がStage2の変更内容です。Stage2は2月中旬からスタートし、4/1までに議論を終える予定でしたが、主にStringやMemory Ownership Modelについては継続して議論が続きます。https://goo.gl/GiWngg

---

ここからは、Stringの再評価によって、どのようにAPIが変わるのか見てみましょう。String Manifestに記載されています。https://goo.gl/en9vg3
![](./imgs/swtws/swtws.018.jpeg)

---

StringはPerlよりも優れていることを目指しています🤔 特に改善したいのは、人間工学、正確さ、パフォーマンスの３つです。Stringは標準ライブラリやFoundationによって複雑になっていて、205個もAPIがあるようです。

---

APIをわかりやすくするために、以下の変更が加わります。 https://goo.gl/GW7dWp
![](./imgs/swtws/swtws.019.jpeg)

---

現在レビュー中のProposalと一緒に見てみましょう。Stringを再びCollectionに、Substring型、Unicodeプロトコルの導入などが主となります。
![](./imgs/swtws/swtws.020.jpeg)

---

Swift1.0では、Collectionでしたが、問題があって2.0で、そうではなくなりました。が、その問題も特に問題じゃなかったから復活させるそうです。とにかくSwift4では再びCollectionになります。https://goo.gl/YAXqyI
![](./imgs/swtws/swtws.021.jpeg)

---

Swift3ではスライスを行え、SubStringが生成されますが、Stringとの共有ストレージとなります。これによってStringが開放されたあともSubStringが生き続け、メモリリークを引き起こします。Javaでは1.7からコピーに変更されています。
![](./imgs/swtws/swtws.022.jpeg)

---

スライスに関しては、マニフェストによると、以下のようになる予定です。
![](./imgs/swtws/swtws.023.jpeg)

---

Swift4では、Arrayに対するArraySliceのように、Substringという違う型となります。Substringが生成されるタイミングで、別のストレージにコピーされます。

---

StringとSubstringは型が違うを同様に扱えるようにするために、Unicodeプロトコルも導入されます。
![](./imgs/swtws/swtws.024.jpeg)

---

またUnicodeプロトコルの導入により、既存のunicodeCodecプロトコルはUnicodeEncodingプロトコルにリファインされます。
![](./imgs/swtws/swtws.025.jpeg)

---

これはまだProposalにはなっていませんが、ローカライズ周りも変わる予定です。
![](./imgs/swtws/swtws.026.jpeg)

---

文字列操作に適応できるオプションはこのようになります。この国際化の話もSwift4ではまだ不完全で、Swift5でも議論が続くようですね。https://goo.gl/k7WfzH
![](./imgs/swtws/swtws.027.jpeg)

---

ここからは現時点でAcceptされたプロポーサル、実装済みのプロポーサルを見てみましょう。※SPM関係は今回除外します。また、Swift4で入るかは未定のものも多いです。プロポーサルのステータスはここから。https://goo.gl/Un6n1b
![](./imgs/swtws/swtws.028.jpeg)

---

[SE-0110]タプルの表記について。現在は、タプルとして変数をクロージャで用いる場合も、その要素をそれぞれ変数として扱う場合も、引数に区別ありませんが、Swift4から関数型として宣言する場合は、二重括弧が必要となります。これはSwift4で入ります。
![](./imgs/swtws/swtws.029.jpeg)

---

[SE-0042]Unapplied Method Referenceです。現在はcurry化されますが、Typeと引数を同じネストで引数となるように変更されます。
![](./imgs/swtws/swtws.030.jpeg)

---

[SE-0075]Build Configuration Import Testの追加です。canImportでモジュール単位での判定が可能になります。今後OS単位のみでの判定のみでは難しくなるとの判断です。
![](./imgs/swtws/swtws.031.jpeg)

---

[SE-0153]@NSCopyingの挙動についてです。NSCopyingに準拠したこのようなPersonクラスがあるとします。
![](./imgs/swtws/swtws.032.jpeg)

---

現状では、@NSCopyingをつけても、initializerでコピーが起こりません。そのせいでディープコピーが起こらず予期せぬ挙動が起こることがあります。
![](./imgs/swtws/swtws.033.jpeg)

---

期待した挙動を実現するには、現状ではinitializerで.copy()を明示的に呼ぶ必要があります。
![](./imgs/swtws/swtws.034.jpeg)

---

これはわかりにくいので、@NSCopyingをつけると、常にコピーが起こるように修正されます。
![](./imgs/swtws/swtws.035.jpeg)

---

[SE-0156]クラスとプロトコルを組み合わせて型を宣言することができます。クラス、サブクラス、プロトコルの組み合わせは5つのルールにもとづいて適用されます。
![](./imgs/swtws/swtws.036.jpeg)

---

クラスであることはAnyObjectを使って示します。また、クラスとプロトコルを組み合わせることで、そのプロトコルに準拠したクラスのサブクラスであることを示します。
![](./imgs/swtws/swtws.037.jpeg)

---

プロトコル構成にAnyObjectとClass両方が含まれる場合は、Classが優先されます。Classが2つ含まれる場合、そのクラスは同じ、またはサブクラスである必要があります。サブクラスの場合はスーパクラスをオーバーライドします。
![](./imgs/swtws/swtws.038.jpeg)

---

プロトコル構成に含まれるtypealiasはプロトコル、クラス、AnyObjectに展開し、ルール1〜4を適応します。この5つのルールでクラスとプロトコルを組み合わせることができるようになります。
![](./imgs/swtws/swtws.039.jpeg)

---

[SE-0160]@objcの推論の挙動が変わります。簡単にまとめておくと、dynamicのついたもの、NSObject由来のクラスは従来は@objcをつけなくても@objcとして振る舞いましたが、swift4ではそうではなくなります。
![](./imgs/swtws/swtws.040.jpeg)

---

プロポーサルにはとても詳しく書いてありますが、ツイートにまとめられなかったので、一昨日potatotipsで先に発表しておきました。資料：https://goo.gl/bZ2u9O

---

さて、今後のSwiftを簡単に覗いてみましたが、いかがでしたでしょうか？Swift4の変更に関する議論はまもなく収束しますが、Swift5の議論も7月にはスタートしそうです。

---

今回登壇駆動でキャッチアップしてみましたが、内容が内容だけに結構きつかったです😇 ですが、今後Swiftの進化が垣間見えて楽しかったです。みなさんもぜひ今後のSwiftがどうなっていくのか、覗いたり、議論に参加してみてくださいね。どうもありがとうございました！
