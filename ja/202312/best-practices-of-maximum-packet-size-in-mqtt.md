## 最大パケットサイズとは何ですか?

[MQTT パケット](https://www.emqx.com/en/blog/introduction-to-mqtt-control-packets)の理論上の最大長は268,435,456 バイトで、256 MB に相当します。ただし、リソースに制約のあるクライアントや、エッジ ゲートウェイとして動作する一部の MQTT サーバーは、このサイズのパケットを処理できない可能性があることは明らかです。

クライアントによってパケットの処理能力が大幅に異なる可能性があることを考えると、過度に大きなパケットを送信すると、ピアの通常のビジネス処理に影響を与えるだけでなく、ピアに直接的な負荷がかかる可能性があります。したがって、[最大パケットサイズ] プロパティを使用して、クライアントとサーバーが処理できる最大パケット長さをネゴシエートする必要があります。

クライアントはまず、CONNECT パケットの最大パケット サイズを使用して、サーバーがクライアントに送信できる最大許容パケット長を指定します。一方、サーバーは、CONNACK パケットの最大パケット サイズを使用して、クライアントがサーバーに送信できる最大許容パケット長を指定します。

![MQTT CONNECT packet](https://assets.emqx.com/images/1f64b4c59e8da8d446d823d6b8f20535.png)

接続が確立されたら、パケットを送信する際には双方がこの契約に従う必要があります。どちらの当事者も、合意された長さ制限を超えるパケットを送信することはできません。それ以外の場合、受信側は理由コード 0x95 を持つ DISCONNECT パケットを返し、ネットワーク接続を閉じます。

クライアントが CONNECT パケットに Will メッセージを設定すると、知らず知らずのうちに CONNECT パケットがサーバーで許可されている最大パケット長を超える可能性があることに注意することが重要です。このような場合、サーバーは理由コード 0x95 の CONNACK パケットで応答し、ネットワーク接続を閉じます。

## 送信者は制限内でどのように機能しますか?

クライアントの場合、パブリッシュしているかサブスクライブしているかに関係なく、アクティブな送信者として、長さの制限を超えないようにするために、パケットを複数の部分に分割して送信できます。

しかし、サーバーの場合、サーバーはメッセージの転送のみを担当し、メッセージのサイズを決定することはできません。したがって、転送されるメッセージのサイズがクライアントが受信できる最大値を超えていることが判明した場合、このメッセージはドロップされます。共有サブスクリプションの場合、サーバーはドロップするだけでなく、メッセージを受信できるグループ内の他のクライアントにメッセージを送信することも選択できます。

上で説明した 2 つの制御に加えて、クライアントであってもサーバーであっても、パケットのコンテンツをトリミングして長さを減らすことができます。レスポンダは、追加情報をピアに伝えるために、CONNACK や PUBACK などの応答パケットに User Property と Reason String という 2 つのプロパティを含めることができることがわかっています。

ただし、応答パケットの長さの最大制限を超える可能性は、まさにこれら 2 つの特性によって引き起こされます。明らかに、これらのプロパティを送信する優先順位は、プロトコルの適切なフローを確保することよりも低くなります。したがって、応答側は、パケットの長さが制限を超えた場合にこれら 2 つのプロパティをパケットから削除して、応答パケットが正常に送信される可能性を最大化できます。

これは応答パケットにのみ適用され、PUBLISH パケットには適用されないことをお知らせします。PUBLISH パケットの場合、 User Property はパケットの一部とみなされ、サーバーはパケット配信を確実にするためにパケットから User Property を削除しようとするべきではありません。

## デモ

1. インストールされた[MQTTX](https://mqttx.app/ja)を開きます。

2. MQTT 接続を作成し、最大パケット サイズを 100 に設定して、[無料のパブリック MQTT サーバー](https://www.emqx.com/ja/mqtt/public-mqtt5-broker)に接続します。

   ![Create an MQTT connection](https://assets.emqx.com/images/784f1078a559f75b0c9ed10f30a5a218.png)

3. 接続が成功すると、Wireshark パケット キャプチャ ツールを通じて、サーバーから返された CONNACK パケットの最大パケット サイズ プロパティが 1048576 であることがわかります。これは、クライアントがパブリック MQTT に最大 1 KB のパケットしか送信できないことを意味します。毎回サーバー:

   ![Wireshark packet capture tool](https://assets.emqx.com/images/0d6c9d52f8dbb2c052119386f0bb10b3.png)

4. MQTTX に戻り、トピックをサブスクライブします`mqttx_0c668d0d/demo`。

   ![Subscribe to the topic "mqttx_0c668d0d/demo"](https://assets.emqx.com/images/8653151cecd5a961b77ba24a40373a4a.png)

5. 次に、2 つのメッセージをトピックにパブリッシュします`mqttx_0c668d0d/demo`。1 つは長さ 5 バイト、もう 1 つはサイズが 172 バイトです。長さ 5 バイトのメッセージのみが受信され、100 バイトの長さ制限を超える別のメッセージはサーバーによって転送されないことがわかります。

   ![Publish two messages to the topic "mqttx_0c668d0d/demo"](https://assets.emqx.com/images/833e937b1195e1ca9e11f719e350053d.png)



<section class="promotion">
    <div>
        無料トライアルEMQX Cloud
        <div class="is-size-14 is-text-normal has-text-weight-normal">IoT向けフルマネージド型MQTTサービス</div>
    </div>
    <a href="https://accounts.emqx.com/signup?continue=https://cloud-intl.emqx.com/console/deployments/0?oper=new" class="button is-gradient px-5">無料トライアル →</a>
</section>