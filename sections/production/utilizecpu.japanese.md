# すべての CPU コアを利用する

<br/><br/>

### 一段落説明

意外ではないかもしれませんが、基本的な形として、Node は単一のスレッド、単一のプロセス、単一の CPU 上で動作します。4もしくは8個のCPUがある頑丈なハードにお金を払っておいて、1つだけを利用するのはバカバカしく聞こえるのではないでしょうか？ 中規模なアプリケーションに適用する最速のソリューションは、10行のコードで各論理コアのためにプロセスを生成し、ラウンドロビン形式でプロセス間のリクエストをルーティングする、Node クラスターを使うことです。さらに良いのは、シンプルなインタフェースとクールなモニタリング UI でクラスタリングモジュールの体裁を整えている PM2 を使用することです。このソリューションは従来のアプリケーションには適していますが、一流のパフォーマンスと堅牢な DevOps フローを必要とするアプリケーションには不向きかもしれません。これらの高度なユースケースでは、カスタムデプロイスクリプトを使用して NODE プロセスをレプリケートし、nginx などの専用ツールを使用してバランシングを行うか、デプロイとプロセスのレプリケーションのための高度な機能を持つ AWS ECS や Kubernetees などのコンテナエンジンを使用することを検討してみてはいかがでしょうか。

<br/><br/>

### 比較: Node クラスタと nginx それぞれを使ったバランシング

![バランシングの比較 Node クラスタ vs nginx](../../assets/images/utilizecpucores1.png "バランシングの比較 Node クラスタ vs nginx")

<br/><br/>

### 他のブロガーが言っていること

* [Node.js documentation](https://nodejs.org/api/cluster.html#cluster_how_it_works) より:
> ... 2番目のアプローチである Node クラスタは、理論的には最高のパフォーマンスを発揮するはずです。しかし、実際には、オペレーティングシステムのスケジューラのばらつきのために、ディストリビューションは非常にアンバランスになる傾向があります。8つの全てのプロセスのうち、70%以上の接続が2つのプロセスで終了するという負荷が観測されています。 ...

* ブログ [StrongLoop](https://strongloop.com/strongblog/best-practices-for-express-in-production-part-two-performance-and-reliability/) より:
> ... クラスタリングは Node のクラスタモジュールで可能になります。これにより、マスタープロセスがワーカープロセスを spawn し、ワーカー間で着信接続を分配できるようになります。しかし、このモジュールを直接使用するよりも、自動的にそれを行ってくれる多くのツールのうちの一つを使用する方がはるかに良いでしょう; node-pm やクラスタサービスなど ...

* Medium のポスト [Node.js process load balance performance: comparing cluster module, iptables, and Nginx](https://medium.com/@fermads/node-js-process-load-balancing-comparing-cluster-iptables-and-nginx-6746aaf38272) より
> ... Node クラスタは実装と設定が簡単で、他のソフトウェアに依存することなくノードの領域内に保持されます。マスタープロセスは、他のソリューションに比べてリクエスト率が若干低くても、作業者のプロセスとほぼ同等に機能することを覚えておいてください。 ...