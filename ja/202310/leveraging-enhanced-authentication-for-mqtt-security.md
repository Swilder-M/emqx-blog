[前記事](https://www.emqx.com/ja/blog/securing-mqtt-with-username-and-password-authentication)では、MQTT CONNECT パケットの Username フィールドと Password フィールドを使用して、パスワード認証やトークン認証などの単純な認証を実装できることを紹介しました。この記事では、強化認証として知られる、より高度な認証アプローチについて詳しく説明します。

## 強化認証とは何ですか?

強化認証は、MQTT 5.0 で導入された新しい認証フレームワークです。従来のパスワード認証より安全なさまざまな代替方法を提供します。

ただし、セキュリティを強化すると、複雑さが増します。SCRAM などの特定の認証方法では、認証データを複数回交換する必要があります。これにより、CONNECT パケットと CONNACK パケットの単一交換認証フレームワークが時代遅れになります。この制限に対処するために、MQTT 5.0 では、認証データの複数の交換をサポートする AUTH パケットが導入されています。[これにより、 MQTT](https://www.emqx.com/ja/blog/the-easiest-guide-to-getting-started-with-mqtt)でのチャレンジ/レスポンス形式の[SASL](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) (Simple Authentication and Security Layer) メカニズムの使用が可能になります。

## 強化認証はどのような問題を解決しますか?

強化された認証について詳しく説明する前に、セキュリティの観点からパスワード認証の欠点を理解することが重要です。

実際、パスワードを安全に保存するためにソルトやハッシュなどの技術を採用しているにもかかわらず、クライアントはネットワーク上でパスワードを平文で送信する必要があるため、盗難に対して脆弱になります。通信に TLS 暗号化を採用している場合でも、古い SSL バージョン、弱い暗号スイート、または偽の CA 証明書の存在により、攻撃者がパスワードなどの機密データを取得するリスクが残ります。

さらに、単純なパスワード認証では、サーバーはクライアントの ID を確認するだけで、その逆はできないため、攻撃者がサーバーになりすましてクライアントから機密データを取得することができます。これは、私たちがよく中間者攻撃と呼ぶものです。

強化された認証により、ユーザーは SASL フレームワーク内で安全性の高い認証方法を使用できるようになります。これらの方法には、ネットワーク上でのパスワードの送信が不要になる、クライアントとサーバー間の相互 ID 検証が容易になるなど、いくつかの利点があります。これらのオプションを提示することで、ユーザーは自分の特定のニーズやセキュリティ設定に合わせた認証方法を選択できます。

## 強化認証に使用される一般的な SASL メカニズム

### ダイジェスト-MD5

DIGEST-MD5 は、SASL フレームワーク内の認証方法です。メッセージ ダイジェスト 5 (MD5) ハッシュ アルゴリズムとチャレンジ/レスポンス メカニズムを利用して、クライアントとサーバー間の ID を検証します。注目すべき利点の 1 つは、クライアントがネットワーク上でパスワードを平文で送信する必要がないことです。

簡単に言えば、クライアントが保護されたリソースにアクセスしたい場合、サーバーは 1 回限りの乱数といくつかの必要なパラメーターを含むチャレンジを送信します。クライアントは、ユーザー名とパスワードとともにこれらのパラメーターを使用して応答を生成し、それがサーバーに送信されます。サーバーは、同じメソッドを使用して期待される応答を独自に作成し、それを受信した応答と比較します。一致すれば認証成功です。このアプローチにより、ネットワーク スヌーピングによるパスワード漏洩のリスクが効果的に軽減されます。さらに、接続ごとに 1 回限りの乱数を利用することで、リプレイ攻撃に対する保護が強化されます。

ただし、DIGEST-MD5 では、サーバー側でクライアントの ID を検証できますが、クライアントがサーバーの ID を検証する機能がないことに注意することが重要です。この制限により、中間者攻撃の可能性が残されます。さらに、MD5 は安全ではなくなったため、SHA-256 など、衝突に対するより強力な耐性を備えたハッシュ関数に置き換えることを強くお勧めします。

### スクラム

SCRAM (Salted Challenge Response Authentication Mechanism) は、SASL フレームワーク内の別の認証方法です。アプローチの点では DIGEST-MD5 と類似点があります。SCRAM は、クライアントに 1 回限りの乱数を使用して応答を生成するように促し、これにより、ネットワーク上でパスワードが平文で送信されるのを回避します。ただし、SCRAM は、Salt、Iterations、および SHA-256 や SHA-512 などのより堅牢なハッシュ アルゴリズムを組み込むことで、セキュリティをさらに強化します。これらの追加により、パスワード ストレージのセキュリティが大幅に強化され、オフライン攻撃、リプレイ攻撃、その他の潜在的な脆弱性に関連するリスクが効果的に軽減されます。

さらに、SCRAM には、クライアントに送信されるサーバー側の証明を含む、より複雑なチャレンジ/レスポンス プロセスが組み込まれています。クライアントはこの証明を利用して、サーバーが正しいパスワードを所有していることを確認し、相互認証を可能にします。この追加手順により、中間者攻撃に対する脆弱性が軽減されます。

ただし、SCRAM で SHA256 などのハッシュ アルゴリズムを使用すると、追加の計算オーバーヘッドが発生し、リソースが限られているデバイスのパフォーマンスに影響を与える可能性があります。

### ケルベロス

Kerberos は、信頼できるサードパーティの Kerberos サーバーを利用して、認証サービスを容易にします。サーバーは検証済みのユーザーにトークンを発行し、ユーザーがリソース サーバーにアクセスできるようにします。注目すべき利点は、ユーザーが 1 回の認証で複数のシステムやサービスにアクセスできるため、シングル サインオン (SSO) の利便性が実現できることです。

Kerberos サーバーによって発行されるトークンには有効期限があり、クライアントはこのトークンを使用してサービスにアクセスできるのは一定期間だけであるため、トークン漏洩によるセキュリティ問題を防ぐことができます。もちろん、寿命を短くするとセキュリティは強化されますが、利便性がある程度犠牲になります。ユーザーは独自のトレードオフを行う必要があります。

Kerberos の中核には、対称暗号化アルゴリズムの利用があります。サーバーはローカルに保存されたパスワード ハッシュを使用して認証データを暗号化し、その後クライアントに送信します。次に、クライアントは自身のパスワードをハッシュし、それを使用して受信した認証データを復号します。このプロセスには、ネットワーク上でパスワードを平文で送信する必要がなくなること、サーバーとクライアント間で正しいパスワードを相互検証できることなど、いくつかの利点があります。さらに、対称暗号化により、サーバーとクライアントはセッション キーを安全に共有でき、その後の暗号化通信に利用できます。したがって、Kerberos は、認証を超えた後続の通信を保護するためのセキュリティ対策も提供します。

Kerberos は強力なセキュリティを提供する一方で、かなりの複雑性ももたらします。Kerberos の実装と構成には独自の課題が伴い、最大 6 回のハンドシェイクに依存するため、高いネットワーク遅延と信頼性の要件が発生する可能性があります。その結果、Kerberos は通常、企業の内部ネットワーク環境内で使用されます。

## 強化認証は MQTT でどのように機能しますか?

SCRAM を例として使用して、MQTT で強化認証がどのように機能するかを調べてみましょう。この記事では SCRAM の特定の原則については詳しく説明しませんが、SCRAM では認証を完了するために次の 4 つのメッセージが必要であることに注意することが重要です。

- クライアントファーストメッセージ
- サーバーファーストメッセージ
- クライアント最終メッセージ
- サーバー最終メッセージ

![How Does Enhanced Authentication Work in MQTT](https://assets.emqx.com/images/0e5a173ff8a357054f5f57aacec41bc6.png)

SCRAM 認証を開始するには、クライアントは、認証方法属性を SCRAM-SHA-256 に設定して、SCRAM 認証を使用する意図を示した CONNECT パケットを送信します。SHA-256 は、使用されるハッシュ関数を示します。認証データ属性は、クライアントファーストメッセージの内容を保存するために使用されます。認証方法属性は、サーバーが認証データ フィールドに含まれるデータをどのように解析して処理するかを決定します。

サーバーが SCRAM 認証をサポートしていない場合、またはクライアントファーストメッセージの内容が無効であることが判明した場合、サーバーは認証失敗の理由を示す理由コードを含む CONNACK パケットを返し、ネットワーク接続を閉じます。

それ以外の場合、サーバーは次のステップに進み、AUTH パケットを返し、理由コードを 0x18 に設定して認証の継続を示します。パケット内の認証方式は CONNECT パケットと同じになり、認証データ属性にはサーバーファーストメッセージの内容が含まれます。

サーバーファーストメッセージの内容が正しいことを確認した後、クライアントは理由コード 0x18 を含む AUTH パケットも返し、認証データ属性にはクライアント最終メッセージの内容が含まれます。

クライアント最終メッセージの内容が正しいことを確認した後、サーバーはクライアントの ID の検証を完了します。したがって、今回、サーバーは AUTH パケットを返しませんが、認証の成功を示す理由コード 0 を含む CONNACK パケットを返し、パケット内の認証データ属性を通じてサーバー最終メッセージを渡します。

サーバーの ID が正常に検証されると、クライアントはトピックのサブスクライブまたはメッセージのパブリッシュを続行できます。ただし、検証が失敗した場合、クライアントは DISCONNECT パケットを送信して接続を終了します。

## まとめ

強化された認証により、ユーザーはさらに多くの ID 検証方法を導入できるようになります。特定のニーズに適した認証方法を選択し、システムのセキュリティをさらに強化できます。

[EMQX](https://github.com/emqx/emqx)は、高いスケーラビリティと可用性で知られる広く使用されている MQTT ブローカーとして、ユーザーのセキュリティの確保を常に優先してきました。[パスワードベースの認証](https://docs.emqx.com/en/emqx/v5.0/access-control/authn/pwoverview.html)に加えて、EMQX は強化認証もサポートしています。ユーザーは、EMQX による SCRAM 認証を有効にして、MQTT インフラストラクチャのセキュリティ レベルを向上させることができます。詳細については、「[MQTT 5.0 強化認証」](https://docs.emqx.com/en/emqx/v5.0/access-control/authn/scram.html#configure-with-dashboard)を参照してください。



<section class="promotion">
    <div>
        無料トライアルEMQX Cloud
        <div class="is-size-14 is-text-normal has-text-weight-normal">IoT向けフルマネージド型MQTTサービス</div>
    </div>
    <a href="https://accounts.emqx.com/signup?continue=https://cloud-intl.emqx.com/console/deployments/0?oper=new" class="button is-gradient px-5">無料トライアル →</a>
</section>
