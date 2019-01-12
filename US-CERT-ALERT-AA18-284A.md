世界のサイバーインシデントで利用される公開ツール
==

# 注意事項
- 本資料は、US-CERTのAlert（AA18-284A）"[Publicly Available Tools Seen in Cyber Incidents Worldwide](https://www.us-cert.gov/ncas/alerts/AA18-284A)"を翻訳した資料です。
- 他の翻訳は、『[Scientia Securtity on GitHub](https://scientia-security.github.io/)』を参照してください。
- ブログは『[セキュリティコンサルタントの日誌から](https://www.scientia-security.org/)』を参照してください。


# サマリ
本レポートは、オーストラリア、カナダ、ニュージーランド、イギリス、アメリカのサイバーセキュリティ専門機関の共同研究成果です。[^1][^2][^3][^4][^5]

[^1]:https://www.acsc.gov.au/
[^2]:https://cyber.gc.ca/en/
[^3]:https://www.ncsc.govt.nz/
[^4]:https://www.ncsc.gov.uk/
[^5]:https://www.us-cert.gov/

本レポートでは、世界中で発生している最近のサイバーインシデントで悪用する目的で利用されている公開ツールの利用に焦点を当てています。取り上げるツールは以下の５種類です。

1. Remote Access Trojan: JBiFrost
2. Webshell: China Chopper
3. Credential Stealer: Mimikatz
4. Lateral Movement Framework: PowerShell Empire
5. C2 Obfuscation and Exfiltration: HUC Packet Transmitter

ネットワーク防御者とシステム管理者を支援するため、これらのツールの効力を制限する方法とネットワーク内での利用を検知する方法についても言及します。

このレポートで取り扱う各ツールは、攻撃グループによって利用されるツールのごく限られた事例に過ぎません。そのため、ネットワーク防御を検討する際に、これらを網羅的なリストと捉えるべきではありません。

ネットワークを侵害するツールとテクニックと彼らが持つ情報は、決して国家やダークウェブにいる攻撃者の領分ではありません。現在、多くの機能を持った悪意のあるツールは、技術を持ったペネトレーションテスター、敵対的な国家アクターや組織犯罪グループ、アマチュアのサイバー犯罪者に至るまで、誰にでも無料で利用できるようになっています。

このアクティビティ・レポートで紹介するツールは、ヘルスケア、金融、政府機関、国防など多くの重要セクタにおける情報漏洩に利用されてきました。こうしたツールが広く自由に利用できる状態は、ネットワーク防御や攻撃グループの特定（アトリビューション技術）にとって障害となります。

攻撃グループが攻撃技術を高め続ける一方、こうした確立したツールと技術も使い続けているという事実が、研究に参加した５つの国の経験から明らかになりました。最も高度な攻撃グループですらも、こうした汎用的で公開されているツールを使い、目的を達成しています。

攻撃グループの目的が何であれ、最初のシステムへの侵害は一般的なセキュリティの脆弱性を悪用することを通じて成立します。パッチがあてられていないソフトウェアの脆弱性や適切に設定されていないシステムを悪用してアクセスを奪取することが攻撃グループが一般的に行う方法です。このアクティビティ・レポートで紹介するツールは、初期侵入が成功した後、被害者のシステム内で更なる目的を達成するために利用します。

## このレポートの利用方法
このアクティビティ・アラートで紹介するツールは、５種類のカテゴリーに分類されます。

1. RATs（Remote Access Trojans）
2. webshells
3. 認証情報奪取ツール（credential stealers）
4. 横断的侵害フレームワーク（lateral movement frameworks）
5. C2難読化ツール（command and control obfuscators）

アクティビティ・アラートは、各ツールによってもたらされる脅威の概要、攻撃グループによってツールがいつ、どこに展開されるか洞察に加え、検知を補助する方法と、ツールの効力を制限する方法についても言及します。

このアクティビティ・アラートは、ネットワーク防御の活動を向上させるための一般的なアドバイザリで終わります。

# 技術的詳細
## Remote Access Trojan: JBiFrost
最初に確認されたのは、2015年５月で、JBiFrost RATは、Adwind RATから発展したツールです。より遡ると2012年のFrutas RATがオリジナルのツールです。

RATとは、被害マシンにインストールすると、リモート管理のコントロールを許可するプログラムです。悪意のある使い方としては、（他の機能も多数ありますが）バックドアやキーロガーをインストールしたり、スクリーンショットを取ったり、データを持ち出したりすることが一般的です。

悪性のRATは、検知することが難しいとされています。なぜなら、通常実行プログラムの一覧に表示されないように通常設計され、正当なアプリケーションの挙動を模倣するためです。

フォレンジック分析を避けるため、RATは侵害したシステムにおいて、（タスクマネージャーなどの）セキュリティ機能や（Wiresharkなどの）ネットワーク分析ツールを無効にすることで知られています。

### 利用状況
JBiFrost RATは、サイバー犯罪者やスキルがあまり高くない攻撃グループによって典型的に利用されます。しかし、このツールが持つ機能は、国家に支援された攻撃グループにも利用されています。

他のRATは、標的型攻撃（APT）を行う攻撃グループによって広く使われています。その例として、航空宇宙産業や防衛産業に利用されているAdwind RAT、様々な産業に攻撃を仕掛けたAPT10が利用したQuasar RATが挙げられます。

攻撃グループは、更なる侵害のためにリモートアクセスを得るため、あるいは銀行の認証情報や知的財産、個人情報など価値ある情報を盗む取るために、悪性のRATを被害サーバへ配置し、我々の国のサーバを繰り返し侵害します。

### 機能

JBiFrost RATは、Javaベースで作成され、クロスプラットフォーム環境で動作し、多機能なツールです。Windows、Linux、MAC OS X、Androidなど、複数の異なるOSへ脅威をもたらします。

JBiFrost RATは、攻撃グループにネットワーク内を移動できるように機能を提供し、さらなる悪性ツールをインストールできるようにします。このツールは、請求書の通知、見積書のリクエスト、送金通知、荷物配送通知、支払い通知などメールの添付ファイルとして、あるいはファイルストレージサービスのリンクとして配布されることが一般的です。

過去の感染事例では、知的財産、銀行の認証情報、個人情報などを漏洩をしています。JBiFrost RATに感染した端末は、DDoS攻撃を実行するためのボットネットに使われることもあります。

### 事例

2018年の前半から、JBiFrost RATが、重要な国家インフラを持つ組織とそのサプライチェーンに属する組織への標的型攻撃において利用されていることを観測しています。我々の国にあるインフラに、RATが配置されている例は増加しています。

2017年の前半から、Adwind RATはSWIFT（国際銀行間通信協会：Society for Worldwide Interbank Financial Telecommunication）やネットワークサービスに由来したように見えるなりすましメールを通じて配布されました。

Gh0st RATの亜種を含む他の多くの公開ツール（RAT）は、世界中の被害で利用されていることが観測されています。

### 検知と防御

JBiFrost RATの感染には、いくつかの指標が含まれますが、それだけには限りません。

- セーフモードでコンピュータを再起動できないこと
- Windowsのレジストリエディタやタスクマネージャーを開けないこと
- ディスクの挙動やネットワークトラフィックなどの著しい増加
- 既知の悪性IPアドレスへの接続試行
- 難読化、あるいはランダムなファイル名を持つ新しいファイルやディレクトリの作成

システムとインストールされたアプリケーションにパッチが完全に適用され、アップデートされていることを保証することで、最大の保護が実現します。定義ファイルの自動更新と定期的なシステムスキャンを備えた最近のウイルス対策プログラムの使用することで、最近の亜種の大半を確実に停止することができます。そして、ウイルスの検出を一元的に収集し、RATの検出を効率的に調査できることを組織全体で確実にする必要があります。

感染が発生することを防ぐため、厳しいアプリケーションのホワイトリスト制御を推奨します。

JBiFrost RATを含むRATの初期感染メカニズムは、フィッシングメールを通じて行われます。そのため、フィッシングメールを特定して報告する仕組みを導入し、悪性メールが自組織の端末に侵害しないようにセキュリティコントロールを実装することにより、自組織のユーザに届くフィッシングメールを止め、JBiFrost RATの感染はを防ぐことを助けてくれるでしょう。英国国立サイバーセキュリティセンター（UK NCSC：United Kingdom National Cyber Security Centre）がフィッシング攻撃対策の資料を公開しています。

## Webshell: China Chopper 
China Chopperは、公開されており、手順書も良くそろっている、2012年頃から広範に使用されているWebshellツールです。

Webshellsは、初期侵入後に攻撃対象ホストにアップロードされ、攻撃グループにリモート管理機能を提供します。

一度アクセスが確立されると、Webshellはネットワーク内の追加ホストへ移動するために利用されます。

### 利用状況

China Chopperは、侵害されたホスト上に仮想的なターミナルコンソールと連動することで、侵害されたWebサーバへリモートへのアクセスとファイル・ディレクトリ管理機能を提供するため、攻撃者に広範囲に利用されています。

China Chopperはたった4KBのファイルサイズであり、簡単に修正可能なペイロードであるため、ネットワーク防御担当者にとって検知と緩和は難しいと言えます。

### 機能
China Chopperは、二つの主要なコンポーネントで構成されています。一つは、China Chopperのクライアントサイドアプリで、攻撃者によって実行されます。もう一つは、China Chopperサーバで、攻撃者のコントロールのもとで、被害者のWebサーバにインストールされます。

Webshellクライアントは、ターミナルコマンドの発行と、被害サーバ上でファイルを管理します。以下は、広く知られているMD5ハッシュ値です(本来、hxxp://www.maicaidao.comで投稿されたものです)。

WebクライアントのMD5ハッシュ値を表１に示します。

__Table 1: China Chopper webshell client MD5ハッシュ値__

| Webshell Client | MD5 Hash |
| :---------------| :------- |
| caidao.exe      | 5001ef50c7e869253a7c152a638eab8a |

Webshellサーバは、テキストとしてアップロードされ、攻撃者によって簡単に変えることができます。この特徴は、攻撃的なアクティビティを特定するための特定のハッシュ値を定義することが非常に難しくなります。2018年の夏に、CVE-2017-3066の脆弱性を持つインターネット公開Webサーバを狙った攻撃グループの存在を観測しました。Webアプリケーション開発プラットフォームであるAdobe ColdFusionに存在していたリモートコード実行の脆弱性に関係した攻撃でした。

China Chopperは、システムが一度侵入し、攻撃グループが被害ホストへリモートアクセスできるようにした後、第二フェーズのペイロードとして作成されています。被害マシンの脆弱性の悪用に成功した後、テキストベースのChina Chopperは被害Webサーバへ配置されます。一度アップロードすると、Webshellサーバは、クライアントアプリを使っていつでも攻撃グループによりアクセス可能となります。一度接続に成功すると、攻撃グループはWebサーバ上のファイルとデータを改竄することを始めます。

China Chopperの機能は、インターネットから対象へファイルのダウンロードを行うファイル操作ツールwgetを利用し、被害サーバとの間でファイルをアップロード・ダウンロードのやり取りを行います。そして、既存のファイルの変更、削除、コピー、ファイル名変更、そしてタイムスタンプの変更をも実現します。

### 検知と防御

Webshellに対する最も強力な防御は、そもそもWebサーバへの侵害を防ぐことです。インターネット公開されているWebサーバ上で動いている全てのソフトウェアは、セキュリティパッチが適用され、最新の状態にあることを確実にしてください。そして、カスタマイズされたアプリケーションについては、一般的なWeb脆弱性が存在しないか監査してください。[^6]

[^6]:https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project

China Chopperの一つの属性として、全てのアクションにおいてHTTPプロトコルのPOSTメソッドが発行されます。もしネットワーク防御担当者が調査すれば、目立つためすぐに検出されるでしょう。

China Chopper webshellサーバが、テキスト形式でアップロードされる一方、クライアントが発行するコマンドは、（簡単にデコードすることが可能ではありますが）、BASE64形式でエンコードされています。

WebサーバによりTLS（Transport Layer Security）へ対応している場合、Webサーバ通信は全て暗号化されることを意味します。そのため、China Chopperのアクティビティをネットワークベースのツールを使って検出することはより困難になります。

China Chopperを検出し緩和する最も効果的な方法は、ホスト自体、特にインターネット公開されているWebサーバを調査することです。Webshellの存在を検索する簡単な方法は、LinuxベースのOS、あるいはWindows OS両方で利用できるコマンドラインツールを利用することです。[^7]

[^7]:https://www.fireeye.com/blog/threat-research/2013/08/breaking-down-the-china-chopper-web-shell-part-ii.html

webshellの存在をより広範囲で検出する場合、ネットワーク防御担当者はWebサーバ上の怪しいプロセス実行（例えば、PHPのプロセス開始）に焦点を当てる方法か、Webサーバからのアウトバウンドへのネットワーク通信で、通常のパターンから外れるものへ焦点を当てる方法が考えられます。典型的には、Webサーバは内部ネットワークに想定可能な通信を行います。こうしたパターンの変化は、webshellの存在を示唆します。ネットワーク防御担当者は、PHPが実行されているディレクトリへの書き込みや既存のファイルの改竄からWebサーバのプロセスを守るため、ットワーク権限を管理する必要があります。

トラフィック分析を通じた監視の情報ソースとして、Webアクセスログを利用することも推奨します。想定外のページへのアクセスやトラフィックパターンの変化は、簡単な指標となるでしょう。

## Credential Stealer: Mimikatz
2007年に開発されたMimikatzは、攻撃者が、攻撃対象としているWindows端末にログインしている他のユーザのクレデンシャル情報（＝認証情報）を収集する際に主に利用されています。LSASS（Local Security Authority Subsystem Service）と呼ばれるWindowsプロセス内のメモリに存在する、クレデンシャル情報にアクセスすることでこれを実現します。

平文もしくはハッシュ値形式であれ、こうしたクレデンシャルはネットワーク上の他のマシンへアクセスする際に再利用されます。

もともとハッキングツールとして開発されたされたわけではないですが、現在ではMimikatzは様々な目的で様々なグループによって利用されています。Mimikatzは世界中の侵害で使われており、各組織がネットワーク防御を世界中で再評価を進めるきっかけになりました。

Mimikatzは、ホストへのアクセスを奪取した後、内部ネットワークを自由に動き回る目的で攻撃グループに使用されます。このツールの使用は、適切なネットワークセキュリティ対策が講じられていない環境を著しく影響を与えます。

### 利用状況

Mimikatzのソースコードは公開されており、誰でも新しいツールの独自バージョンとしてコンパイルすることができ、Mimikatzのカスタムプラグインや追加機能を実装することも可能です。

本研究に携わったサイバー専門機関は、組織的犯罪グループや、国家に支援された攻撃グループ間でMimikatzの使用が広がっていることを確認しています。

攻撃グループがホストのローカル管理者権限を奪取したすると、Mimikatzはハッシュ値と平文形式で、他ユーザのクレデンシャル情報を取得する機能を持っています。これにより、攻撃グループはドメイン内で権限昇格を行うことができ、Post-Exploitationタスクや横断的侵害（Lateral Movement）タスクを実行することができます。

この理由により、Mimikatzは、PowerShell EmpireやMetasploitなど、他のペネトレーションテストツールやや攻撃ツールパックに含まれています。

### 機能

Mimikatzは、メモリから平文とハッシュ値形式でクレデンシャル情報を抜き出すツールとして一番知られています。しかし、そのMimikazが持つ機能は、非常に広範囲にわたります。

Mimikatzは、LMハッシュ（Local Area Network Manager Hash）やNTLMハッシュ（NT LAN Manager Hashes）、証明書、Windows XP（2003）からWindows 8.1（2012r2）の長期鍵（LTK：Long Term Key）などを盗み出すこともできます。さらに、 pass-the-hash攻撃やpass-the-ticket攻撃を実行したり、KeroberosのGolden Ticketを作ることもできます。

Mimikatzの持つ多くの機能は、Powershellなどによりスクリプトで自動化することができ、攻撃グループは素早く侵入し、侵害したネットワーク内を動き回ることを可能にします。さらに、PowerShellスクリプトであり、無償で利用できる「Invoke-Mimikatz」を利用し、メモリ上でMimikatzを実行されれば、Mimikatzの活動を隔離し、特定することは非常に難しくなります。

### 事例

Mimikatzは、複数年にわたり、様々な攻撃グループから様々なインシデントにおいて使われています。2011年には、オランダの認証局であるDigiNotarから、管理者クレデンシャルを盗み出すため、正体不明の攻撃グループにより利用されました。DigiNotarが急速に信頼を失ったことで、侵害後１か月以内にこの企業は破産に追い込まれてしまいました。

より最近では、Mimikatzは他の悪意のあるツールと一緒に使われるケースも確認されています。例えば、何千台ものコンピュータが持つ管理者クレデンシャル情報を盗み出した、2017年のランサムウェア攻撃であるNotPetyaやBadRabbitなどが挙げられます。こうしたクレデンシャルは、横断的侵害（Lateral Movement）を容易にして、ランサムウェアをネットワーク内にバラまき、有効なクレデンシャル情報を持つ多くのシステムのハードドライブを暗号化することを可能にします。

さらに、Microsoft社の研究チームは、複数の有名なテクノロジー企業や金融機関を対象とした、高度なサイバー攻撃におけるMimikatzの利用を特定しています。他のツールと悪用された脆弱性とともに、Mimikatzはシステムハッシュ値を出力し、再利用するために使われています。

### 検知と防御

Microsoft社は、新しいWindowsバージョンごとに改善された防御メカニズムを提供しているため、Windowsをアップデートしていくことにより、Mimikatzツールを使って、攻撃者が奪取できる情報を減らすのに役立ちます。

Mimikatzによるクレデンシャル情報の奪取を防ぐためには、ネットワーク防御担当者はLSASSメモリ内に平文パスワードを保存することを無効化する必要があります。これは、Windows 8.1とWindows Server 2012 R2以降のデフォルト設定ですが、関連するセキュリティパッチがインストールされている古いシステムでも設定方法が公開されています。[^8]

[^8]:https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a

Windows 10とWindows Server 2016では、新しいセキュリティ機能であるCredential Guardnにより保護されています。 Credential Guardは以下の条件を満たすとき、デフォルトで有効になっています。

- ハードウェアがMicrosoft社の「[Windows Hardware Compatibility Program Specifications and Policies](https://docs.microsoft.com/en-us/windows-hardware/design/compatibility/whcp-specifications-policies)」の要件のうち、Windows Server 2016に対する要件、あるいはWindows Server Semi-Annual Branchの要件を満たしている場合
- サーバがドメインコントローラとして機能していない場合

物理的、あるいは仮想的なサーバがMicrosoft社のWindows 10 もしくはWindowsサーバの各リリース時における最低条件を満たしているか、検証する必要があります。

アカウントに対するパスワードの再利用は、特に管理者アカウントの場合、ass-the-hash攻撃をより簡単にします。ネットワーク上の一般権限のアカウントを含め、パスワードの再利用をやめさせるユーザポリシーを組織内で確立する必要があります。Microsoft社が提供する無償ツールLAPS（Local Administrator Password Solution）は、ローカル管理者権限のパスワードの管理を簡単にし、パスワードを手動で設定し保存する必要から解放されます。

ネットワーク管理者は、不自然、あるいは許可されていないアカウント作成、認証（Keroberosチケット悪用を防ぐため）、ネットワーク上の持続メカニズム（Network Persistence）や横断的侵害（Lateral Movement）を監視し、対応する必要があります。Windowsにおいては、Microsoft ATA（Advanced Threat Analytics）やAzure ATP（Advanced Threat Protection）などのツールもこれらの監視の役に立つでしょう。

ネットワーク管理者は、システムがパッチされ、最新の状態にあることを確実にする必要があります。Mimikatzがもつ多くの機能は、最新のシステムバージョンやアップデートにより、緩和されたり、厳しく制限されています。しかし、アップデートは完全な対策にはならず、Mimikatzは継続的に進化を続け、新しいサードパーティ製モジュールもしばしば開発されています。

最も最新化されたウイルス対策ツールは、カスタマイズされていないMimikatzを検知し、隔離できるため、おうしたインスタンスを検知するために使われるべきです。しかし、攻撃グループは時々、メモリ上でMimikatzを実行したり、ツールのオリジナルコードを少しだけ変更することにより、ウイルス対策システムを出し抜こうとします。どこでMimikatzを検知しようと、攻撃グループがネットワーク内に現在もいることを示すほぼ確実な指標であるため、自動的なインシデント対応プロセスではんく、厳格な調査を実行する必要があります。

Mimikatzの機能の一部は、管理者アカウントの悪用に依存します。そのため、管理者アカウントが必要に応じてのみ発行されることを確実にする飛鳥があります。管理者権限によるアクセスが必要な場合、権限アクセス管理システムを申請する必要があります。

Mimikatzは、侵害システムにログインしているユーザのアカウントのみ奪取するため、高権限ユーザ（例えば、ドメイン管理者）は、高い権限を持ったクレデンシャル情報で端末にログインすることは避けるべきでしょう。Active Directoryをセキュアにする詳細な情報はMicrosoft社から提供されています。[^9]

[^9]:https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/best-practices-for-securing-active-directory

ネットワーク防御担当者は、スクリプトの使用、特にPowerShellを監査し、異常を検知するためにログ分析を行ってください。こうした活動は、Mimikatzやpass-the-hash攻撃を特定するのに役立つだけでなく、検知ソフトウェアを回避する試みへの緩和策としても機能します。

## Lateral Movement Framework: PowerShell Empire
PowerShell Empireは、Post-Exploitationもしくは横断的侵害（Lateral Movement）ツールの例として知られています。このツールは、初期アクセスを奪取した後、攻撃者（あるいはペネトレーションテスター）がネットワーク内を動き回れることを支援するために作られました。こうしたツールの別の例として、Cobalt StrikeやMetasploitが挙げられます。PowerShell Empireは、ネットワークにソーシャルエンジニア攻撃をしてアクセスするため、攻撃コードつきのドキュメントファイルや実行ファイルを作成できることでも知られています。

PowerShell Empireフレームワークは、2015年に正当なペネトレーションテストツールとして設計されました。PowerShell Empireは、一度攻撃グループがシステムのアクセスを奪取した後、悪用を続けるためのフレームワークとして機能します。

このツールは、攻撃グループに、権限昇格（escalate privileges）、クレデンシャル情報の収集（harvest credentials）、情報持ち出し（exfiltrate information）、ネットワーク内の横断的侵害（Lateral Movement）の機能を提供します。こうした機能により、こPowerShell Empireは強力な攻撃ツールとなっています。PowerShell Empireは、一般的で正当性のあるアプリ（Powershell）で作成されており、ほとんどメモリ上で動くため、伝統的なウイルス対策ソフトを使ってネットワーク上で検知するのは非常に難しいと考えられます。

### 利用状況

PowerShell Empireは、敵対的な国家攻撃グループや組織的犯罪グループにより非常に有名になっています。最近では、世界中でおこるサイバーインシデントにおいて、業界を問わず広く使われていることが確認されています。

初期侵入の方法は様々ですが、攻撃グループは各シナリオと攻撃対象に合わせて、PowerShell Empireを個別設定します。この事実は、PowerShell Empireのユーザコミュニティないでもスキルと意図のばらつきがあるという事実とも組み合わせると、検知の容易性も様々です。それでもなお、このツールへのより深い理解と意識を高めることは、攻撃グループの利用に対して防御するための一歩です。

### 機能

PowerShell Empireは、攻撃グループが被害者端末上で様々なアクションを実行することを可能にし、PowerShellスクリプトをpowershell.exeなしに実行できる機能を提供します。その通信は暗号化され、アーキテクチャは柔軟性があります。

PowerShell Empireは、より具体的な悪意のアクションを行うためモジュールを利用します。こうしたモジュールは、被害端末上で目的を達成するため、カスタマイズ可能なオプションを攻撃グループに提供します。こうした目的は、権限昇格（escalation of privileges）、クレデンシャル情報の収集（credential harvesting）、ホスト列挙（host enumeration）、キーロガー、ネットワーク内を横断的侵害（lateral movement）する能力などが含まれます。

PowerShell Empireは、簡単に使え、柔軟性のある設定、そして検知を回避する機能を持っているため、異なるスキルを持つ攻撃グループが良く利用します。

### 事例

2018年２月のインシデントにおいて、イギリスのエネルギー企業が未知の攻撃グループにより侵入されました。この侵入は、このツールのデフォルトのプロファイル設定であるPowerShell Empireのビーコンを利用していたことで検知されました。被害組織の管理者アカウントの一つの弱いクレデンシャルが、初期アクセス時に攻撃グループにより利用されたと考えられます。

2018年の前半に、未知の攻撃グループは、複数の韓国組織に標的を定め標的型フィッシングキャンペーンを行うため、冬季オリンピックを装ったソーシャルエンジニアリングメールと悪性の添付ファイルを利用しました。この攻撃は、さらに洗練されたポイントがあり、Invoke-PSImageというイメージ上にPowerShellスクリプトを隠すステガノグラフィツールを利用しました。

2017年12月には、APT19がフィッシングキャンペーンを行う国際的な法律事務所を攻撃しました。APT19は、PowerShell Empireによって生成されたMicrosoft Wordドキュメントに追加された難読化されたPowerShellマクロを利用していました。

本研究に携わったサイバー専門機関は、PowerShell Empireは学術界を狙った攻撃にも利用されていることを確認しています。ある報告された事例では、攻撃グループがPowerShell Empireを使い、WMI（Windows Management Instrumentation）イベントを利用しているユーザを活用して持続性を確保しようと試みたこともあります。しかし、この事例では、ローカルのセキュリティ機器によりHTTPコネクションがブロックされていたため、PowerShell Empireのエージェントがネットワーク接続の確率に失敗していました。

### 検知と防御

悪意のあるPowerShellの活動を特定することは非常に難しいと言えます。なぜなら、ホストにおける正当なPowerShellの利用が流行しており、企業環境のメンテナンスにPowerShellの利用は増加しているためです。

潜在的に悪性なスクリプトを特定するため、PowerShell活動は網羅的にロギングされるべきです。このログには、スクリプトをブロックしたログやPowerShell transcriptsの結果も含まれます。

古いバージョンのPowerShellは、環境から削除され、追加のロギング機能を出し抜いて使われないこと、およびより最近のバージョンのPowerShellが持つ追加のコントロールを有効にすることを確実に実装する必要があります。以下のページは、PowerShellセキュリティでやるべき良いサマリを提供してくれます。[^10]

[^10]:https://www.digitalshadows.com/blog-and-research/powershell-security-best-practices/

最近のバージョンのWindowsで実装されているコードの整合性機能は、侵入に成功した際に、悪意のあるPowerShellの実行を防いだり妨害することで、PowerShellの機能を制限することが出来ます。

スクリプトコード署名、アプリケーション・ホワイトリスト制御、Constrained Language Modeの併用は、侵入成功時において、悪意のあるPowerShellの有効性を妨げ、限定することが出来ます。こうしたコントロールは、正当性のあるPowerShellスクリプトにも影響を与えるため、展開前にテストすることを強く推奨します。

組織がPowerShellを使っている場合、技術スタッフのごく一部のみが正当な目的で利用していることを確認できる場合もあるでしょう。正当な活動の範囲を特定することは、ネットワーク上のあらゆる場所で実行される疑わしい、あるいは想定されていないPowerShellの存在を監視し、調査することを容易にしてくれます。

## C2 Obfuscation and Exfiltration: HUC Packet Transmitter 
攻撃者は対象に侵入したとき、接続元を偽装したいと考えます。それを実行するために、汎用的なプライバシーツール（例：Tor）を使ったり、接続元をたどりづらくする専用のツールを使うこともあります。

HUC Packet Transmitter (HTran)は、ローカルホストからリモートホストへのTCP接続に介入し、リダイレクトするプロキシツールです。これは、被害者のネットワークにおける香華k氏やの通信を難読かすることを実現します。このツールは、少なくても2009年からインターネット上で無償で公開されていました。

HTranは、被害者と攻撃グループにより管理されているホップポイントのTCP接続を容易にします。悪意のある攻撃グループは、HTranを動かした複数の侵害したホストを経由してパケットをリダイレクトされるテクニックを使い、被害者ネットワークへのアクセスを確保することができます。

### 利用状況

HTranの利用は、政府機関や民間セクタでの侵害でも定期的に確認されています。

攻撃グループの多くがHTranやその他のプロキシ接続ツールを以下の目的で利用しています。

- ネットワークの侵入検知システムを回避するため
- セキュリティコントロールを回避するため、汎用的なトラフィックに通信を紛れ込ませたり、ドメインの信頼性をレバレッジとして活用するため
- C2インフラストラクチャと通信を偽装したり隠したりするため
- 検知を回避し、回復力のある接続を得る目的でピア・ツー・ピア、もしくはメッシュ方のC2インフラストラクチャを構築するため

### 機能

HTranは複数のモードで機能します。各モードは、二つのTCPソケットをブリッジすることにより、ネットワークを介してトラフィックを転送します。TCPソケットがどこから開始されているか（ローカル or リモート）によってモードが異なります。３種類のモードは、以下の通りです。

- Server（Listen）：TCPソケット両方がリモートで開始される
- Client（Slave）：TCPソケット両方がローカルで開始される
- Proxy（Tran）：最初の接続からのトラフィックを受診すると、一方のTCPソケットがリモートで開始され、一方がローカルで開始される。

HTranは、稼動しているプロセスに自分をインジェクトして、ホストOSからネットワーク接続を隠すルートキットをインストールします。この仕組みを使って、HTransが被害者ネットワークへのアクセスを持続・維持できることを確実にするため、Windowsレジストリに書き込みを行います。

### 事例

本研究に携わったサイバー専門機関が行った最近の調査では、攻撃対象環境へのリモートアクセスを維持し、偽装するためにHTranの利用を確認しました。あるインシデントでは、攻撃グループは、サポートが切れ、脆弱なWebアプリケーションを動かしていたインターネット公開のWebサーバへ侵入しました。この侵入により、WebShellをアップロードすることが可能になり、それを使いHTranを含む他のツールを配置しました。

HTranは、ProgramDataディレクトリインストールされ、サーバがRDP接続を許可する再設定を行う目的で、他のツールも配置されました。

攻撃グループは、HTransをクライアントとして実行するコマンドを発行し、ローカルのインターフェイスからRDPトラフィックを転送し、ポート80経由でインターネットに配置されていたサーバへの接続を開始しました。

この例の場合、Webサーバからインターネットへの接続に見える他のトラフィックの中に攻撃通信を混ぜこむ目的でHTTPが選択されました。他によく使われる既知のポートとして、以下があげられます。

- ポート53：DNS（Domain Name System）
- ポート443：HTTPS（HTTP over TLS/Secure Sockets Layer）
- ポート3306：MySQL

HTranをこのように利用すると、攻撃グループはRDPを数ヶ月間、検知されることなく使い続けることが出来ます。

### 検知と防御

攻撃グループは、HTranをインストールし実行するために、端末にアクセスする必要があります。そのため、ネットワーク防御担当者は、セキュリティパッチを適用し、悪意のあるアプリケーションのインストールを防ぐため、適切なアクセスコントロールを設定する必要があります。

ネットワーク監視とファイアウォールは、HTranのようなツールによる不正通信を防御、検知することを助けてくれるでしょう。

分析されたいくつかのサンプルによれば、HTranのルートキットは、Proxyモードを使ったときのみ、接続の詳細を隠すようです。もしClientモードが使われていれば、TCP接続の詳細を見ることが出来るかも知れません。

HTranは、デバッグ条件を含んでおり、これはネットワーク防御には有益です。接続先が利用できないという状況が発生した場合、HTranは以下のフォーマットのエラーメッセージを出力します。
```
sprint(buffer, “[SERVER]connection to %s:%d error\r\n”, host, port2);
```

このエラーメッセージは、接続クライアントに平文で中継されます。ネットワーク防御担当者はこのエラーメッセージを監視することで、環境内で活動しているHTranを検知することが出来ます。

# 緩和策
あなたの組織の全体的なサイバーセキュリティを向上させるいくつかの対策があり、このレポートで取り上げた類のツールに対する防御を助けてくれるでしょう。ネットワーク防御担当者は、以下のリンクからより詳細な情報を入手して参考にしてください。

**<font color="DeepPink">（以下リンクは翻訳省略）</font>**

- Protect your organization from malware.
-- See NCCIC Guidance: https://www.us-cert.gov/ncas/tips/ST13-003.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/protecting-your-organisation-malware.

- Board toolkit: five question for your board’s agenda.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/board-toolkit-five-questions-your-boards-agenda.

- Use a strong password policy and multifactor authentication (also known as two-factor authentication or two-step authentication) to reduce the impact of password compromises.
-- See NCCIC Guidance: https://www.us-cert.gov/ncas/tips/ST05-012.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/multi-factor-authentication-online-services and https://www.ncsc.gov.uk/guidance/setting-two-factor-authentication-2fa.

- Protect your devices and networks by keeping them up to date. Use the latest supported versions, apply security patches promptly, use antivirus and scan regularly to guard against known malware threats.
-- See NCCIC Guidance: https://www.us-cert.gov/ncas/tips/ST04-006.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/mitigating-malware.

- Prevent and detect lateral movement in your organization’s networks.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/preventing-lateral-movement.

- Implement architectural controls for network segregation.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/10-steps-network-security.

- Protect the management interfaces of your critical operational systems. In particular, use browse-down architecture to prevent attackers easily gaining privileged access to your most vital assets.
-- See UK NCSC blog post: https://www.ncsc.gov.uk/blog-post/protect-your-management-interfaces.

- Set up a security monitoring capability so you are collecting the data that will be needed to analyze network intrusions.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/introduction-logging-security-purposes.

- Review and refresh your incident management processes.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/10-steps-incident-management. 

- Update your systems and software. Ensure your operating system and productivity applications are up to date. Users with Microsoft Office 365 licensing can use “click to run” to keep their office applications seamlessly updated.

- Use modern systems and software. These have better security built-in. If you cannot move off out-of-date platforms and applications straight away, there are short-term steps you can take to improve your position.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/obsolete-platforms-security-guidance. 

- Manage bulk personal datasets properly.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/protecting-bulk-personal-data-introduction. 

- Restrict intruders' ability to move freely around your systems and networks. Pay particular attention to potentially vulnerable entry points (e.g., third-party systems with onward access to your core network). During an incident, disable remote access from third-party systems until you are sure they are clean.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/preventing-lateral-movement and https://www.ncsc.gov.uk/guidance/assessing-supply-chain-security.

- Whitelist applications. If supported by your operating environment, consider whitelisting of permitted applications. This will help prevent malicious applications from running.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/eud-security-guidance-windows-10-1709#applicationwhitelistingsection. 

- Manage macros carefully. Disable Microsoft Office macros, except in the specific applications where they are required.

- Only enable macros for users that need them day-to-day and use a recent and fully patched version of Office and the underlying platform, ideally configured in line with the UK NCSC’s End User Device Security Collection Guidance and UK NCSC’s Macro Security for Microsoft Office Guidance: https://www.ncsc.gov.uk/guidance/end-user-device-security and https://www.ncsc.gov.uk/guidance/macro-security-microsoft-office. 

- Use antivirus. Keep any antivirus software up to date, and consider use of a cloud-backed antivirus product that can benefit from the economies of scale this brings. Ensure that antivirus programs are also capable of scanning Microsoft Office macros.
-- See NCCIC Guidance: https://www.us-cert.gov/ncas/tips/ST04-005.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/macro-security-microsoft-office.

- Layer organization-wide phishing defenses. Detect and quarantine as many malicious email attachments and spam as possible, before they reach your end users. Multiple layers of defense will greatly cut the chances of a compromise.

- Treat people as your first line of defense. Tell personnel how to report suspected phishing emails, and ensure they feel confident to do so. Investigate their reports promptly and thoroughly. Never punish users for clicking phishing links or opening attachments.
-- NCCIC encourages users and administrators to report phishing to phishing-report@us-cert.gov.
-- See NCCIC Guidance: https://www.us-cert.gov/ncas/tips/ST04-014.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/phishing. 

- Deploy a host-based intrusion detection system. A variety of products are available, free and paid-for, to suit different needs and budgets.

- Defend your systems and networks against denial-of-service attacks.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/denial-service-dos-guidance-collection. 

- Defend your organization from ransomware. Keep safe backups of important files, protect from malware, and do not pay the ransom– it may not get your data back.
-- See NCCIC Guidance: https://www.us-cert.gov/Ransomware.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/mitigating-malware and https://www.ncsc.gov.uk/guidance/backing-your-data.

- Make sure you are handling personal data appropriately and securely.
-- See NCCIC Guidance: https://www.us-cert.gov/ncas/tips/ST04-013.
-- See UK NCSC Guidance: https://www.ncsc.gov.uk/guidance/gdpr-security-outcomes.  

追加情報：様々なシナリオで、マルウェアを起点とした攻撃を防ぐために投資しましょう。UK NCSC Guidanceが参考になります。https://www.ncsc.gov.uk/guidance/mitigating-malware

# 国際的パートナーからの追加リソース
- Cyber Security Centre (ACSC) Strategies - https://acsc.gov.au/infosec/mitigationstrategies.htm
- ACSC Essential Eight -
https://acsc.gov.au/publications/protect/essential-eight-explained.htm
- Canadian Centre for Cyber Security (CCCS) Top 10 Security Actions -
https://cyber.gc.ca/en/top-10-it-security-actions
- CCCS Cyber Hygiene - 
https://www.cse-cst.gc.ca/en/cyberhygiene-pratiques-cybersecurite
- CERT New Zealand's Critical Controls 2018 - 
https://www.cert.govt.nz/it-specialists/critical-controls/
- CERT New Zealand’s Top 11 Cyber Security Tips for Your Business - 
https://www.cert.govt.nz/businesses-and-individuals/guides/cyber-security-your-business/top-11-cyber-security-tips-for-your-business/
- New Zealand National Cyber Security Centre (NZ NCSC) Resources -
https://www.ncsc.govt.nz/resources/
- New Zealand Information Security Manual - 
https://www.gcsb.govt.nz/publications/the-nz-information-security-manual/
- UK NCSC 10 Steps to Cyber Security - 
https://www.ncsc.gov.uk/guidance/10-steps-cyber-security
- UK NCSC Board Toolkit: five questions for your board's agenda - 
https://www.ncsc.gov.uk/guidance/board-toolkit-five-questions-your-boards-agenda
- UK NCSC Cyber Security: Small Business Guide - 
https://www.ncsc.gov.uk/smallbusiness

# 連絡先
**<font color="DeepPink">（翻訳省略）</font>**

NCCIC encourages recipients of this report to contribute any additional information that they may have related to this threat. For any questions related to this report, please contact NCCIC at

- 1-888-282-0870 (From outside the United States: +1-703-235-8832)
- NCCICCustomerService@us-cert.gov (UNCLASS)
- us-cert@dhs.sgov.gov (SIPRNET)
- us-cert@dhs.ic.gov (JWICS)

NCCIC encourages you to report any suspicious activity, including cybersecurity incidents, possible malicious code, software vulnerabilities, and phishing-related scams. Reporting forms can be found on the NCCIC/US-CERT homepage at http://www.us-cert.gov/.

# フィードバック
**<font color="DeepPink">（翻訳省略）</font>**

NCCIC strives to make this report a valuable tool for our partners and welcomes feedback on how this publication could be improved. You can help by answering a few short questions about this report at the following URL: https://www.us-cert.gov/forms/feedback.

# 参考文献
**<font color="DeepPink">（脚注という形で記載）</font>**

# リビジョン
初期バージョン 2018.10.11
