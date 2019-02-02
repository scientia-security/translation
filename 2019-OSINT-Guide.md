OSINT 2019 Guide
====

# 注意事項
- 本資料は、セキュリティ専門家のTek氏に許可をもらい、ブログ記事『[2019 OSINT Guide](https://www.randhome.io/blog/2019/01/05/2019-osint-guide/)』を翻訳したものです。
- 内容については、最大限の努力を持って正確に期していますが、本書の内容に基づく運用結果については責任を負いかねますので、ご了承ください。
- 他の翻訳は、『[Scientia Securtity on GitHub](https://scientia-security.github.io/)』を参照してください。
- ブログは『[セキュリティコンサルタントの日誌から](https://www.scientia-security.org/)』を参照してください。

# 概要
最近多数のOSINTプロジェクト（Open Source Intelligence）を実施しています。2019年新年のお祝いとして、私が学んだ多くのテクニックを紹介します。もちろん、これは完璧なガイドではありませんが（そして、どんなガイドも完璧にはなり得ないでしょう）、この資料が初心者にとってOSINTを学ぶ助けになり、経験豊富なOSINTハッカーが新しいテクニックを学ぶ助けになることを願っています。

# 方法論
伝統的なOSINT方法論を端的にいえば、以下のように言えるでしょう。
- 要求定義（何を探しているのか？）
- データ収集
- 収集した情報の分析
- ピボットと報告（収集した情報をもとにピボットを行い、更なる要求定義を行うか、調査を終了し報告書を書くか、決定します。）

この方法論はすこし直観的であり、あまり助けにならないかもしれません。しかし、私はそれでも定期的にこのフレームワークに戻ることが重要だと考えています。そして、このプロセスを反復できるように時間を取ることをお勧めします。調査中、収集されたデータ量がわからなくなり、調査の方向性を把握するのが難しくなります。この場合、少し休憩を取り、まずはStep 3とStep 4にに戻ることを推奨します。つまり、分析して、何を発見できたか整理し、ピボットしたり、答えを引き続き探すべき新しい質問（あるいはより具体的な質問）を定義する手助けをなることを列挙します。


![image](https://user-images.githubusercontent.com/23012571/52159164-b5bced00-26e3-11e9-9ae3-3a273463ed32.png)


他にできるアドバイス：
* **決してあきらめないこと。** 情報を得るためにあらゆる可能性を試し、やりつくしたと感じる瞬間が来るでしょう。でもあきらめないでください。すこし休憩を取っりましょう（１時間、あるいは１日別のことをしても良いかもしれません）。そして持っているデータを改めて分析をやり直し、別の視点から再度捉えなおしてみまてください。ピボットできる新しい情報があったでしょうか？もし、そもそも立てた調査用の質問が間違っていた可能性ないでしょうか？Justin Seitzが最近執念に関する[ブログ記事](https://medium.com/@hunchly/osint-tenacity-hunchly-89727adc471a)を投稿し、様々な事例を紹介しています。
* **証拠を保存すること。** オンラインにある情報はすぐに消失します。一回のOpSecのミス、例えばTweetに「Like」ボタンを押してしまい調査対象の人が疑いだしたら、突然全てのソーシャルメディアアカウントとWebサイトが消えてしまう可能性があります。そのため、証拠は保存しておく必要があります。スクリーンショット、アーカイブ、Webアーカイブ（より詳細には後で議論します）、あるいは自分にあった方法で構いません。
* **タイムラインが適切であること。** フォレンジック調査では、タイムラインとイベント発生時のピボットが同じタイミングであることが重要です。OSINTにおいて、タイムラインはそこまで重要ではありません。しかし、データを体系化する上でタイムラインは重要なツールとなります。いつWebサイトが作成されたのでしょうか？いつFBアカウントが作成されたのでしょうか？最後にブログが投稿されたのはいつでしょうか？こうした情報を一度テーブルに並べることで、自分が探していることについて良い視点が得られるでしょう。

そして、私が有益だと思う二つのメソッドがあります。最初の方法はフローチャートです。これは、データの種類（例えば、e-mail）に基づきより多くの情報を探し出すためのワークフローを表現した図です。私が見た中で最も良い図は、[IntelTechniques.com](https://inteltechniques.com/blog/2018/03/06/updated-osint-flowcharts/)を運営しているMichael Bazzell氏が提唱している図です。例えば、E-mailアドレス情報を調査する場合、Michael Bazzell氏のワークフローを以下に示します。

![image](https://user-images.githubusercontent.com/23012571/52159251-bb670280-26e4-11e9-9c4e-ff08b9368ab8.png)
*Michael Bazzell氏によるメールアドレスに対するOSINTワークフロー*


これ以降、私は自分独自の調査ワークフローを構築し、時間をかけて少しづつ自分が見つけたテクニックを付け加えて改善していくのがよい考えだと思うようになりました。

私が推奨するもう一つの方法は、特に長期間の調査に有効で、[ACH（Analysis of Competing Hypotheses）](https://en.wikipedia.org/wiki/Analysis_of_competing_hypotheses)という方法です。この方法論は、CIAにより1970年代に開発された方法で、アナリストが分析時にバイアスに影響されないようにし、異なる仮説を注意深く評価することを助けてくれます。これは非常に時間を必要するツールであるため粘り強く使う必要があります。しかしもし１年間の長い調査中に調査指針を失った場合は、注意深く仮説を評価することを支援する上でよいプロセスになるのでしょう。

# 自分のシステムを準備する
調査に入る前に、調査対象の人たちに警戒されないようにOpSecの観点から考えるべきことがあります。目立たない個人のWebサイトへアクセスすればIPアドレスを露呈することになり、自身の場所や所属をさらすことになります。ほかにも、個人のソーシャルメディアアカウントを利用し、間違って「Like」ボタンを押してしまう可能性も考えられるでしょう。
そのため、自分が調査する時は以下のルールに従うようにしています。
* **調査用ブラウザからの全てのアクセスは、商用のVPNやTORを利用することです。** 多くの商用VPNは、異なる国でサーバを提供しており、Torは[出口ノードとして特定の国を選択すること](https://www.wikihow.com/Set-a-Specific-Country-in-a-Tor-Browser)ができます。そのため、国を選択することで、不用意なコンテキストとその痕跡を残さずに済みます（USの組織から、USのために調査を実施しているなど）
* スキャンやクローリングのタスクは、自分との関係性がない安いVPSから行う必要があります。
* 調査専用のソーシャルメディアアカウントを利用し、偽名で作成します。

こうした試みをすることで、臨んだ通り自分の姿を隠しながら調査をすることができ、調査対処から特定される可能性もほとんどありません。

# ツール
情報セキュリティの世界において、ツールは常にみんなの興味を引く話題でしょう。但し、持っているスキルではなく、数えきれないほどのツールのリストをレジュメ（CV）に列挙している人々を除いてですが。では、はっきり言ってしまいましょう。**ツールは基本的に問題ではありません。ツールを使って何をするかが問題となります。**もし、何をしているか理解できていなければ、ツールは訳に立たないでしょう。理解できない、あるいは評価できないデータの長いリストが並ぶだけです。ツールをテストして、コードを読み、自分自身のツールを作る必要がありますが、自分のやろうとしていることを確実に理解する必要があります。
完璧なツールキットが存在しないのは当然です。最も良いツールキットは、自分がい知っており、利用しやすく、完璧に使いこなすことができるツールです。しかし、私が使っているツール、あるいは読者が興味をもつであろう他のツールを紹介したいと思います。

## Chromeとプラグイン
私は調査用ブラウザとして、Chromeを使います。なぜなら、HunchlyというツールがChromeでしか使えないためです。ここでは、さらに有益なプラグインを紹介しておきましょう。
* [archive.is Button](https://chrome.google.com/webstore/detail/archiveis-button/cgjpabpjaocpgppajkeplhbipbdippdm) は、archive.isに素早くWebページを保存することができます（詳細は後に記載）
* [Wayback Machine](https://chrome.google.com/webstore/detail/wayback-machine/fpnmgdkabkmnadcjpehmlllkndpkmiak)は、archive.orgの中にあるアーカイブされたページを検索します。
* [OpenSource Intelligence](https://chrome.google.com/webstore/detail/open-source-intelligence/bclnaepfegjimpinlmgnipebbknlmmbh)は、多くのOSINTツールへの素早くアクセスできます。
* [EXIF Viewer](https://chrome.google.com/webstore/detail/exif-viewer/mmbhfeiddhndihdjeganjggkmjapkffm)は、イメージ内のEXIFデータを素早くみることができます。
* [FireShot](https://chrome.google.com/webstore/detail/take-webpage-screenshots/mcbpblocgmgfnpjjppndjkmgjaogfceg)は、スクリーンショットを素早くとることができます。

## Hunchly
私は最近、Hunchlyを使い始めましたが、非常に良いツールです。[Hunchly](https://hunch.ly/)はChromeのエクステンションであり、調査中に見つけた全てのWebデータをセーブ、タグづけ、検索することができます。基本的には、調査を開始するときにエクステンションにある「キャプチャ」のボタンをクリックするだけです。Hunchlyは、アクセスしたページ全てをデータベースに保存し、後でタグやノートを追加できるようにしてくれます。
これは、年間130USDかかりますが、このツールがもたらす利便性に比べればそこまで高いとは言えないでしょう。

![image](https://user-images.githubusercontent.com/23012571/52159352-590f0180-26e6-11e9-9ef1-b1f83280cd2f.png)
*Hunchlyダッシュボードのスクリーンショット*

## Maltego
[Maltego](https://www.paterva.com/web7/buy/maltego-clients/maltego.php)は、OSINTツールというより脅威インテリジェンスツールと呼ぶべきでしょう。しかしグラフは、調査データを分析し、表現するうえで最も良い方法になります。基本的に、Maltegoはグラフを表現し、グラフ内に新しいデータを見つけるために変換するためのGUIを提供してくれます（例えば、Passive DNSデータベースからドメインに紐づくIPアドレスなどを表示してくれます）。少し[高額](https://www.paterva.com/web7/quote.php)ですが、脅威インテリジェンスや攻撃基盤分析などをやっていればその価値はあるでしょう。（初年度999ドル。そして、ライセンス更新ごとに年499ドルです）。ほかにも、[Maltego Community Edition](https://www.paterva.com/web7/community/community.php)を使うこともできます。これは、データの変換やグラフのサイズなどに制約がありますが、小さい調査であれば十分すぎるほどの機能を提供してくれます。

![image](https://user-images.githubusercontent.com/23012571/52159364-bdca5c00-26e6-11e9-85bc-cbc76437b619.png)
*Maltegoのスクリーンショット(情報源: Paterva)*


## Harpoon
私は、[Harpoon](https://github.com/Te-k/harpoon)というコマンドラインツールを作成しました（このツールの詳細は、このブログ[記事](https://www.randhome.io/blog/2018/02/23/harpoon-an-osint-/-threat-intelligence-tool/)を参照してください）。これは脅威インテリジェンスツールとして作成しましたが、OSINT用コマンドを多数追加しました。これは、Linux環境にあるPython3で動き、オープンソースです。（多分、MacOSとWindowsOSでも動くと思います）
例えば、キーサーバーにあるPGPキーを探す際には、Harpoonを以下のように使います。
```
$ harpoon pgp search tek@randhome.io
[+] 0xDCB55433A1EA7CAB  2016-05-30      Tek__ tek@randhome.io
```
さらに、[プラグイン](https://github.com/Te-k/harpoon)に関する長いリストがあります。ぜひ追加すべき新しい機能を思いついたら、提案あるいは開発、あるいは[リクエスト](https://github.com/Te-k/harpoon/issues)を挙げてください。

## Python
しばしば、ツールを使っても簡単には終わらない特定のデータ収集と可視化タスクを早く切り上げたいと思うでしょう。その場合、自分でコードを書く必要があります。私の場合、Pythonを使うことが多いです。最近のプログラミング言語であれば同様に目的を達成できますが、私はPythonが持つフレキシビリティと利用可能な多数のライブラリを活用するため、Pythonを選んでいます。
Justin Seitz (Hunchlyの作者)は、PythonとOSINTについて言及していますので、彼のブログ[Automating OSINT](http://www.automatingosint.com/blog/)と彼の著書[Black Hat Python](https://nostarch.com/blackhatpython)はぜひ読んでみてください。

## 他のツール
OSINT用のツールはほかにも多数ありますが、全ての調査で有益とは言えないケースも多く経験しました。以下に、読者がチェックしておくべき他のツールを紹介します。個人的には、あまりフィットしませんでしたが、こうしたツールは非常に興味深く、良くできたツールです。

* [SpiderFoot](https://www.spiderfoot.net/)は多数の異なるモジュールを持つ、偵察用ツールです。非常に良いWebインターフェースを持っており、異なるタイプのデータ感の関係性を示すグラフを生成してくれます。私がこのツールを好まない点は、利用者のために全てを見つけてきてくれる魔法のツールと考えられてしまう点です。しかし、何を探しているか理解し、結果を分析してくれるツールはありません。そう、自分自身で研究し、全ての結果を一つづつ読まなくてはいけません。良いツールでよいインターフェイスを持っていますが、SpiderFootはその点についてはあまり役に立ちません。
![image](https://user-images.githubusercontent.com/23012571/52162445-6a243680-2717-11e9-8e45-c952b51f7c76.png)
*SpiderFootのスクリーンショット(情報源:[spiderfoot.net](https://www.spiderfoot.net/))*

* [recon-ng](https://bitbucket.org/LaNMaSteR53/recon-ng)は、素晴らしいCLIツールで、異なるプラットフォーム、ソーシャルメディア、脅威インテリジェンスプラットフォームから情報を取得することができます。正直、Harpoonに正直よく似ています。なぜ使わないかというと、Harpoonが自分ニーズに必要な機能を提供してくれており、提供されるシェルインターフェイスが苦手であるためです。
* [Buscador](https://inteltechniques.com/buscador/)は、Linuxの仮想マシンで、異なるOSINTツールが含まれています。私は、いつも自分用にカスタマイズされたシステムを好みますが、ツールを一つづつインストールする手間なく新しいツールを試すことができる良い方法です。

# さあ始めよう！！
さて、それでは具体的な内容に入っていきましょう。OSINT調査において何が助けになるでしょか？

## 技術的なインフラストラクチャ
技術的インフラストラクチャの分析は、脅威インテリジェンスとオープンソースインテリジェンスが交差する点です。しかし、いくつかの点で調査の重要なパートになり得ます。
さて、読者が探すべきは以下の通りです。

* **IPとドメイン**：この目的のツールは多く存在しますが、[Passive Total](https://community.riskiq.com/)（現在は、RiskIQ）が最も良い情報源になると思います。Webインターフェイスから1日１５クエリまで、API経由でも１５クエリまで無料で使えます。ほとんどこのツールを使っていますが、[Robtex](https://www.robtex.com/)、[HackerTargetand](https://hackertarget.com/reverse-dns-lookup/)、[Security Trails](https://securitytrails.com/)も他の良い代替ツールになります。
* **証明書**: [Censys](https://censys.io/)は素晴らしいツールです。しかし、より知られておらず少し劣りますが[crt.sh](https://crt.sh/)も非常によい証明書の透明性データベースです。
* **スキャン**：IPアドレス上でどのようなサービスが動いているか確認することは有益です。[Nmap](https://nmap.org/)を使って自分でスキャンすることもできますが、全てのIPv4アドレスに定期的にスキャンしてくれるプラットフォームを活用することもできます。その二つのプラットフォームこそ、[Censys](https://nmap.org/)と[Shodan](https://www.shodan.io/)です。この二つは異なる側面に焦点を当てており、それぞれの特徴を知り、両方を使いこなす必要があります（ShodanはIoTに焦点を置いており、CensysはTLSに焦点を置いています）。[BinaryEdge](https://www.shodan.io/)は最近登場し急速に発展している代替とプラットフォームです。より最近では、[Fofa](https://fofa.so/)と呼ばれる中国プラットフォームが登場しました。別の情報源として、[Rapid7 Open Data](https://opendata.rapid7.com/)が挙げられますが、スキャンファイルをダウンロードする必要があり、自分で分析を行う必要があります。最後にIPアドレスに関する時系列データはプラットフォームの変遷を理解する上で金脈となることを指摘したいと思います。Censysはこうしたデータを有償プランでしか提供してくれません（アカデミックの研究者は無料で利用することができます）が、ShodanはIPごとにこうしたデータを提供してくれ、非常に素晴らしいです。**harpoon shodan ip -H IP**がコマンドがもたらす内容はぜひ確認してください（Shodanのライフアカウントを支払う必要があります）
* **脅威情報**：OSINTではあまり重要ではありませんが、ドメイン、IP、URLに対する悪性のアクティビティを確認することは常に興味深いことを教えてくれます。これを行うために、私は[Passive Total](https://community.riskiq.com/)と[AlienVault OTX](https://otx.alienvault.com/)に依存しています。
* **サブドメイン**：あるドメインに対するサブドメインのリストを取得する方法は様々あります。例えば、Google検索（Site：ドメイン）を使う方法から、証明書に記載されている代替ドメインを探す方法などが挙げられます。[Passive Total](https://community.riskiq.com/)、[BinaryEdge](https://www.shodan.io/)は、この機能を実装しています。そのため、初期リストを得るためであれば、こうしたサービスに直接クエリを投げることが手っ取り早いでしょう。
* **Googleアナリティックスとソーシャルメディア**：最後の情報は、とても興味深いもので、複数のwebサイトで、同じGoogleアナリティックス・アドセンスIDを使っているか確認することです。このテクニックは2015年発表され、[ココ](https://www.bellingcat.com/resources/how-tos/2015/07/23/unveiling-hidden-connections-with-google-analytics-ids/)に詳しく書かれています。こうしたコネクションを探すためには、私は[Passive Total](https://community.riskiq.com/)、[SpyOnWeb](http://spyonweb.com/)、[NerdyData](https://nerdydata.com/)を使っています。 ([publicwww](https://publicwww.com/)は、別の有償サービスとして知られています)。

# 検索エンジン
コンテキストに依存して、調査中は異なる検索エンジンを使い分けてるかもしれません。私は、そのほとんどをGoogle、Bing（欧州・北米用）、Baidu（アジア用）、Yandex（ロシア・東ヨーロッパ）に依存しています。
もちろん、最も基礎的な調査ツールは、検索演算子です。Googleの検索演算子のリストは[ココ](http://www.googleguide.com/advanced_operators_reference.html)にあります。以下に最も興味深いものを抜粋しました。
* 以下のブーリアン型の論理演算子を使いクエリを結合することができます。**AND**、**OR**、**+**、**-**
* **filetype**：特定のファイル拡張子を検索を行います。
* **site**：特定のWebサイトへフィルタを書けます。
* **intitle**と**inurl**：タイトルやURLにフィルタを行います。
* **link**：特定のURLへのリンクを持つWebサイトを探すことに特化します。（2017年に非推奨になりましたが、現在でも一部機能します）


以下に例文を示します。
* **NAME + CV + filetype:pdf**：この組み合わせは特定のCVを探し出すのに役立ちます。
* **DOMAIN - site:DOMAIN**：Webサイトのサブドメインを探すのに役立ちます。
* **SENTENCE - site:ORIGINDOMAIN**：特定の文章をコピーしたり剽窃したサイトを探し出します。

よく詳しくなるためには、以下を参照してください。
* [Mastering Google Search Operators in 67 Easy Steps](https://moz.com/blog/mastering-google-search-operators-in-67-steps)
* [Google Hacking Database](https://www.exploit-db.com/google-hacking-database)

# 画像
画像の観点では、二つのことを知っておくべきでしょう。どのように画像に関する追加情報を探し出すか、そしてどのように類似した画像を探し出すかです。

追加情報を探し出すためには、最初のステップとして、**EXIF情報**に注目しましょう。EXIF情報は、イメージが作成されたときに付与されるメタデータであり、作成時刻、使われたカメラの情報、さらにはGPS情報が付与されていることもある非常に面白い情報を含んでいます。これらを確認するため、私はコマンドラインツールである[ExifTool](http://owl.phy.queensu.ca/~phil/exiftool/)を使うことが多いですが、[Chrome](https://chrome.google.com/webstore/detail/exif-viewer/kbnpbnmjmgabkfemdehelbgdppngihhg)や[Firefox](https://addons.mozilla.org/en-US/firefox/addon/exif-viewer/)向けに提供されているExif Viewerアドオンも非常に使いやすいと思います。さらに、面白い機能を備えている[Photo Forensic website](https://29a.ch/photo-forensics/)を使うのも一つの手でしょう。(他の代替手段として、[exif.regex.info](http://exif.regex.info/exif.cgi)やFoto Forensics(https://fotoforensics.com/)も挙げられます)。
似たようなイメージを探すためには、[Google Images](https://images.google.com/?hl=fr)、[Bing Images](https://www.bing.com/images/)、[Yandex Images](https://yandex.com/images/)、[TinyEye](https://tineye.com/)を使えばよいでしょう。TinyEyeは使いやすいAPIを提供しており（使い方は[ココ](http://www.automatingosint.com/blog/2015/11/osint-youtube-tineye-reverse-image-search/)を参照のこと）、Bingは[画像の特定の部分を使って検索すること](https://www.cnet.com/news/bing-visual-search-within-images/)ができる有益な機能を持っています。[remove.bg](https://www.remove.bg/)などのツールを使って、イメージのバックグラウンドを取り除くことで、より良い結果を得ることができます。
例えば、場所を見つけ出すなど、画像の内容を分析する楽な方法はありません。どの国が候補に挙がるか推測するためには、画像の中に移りこんでいる特定の目印を探し出す必要があります。そして、ネット上で検索を行い、衛星画像と比較していく必要があります。このテクニックについて学ぶためには、Bellingcatが実施した興味深い調査結果が、[ココ](https://www.bellingcat.com/resources/case-studies/2015/03/03/stunt-geolocation-verifying-the-unverifiable/)や[ココ](https://www.bellingcat.com/resources/case-studies/2014/07/18/identifying-the-location-of-the-mh17-linked-missile-launcher-from-one-photograph/)にありますので参照してください。

さらに学ぶためには、以下のリソースを参照してください。
* Bellingcatによる[Metadata: MetaUseful & MetaCreepy](https://www.bellingcat.com/resources/how-tos/2015/04/24/metadata-metauseful-metacreepy/)
* First Draft newsによる[The Visual Verification Guide](https://firstdraftnews.org/wp-content/uploads/2017/03/FDN_verificationguide_photos.pdf?x40896)

# ソーシャルネットワーク
ソーシャルネットワークの分野では、多くのツールが存在します。しかし、プラットフォームに多く依存します。以下に有益なツールとテクニックを抜粋します。
* **Twitter**：APIは、ツイートが公開された時間とツールを教えてくれます。x0rzが開発した[tweets_analyzer](https://github.com/x0rz/tweets_analyzer)は、アカウントのアクティビティの概要をまとめてくれるツールです。メールアドレスからTwitter IDを探してくる方法も複数存在しますが、[少し特殊な方法](https://booleanstrings.com/2018/05/02/how-to-find-the-twitter-id-from-an-email-address/)を使います。
* **Facebook**：Facebookの調査で最も良いリソースは、[Michael BazzellのWebサイト](https://inteltechniques.com/)でしょう。特に彼の[独自ツール](https://inteltechniques.com/osint/facebook.html)のページは秀逸です。
* **LinkedIn**：LinkedIn上で私が見つけた最も有益なテクニックの一つは、[メールアドレスからLinkedInプロファイルを見つける方法](https://inteltechniques.com/blog/2018/04/25/linkedin-profiles-by-email/)です。

# キャッシュプラットフォーム
調査時において、素晴らしい情報源になるものの一つに、Webサイトのキャッシュを作成するプラットフォーム群が挙げられます。なぜなら、Webサイトは押したり、Webサイトの時系列的な変遷を分析できるためです。こうしたプラットフォームは、自動的にWebサイトをキャッシュするものもあれば、リクエストに応じてキャッシュするものもあります。
* **検索エンジン**：多くの検索エンジンは、クローリングしたときのWebサイトのコンテンツをキャッシュとして保存します。これは非常に有益であり、多くのサイトのキャッシュを見ることができます。但し、最後にキャッシュされるタイミング（多くの場合１週間以内だと思います）を管理できないこと、すぐに削除されてしまう事実も抑えておく必要があります。そのため、もし面白い情報をキャッシュ上で見つけたら、すぐに保存することをお勧めします。私は[Google](https://support.google.com/websearch/answer/1687222?hl=en)、[Yandex](https://yandex.com/support/browser/personal-data-protection/cache-memory.html)、Bingなどの検索エンジンのキャッシュを使っています。

* **インターネットアーカイブ**：[Internet Archive](https://archive.org/)は、インターネットに公開された全てを保存するという目的で動いているプロジェクトです。この中には、自動的にクローリング対象のWebページを保存するだけでなく、そのサイトの変遷も巨大なデータベースとして保存しています。彼らは、[Internet Archive Wayback Machine](https://web.archive.org/)と呼ばれるWebポータルを提供しており、Webサイトの変遷を分析する上で非常に優れた情報源です。一つ知っておくべき重要なことは、Internet Archiveは要請があればコンテンツを削除するということです。（実際、[Stalkerware company Flexispy](https://motherboard.vice.com/en_us/article/nekzzq/wayback-machine-deleting-evidence-flexispy)の件で、削除したことがあります）。そのため、保存しておく必要があるコンテンツは、別の場所に保存しておく必要があります。
* **他の手動キャッシュプラットフォーム**：私は、Webページのスナップショットを保存でき手、他の人が取得したスナップショットを閲覧できる[archive.today](https://motherboard.vice.com/en_us/article/nekzzq/wayback-machine-deleting-evidence-flexispy)を好んで使っています。多くの調査でこのサイトへ依存しています。[perma.cc](https://perma.cc/)も良いですが、無料アカウントでは１か月１０リンクまでしか使えないという制限があり、プラットフォームは、図書館や大学に焦点を当てています。このソースコードは[オープンソース](https://github.com/harvard-lil/perma)で提供されており、キャッシュプラットフォームを独自で構築したいと考えた場合、間違えなくこのソフトを使うでしょう。
Webキャッシュが存在するか一つづつ手作業で確認していくことはめんどくさいと考える人も多いでしょう。そのため、私は、Harpoonに簡単なコマンドを実装しました。
![image](https://user-images.githubusercontent.com/23012571/52163085-752f9480-2720-11e9-9da3-86624a33dbc4.png)

さらに、以前言及した[Hunchly](https://hunch.ly/)は、「レコーディング」機能を有効にした場合、アクセスしたあらゆるページをローカルアーカイブに自動的に保存してくれることも覚えておく必要があります。

# 証拠の取得
次のポイントに移りましょう。それは、証拠の取得です。証拠の取得は、調査における重要なフェーズの一つです。特に、調査が長引く場合には非常に重要です。間違えなく、Webサイトが変更された、Twitterアカウントが削除されたなど、何度か自分が見つけた証拠をなくす経験をするでしょう。
![image](https://user-images.githubusercontent.com/23012571/52163096-b031c800-2720-11e9-9acb-c445ab8f9ee2.png)


覚えておくべきことは、以下の通りです。
* Internet Archiveに完全に依存することは難しいですので、他のキャッシュプラットフォームや可能であればローカルに保存することをお勧めします。
* 画像、文書を保存しましょう。
* スクリーンショットを取得しましょう。
* ソーシャルメディアに関する情報を保存しましょう。攻撃者はいつでもこうした情報を削除することができます。（Twitterアカウントにおいては、Harpoonは、ツイートとユーザ情報をJSON形式のファイルで保存するコマンドを用意しています。）

さらに勉強するためには、以下のリソースをご覧ください。
* [How to Archive Open Source Materials](https://www.bellingcat.com/resources/how-tos/2018/02/22/archive-open-source-materials/)（Aric Toler from Bellingcat）

# 短縮URL
短縮URLは、利用する際に非常に興味深い情報をもたらします。異なるサービスにおいて、統計情報を取得する方法をまとめました。
* **bit.ly**：URLの最後に、[https://bitly.com/aa+](https://bitly.com/aa+) のように **+** を追加してください。
* **goo.gl**：（もうすぐ非推奨になりますが）URLの最後に **+** を追加することで、**https://goo.gl/#analytics/goo.gl/[ID HERE]/all_time** のようになURLへリダイレクトします。
* **ow.ly**：hootsuite社が提供する短縮URLサービスですが、統計情報を見ることはできません。
* **tinyurl.com**：統計情報を取得できませんが、**http://preview.tinyurl.com/[id]** にすることでURL自体をみることができます。
* **tiny.cc**：**https://tiny.cc/06gkny~** のように**~** をつければ、統計情報を見ることができます。
* **bit.do**：**http://bit.do/dSytb-** にハイフン（-）をつければ、情報を取得することができます。(統計情報は非公開の場合があります)
* **adf.ly**：この短縮URLは、リンクへのリダイレクト時に広告を表示することで収益を上げることを提案しているサービスです。彼らは、**j.gs**や**q.gs**などその他のサブドメインを多数利用し、公開の統計情報を閲覧することができません。
* **tickurl.com**：**https://tickurl.com/i9zkh+** のように **+** をつければ統計情報へアクセスできます。

いくつかの短縮URLは連番のIDが使われている可能性があります。その場合、同時刻に作成された似たようなURLを推測できる可能性があります。このアイディアの事例は[このレポート](https://citizenlab.ca/2017/05/tainted-leaks-disinformation-phish/)を参照してください。

# 企業情報
いくつかのデータベースでは、企業情報を検索することが可能です。メインで利用されるものは、[Open Corporates](https://opencorporates.com/)や[OCCRP database of leaks and public records](https://data.occrp.org/)が挙げられます。その後、各国に応じたデータベースに依存していきます。例えば、フランスでは[societe.com](https://www.societe.com/)が有名ですし、米国であれば[EDGAR](https://www.sec.gov/edgar/searchedgar/webusers.htm)へ、イギリスであれば[Company House](https://beta.companieshouse.gov.uk/)へアクセスすべきでしょう（より詳細な情報は [ココ](ttps://www.bellingcat.com/resources/how-tos/2017/06/23/companies-house-short-guide/)から参照してください）。

# 参考文献
OSINTを学ぶ上で更なるリソースとしていかが挙げられます。
* Bellingcatによる貴重なリソース：[ガイド](https://www.bellingcat.com/category/resources/how-tos/)とコミュニティが作成した[情報リスト](https://docs.google.com/document/d/1BfLPJpRtyq4RFtHJoNpvWQjmGnyVkfE2HYoICKOGguA/edit)
* The Github repository：[Awesome OSINT](https://github.com/jivoi/awesome-osint)
* [The OSINT framework](https://osintframework.com/)
* [osint.link](http://osint.link/)

これですべてです。時間をかけてこのブログを読んでくれてありがとうございます。もし追加すべきリソースがあればTwitterなどから気軽にコンタクトしてください。
このブログ投稿は、[Nils Frahm](https://www.youtube.com/watch?v=izhGLGPmvIU)を聞きながら書きました。
更新１：[Yandex Images](https://yandex.com/images/)、[remove Background](https://www.remove.bg/)、[Photo Forensic](https://29a.ch/photo-forensics/#forensic-magnifier)を追加しました。このテクニックを教えてくれた[Jean-Marc Manach](https://twitter.com/manhack/status/1081945710638059523)と[fo0](https://twitter.com/fo0_)に感謝します。
