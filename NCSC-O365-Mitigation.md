アドバイザリ：O365への侵入増加と防御方法について
====

# 注意事項
- 本資料は、英国サイバーセキュリティセンター(UK National Cyber Security Centre)の資料"[Advisory: The rise of Microsoft Office 365 compromise](https://www.ncsc.gov.uk/alerts/rise-microsoft-office-365-compromise)"を翻訳した資料です。
- 内容については、最大限の努力を持って正確に期していますが、本書の内容に基づく運用結果については責任を負いかねますので、ご了承ください。
- 他の翻訳は、『[Scientia Securtity on GitHub](https://scientia-security.github.io/)』を参照してください。
- ブログは『[セキュリティコンサルタントの日誌から](https://www.scientia-security.org/)』を参照してください。

# 本ドキュメントについて
NCSCアドバイザリは、Microsoft Office 365の侵入と、あらゆる規模の組織が考慮すべき防御方法の詳細を提供します。このアドバイザリで紹介された防御方法を取る際には、リスクベースアプローチに基づき実行する必要があります。

# レポートの取り扱いについて
このレポートの取り扱いには、TLP White（Traffic Light Protocol）が割り当てられており、取り扱いの制限なくサイバーセキュリティ情報共有パートナーシップのコミュニティ（CiSP：Cyber Security Information Sharing Partnership）内で共有できることを意味します。TLPに従い、レポートを保存・取り扱い・やり取りする必要があります。

# ディスクレーマー
このレポートは、NCSC（UK National Cyber Security Centre）と業界内の情報源から入手した情報から記述されています。NCSCの調査結果・推奨事項は、すべてのリスクを回避することを目的としたものではなく、推奨事項に従うことによってそのようなリスクがすべて除去されることを保証するものではありません。情報リスク管理は、常に関連するシステムオーナーの責任となります。

# はじめに
Microsoft Office 365（O365）は、Microsoft Officeアプリケーションのオンラインバージョンです。このサービスは、毎月支払われるサブスクリプションに基づきクラウドを経由してユーザに提供され、最近利用者が増加しています。O365はいろんな規模の組織・業界でますます受け入れられているため、金銭的利益を探し求める攻撃グループの主要な攻撃対象となりつつあります。FBIによれば、2013年から2016年においてビジネスメールへの攻撃は5.3億ドル以上の損失を生んでいます。[^1]

NCSCは、標的型のサプライチェーン攻撃など複数の攻撃手口を含め、英国企業のO365アカウントに対して侵入が行われ複数のインシデントが発生していることを確認しています。こうした攻撃の最終的な目標は明らかになっておらず、攻撃対象は特定の業界へ限定されたり、特定の攻撃グループにのみ見られる傾向でもありません。

NCSCアドバイザリは、攻撃グループによるO365への侵入手法を紹介し、情報漏洩が発生するリスクを低減するために組織が取り得る施策を詳細なガイドラインとして提供します。

# 詳細
## O365侵入の目的
O365アカウント侵入への影響は、攻撃グループの目的により深刻度が変化します。攻撃グループがO365アカウントの認証情報を奪取できれば、O365のインターフェース（SharePoint・OneNoteなど）に存在する文書へアクセスできるだけでなく、組織内に更なる侵入を実行するための起点として利用できるでしょう。攻撃グループが窃取したO365アカウントを悪用する様々な方法は以下の通りです。

* 金銭情報を操作・移動したり、組織内の情報を得るため、奪取したO365アカウント所有者に成りすますこと。
* 企業にレピュテーション被害をもたらすため、情報を公開・販売したりすること。また、奪取したアカウントを通じてセンシティブ情報を盗み出すこと。
* 奪取したO365アカウントを利用し、スピアフィッシングメールを配布し、メール受信者から更なる認証情報を奪い取ること。また、攻撃グループが組織・サプライチェーン内における別のO365認証情報へアクセスをできるようにすること。
* 奪取したO365の認証情報を利用し、ソーシャルメディアの個人情報や、攻撃グループが利用・販売できるセンシティブ情報へアクセスすること。
* 奪取したO365アカウントにおいて、転送ルールを設定し、受信したメールをこっそり攻撃者のメールアカウントへ転送すること。

## O365アカウントに対する攻撃
O365へ不正アクセスするため、攻撃グループは二つの攻撃テクニックを使います。総当たり攻撃とスピアフィッシング攻撃です。

### 総当たり攻撃
総当たり攻撃（Brute Force Attacks）は、パスワード推測を含まれ、しばしば自動ツールを活用します。O365アカウントに対する攻撃を行う際には、クラウドサービス業者によって攻撃を検知される可能性を減らすため、複数の社員を狙うよりも、組織内にある特定の個人に対して攻撃を行うことがより一般的です。[^2]

### スピアフィッシング攻撃
攻撃グループは、スピアフィッシング攻撃のメールを送信し、偽ログイン画面へユーザを誘導するため被害者にリンクをクリックさせます。この偽ログイン画面を通じて、攻撃グループは、被害者のO365認証情報を収集することができます。[^3]

# 多要素認証が重要である理由
総当たり攻撃やスピアフィッシング攻撃から、O365への不正アクセスリスクを低減するために重要な施策の一つとして、O365プラットフォームにおける多要素認証（MFA：Multi-Factor Authentication）の実装が挙げられます。ユーザはオンラインサービス・エンタープライズサービスで、パスワードを再利用する傾向にあるため[^4]、多要素認証を追加することではパスワードを奪取される潜在的可能性を低減してくれます。

多要素認証は、以下の認証方法のうち二つ以上を必要とします。
* Something You Know （知識認証）：よくある例としてはパスワード
* Something You Have（所有物認証）：モバイル電話など信頼されたデバイス
* Something You Are（生体認証）

O365プラットフォームは、サブスクリプションにより様々な多要素認証メカニズムがサポートされており、組織は様々な方法を組み合わせて使うことができます。

O365プラットフォーム全体に多要素認証を効果的に実装するためには、IT組織が対象となるユーザグループについて深く理解している必要があります。特に、組織が多様性に富む従業員を抱えている場合、特に重要です。例えば、モバイル電話があまり普及していない地域に従業員がいる場合、SMSトークンを受け取る多要素認証方式は問題が起こり、O365プラットフォームへアクセスすることを困難にしてしまう可能性が考えられます。このシナリオの場合、さらなる配備に抵抗されないため、利用可能な別の多要素認証メカニズムを検討する必要があります。

NCSCは、『Multi-factor authentication for online services』（未翻訳）という資料を最近公開しています。
https://www.ncsc.gov.uk/guidance/multi-factor-authentication-online-services

組織はこのガイダンスに加え、O365サービスに特化された以下の防御方法について検討することを強くお勧めします。

# 防御方法
このセクションは、全ての組織が実装すべき、最低限必要なセキュリティ施策を記載しています。そして、考慮すべき追加ポイントについても指摘しています。

またNCSCは、定期的に[Office 365 Secure Score](https://securescore.office.com)を確認することを推奨します。これは、組織の設定を分析し、時系列でその状況変化を追跡し、セキュリティに影響する更なる推奨策を提言してくれます。定量的なスコアは一部使いづらいかもしれません。しかし、分析結果と推奨案は、組織が実装しているセキュリティコントロールを監査し、新しく登場したセキュリティ技術を知る上で役立つでしょう。Office 365 Secure Scoreに関する更なる情報、および活用方法については、[Microsoft社のサポートページ](https://docs.microsoft.com/ja-jp/office365/securitycompliance/office-365-secure-score?redirectSourcePath=%252fen-gb%252farticle%252fintroducing-the-office-365-secure-score-c9e7160f-2c34-4bd0-a548-5ddcc862eaef)を参照してください。

また、全ての組織はMicrosoft社が提示している[Security Best Practices for Office 365](https://docs.microsoft.com/en-gb/office365/securitycompliance/security-best-practices)を実装することを推奨しています。その中には以下が含まれています。

## 認証（Authentication）
各組織は、全てのアカウントに対して多要素認証（詳細は、[NCSC's MFA Guidance](https://www.ncsc.gov.uk/guidance/multi-factor-authentication-online-services)と制限つきアクセス(https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/)を有効にする必要があります。追加認証の種類は、現在のユーザがどのようにサービスへアクセスしているかに依存します。しかし、通常は以下の一つを利用します。
* 認証アプリ - 認証を入力するか、「承認」・「拒否」のプロンプトをクリックする方式
* Azure ADに登録・準拠した[既知の業務用デバイス](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/require-managed-devices#managed-devices)からサービスへのアクセスする方式
* VPN経由でネットワークにリモートアクセスし、[信頼されたネットワーク](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/best-practices#how-are-assignments-evaluated)からサービスへのアクセスする方式
* SMSや事前に登録した電話コールを使う方式。SMSは最もセキュアな種類の二要素認証ではありませんが、何もないよりもどんな二要素認証でも実装すべきだと言えます。

NCSCは、特権ユーザや管理者権限でアクセスする際には、認証アプリに加え、既知で信頼されたデバイスかネットワークからアクセスする両方の認証を実装することを推奨しています。

昔からある認証プロトコルの一部は、最近の認証方式を完全にサポートしているわけではありません。場合によっては、多要素認証などのセキュリティコントロールを無効化・回避を求める場合があります。古いオフィスクライアント（例：Office 2010）や、IMAP・SMTP・POPなどのメールプロトコルを使うメールクライアントなどではそうした古い認証プロトコルが使われています。そのため、NCSCは古い認証プロトコルを組織の条件付アクセスポリシーの一部として無効化することを強く推奨しています。

## 監査とモニタリング（Auditing and Monitoring）
組織は、情報漏洩の成功や試みに気付くために十分な監査データを集める必要があります。Office 365でも一部の監査データはデフォルトで有効になっていますが、NCSCは以下の機能を有効にすることを推奨しています。

* [監査ログデータの記録](https://social.technet.microsoft.com/wiki/contents/articles/34748.office-365-security-and-compliance-center-how-to-enable-audit-logs.aspx)。どんなユーザがサービスとやり取りしているか記録してくれます。このアクションは、全てのサービスに反映するために最大２４時間かかります。
* [メールボックスの監査](https://docs.microsoft.com/en-gb/office365/securitycompliance/enable-mailbox-auditing)。様々な侵入に対する時系列の可視化を提供してくれます。デフォルトでは、所有者ではないユーザのメールボックスへのアクセスのみが監査されています。

[Office 365 Cloud App Security](https://docs.microsoft.com/en-us/cloud-app-security/editions-cloud-app-security-o365)などのオプショナルサービスを利用することで、レポート作成したり、一般ユーザ・特権ユーザに対する不審なアクセスや異常な認証イベント・アクセスイベントに対してアラートを挙げてくれるでしょう。

## サービスのハードニング（Service Hardening）
組織は、不正にアクセスする試みを遮断し、攻撃の試行に対する影響を下げるため、[Office 365サービスとデバイスの設定](https://docs.microsoft.com/en-gb/office365/securitycompliance/tenant-wide-setup-for-increased-security)を適切に行う必要があります。最低限組織が検討すべき内容は以下の通りです。

* [Microsoft社](https://blogs.technet.microsoft.com/exovoice/2017/12/07/disable-automatic-forwarding-in-office-365-and-exchange-server-to-prevent-information-leakage/)が提示している通り、Exchange Onlineを適切に設定することで、組織外へメールを自動転送することを防ぐこと。
* Officeのマルウェア対策メカニズムを設定すること。[Exchange Online保護とスパムフィルタリング](https://docs.microsoft.com/en-us/office365/securitycompliance/anti-spam-and-anti-malware-protection)を有効化すること。Office 365 ATPなどのオプションがライセンスに含まれていれば、有効にすべきでしょう。この機能は、添付メール、SharePoint・OneDriveなどに格納されたファイル、メールやドキュメントのリンクに対する高度なセキュリティ分析を実行してくれます。サードパーティのメールフィルタリング機能もメールフローの一部として設定することも有効でしょう。
* [NCSC's e-mail security and anti-spoofing guidance](https://www.ncsc.gov.uk/guidance/email-security-and-anti-spoofing)で記述されている通り、SPF・DKIM・DMARCをExchange Onlineで設定すること。これにより、組織ドメインからのなりすましメールを防ぐことができます。Office 365は、デフォルトで受信メール（Inbound Email）にDMARCが適用されるようになっています。Microsoft社は送信メール（Outbound Email）[SPF](https://docs.microsoft.com/ja-jp/office365/SecurityCompliance/set-up-spf-in-office-365-to-help-prevent-spoofing)・[DKIM](https://docs.microsoft.com/ja-jp/office365/SecurityCompliance/use-dkim-to-validate-outbound-email)・[DMARC](https://docs.microsoft.com/ja-jp/office365/securitycompliance/use-dmarc-to-validate-email)を設定するドキュメントを提供しています。ATP Anti-Phishing機能やサードパーティサービスなどのオプションなど、組織のユーザに届く悪意のあるメール数を減らすことができます。また、[NCSC's Phishing Guidance](https://www.ncsc.gov.uk/phishing)を参照されることを推奨します。
* 組織は、ユーザと他のユーザに付与されている権限をレビューする必要があります。[統合されたアプリ](https://docs.microsoft.com/en-us/office365/admin/misc/integrated-apps?view=o365-worldwide)を通じて、サードパーティサービスにどんな権限が付与されたか理解する必要があります。

## デバイスハードニング
Office 365に対する多くの緩和策はサービス自体に適用される一方、サービスにアクセスするデバイスのセキュリティについて検討することも重要です。最低限として、デバイスにパッチがきちんと適用されていること、管理者権限を使わないこと、マルウェアへの防御メカニズムを実装していること、セキュリティログを収集することなどが挙げられます。NCSCは、関連するガイドラインを参照することを推奨します。
* [Mitigating malware](https://www.ncsc.gov.uk/guidance/mitigating-malware)
* [End user Devices Security Guidance](https://www.ncsc.gov.uk/guidance/end-user-device-security)
* [Macro Security for Microsoft Office](https://www.ncsc.gov.uk/guidance/macro-security-microsoft-office)
* [Preventing Lateral Movement](https://www.ncsc.gov.uk/guidance/preventing-lateral-movement)
* [Introduction to logging for security purposes](https://www.ncsc.gov.uk/guidance/introduction-logging-security-purposes)
* [Phishing attacks: defending your organisation](https://www.ncsc.gov.uk/phishing)

[^1]: https://www.itproportal.com/features/office-365-and-linkedin-integration-a-goldmine-for-fraudsters/
[^2]: https://www.tripwire.com/state-of-security/featured/new-type-brute-force-attack-office-365-accounts/
[^3]: https://threatpost.com/office-365-phishing-campaign-hides-malicious-urls-in-sharepoint-files/136525/
[^4]: https://www.ncsc.gov.uk/guidance/password-collection
