# Volt と Meteor の違いは？

Volt と Meteor は多くの点で類似しています。その理由は主に、どちらも同じ問題を解決しようとするものだからです。Volt が Ruby を使って、Meteor が JavaScript を使うという違いの他にも、いくつかの大きな違いがあります:

## コンポーネント

大きな違いの1つは、Volt アプリケーションがコンポーネントから構築されるということです。このことは、コードの (Gem としての) 再利用性を高め、モジュラー化を推進するものだと考えています。また、このことによって、「ページの読み込みの境界」を設定することができ、「シングルページアプリケーション」を構築することが可能になります。これは、最初のページの読み込み時に、クライアント側でアプリケーションすべてを読み込む必要がないことを意味しています。あるパートにおいて、ブラウザが通常の get リクエストを発行し、他のセクションを読み込む、ということができます。

## クライアント側のデータベースアクセス

Meteor では、データベースを直接操作します (insert, create, etc...) が、Volt ではオブジェクトとして扱います。クラスは Volt::Model を継承して作ることができます。オブジェクトの設定にしたがって、どこにデータが永続化されるかが決定されます。オブジェクト自体が直接永続化機構になっているわけではないので (リポジトリパターンを想像してください)、個別にテストをすることが可能です。また、オブジェクトに関連するビジネスロジックを配置する場所を提供し、それらのロジック (主にバリデーションやパーミッション) が侵されることを防止します。

## データハンドリング

もう1つの大きな違いは、データをハンドリングする方法です。Meteor では、publish によってページがロードされたとき、クライアントは必要データを要求します。それに応じて、クライアント側でデータを subscribe します。subscribe で publish のオプションを渡すことも可能ですが、これは私の考えだとちょっとトリッキーに感じます。このようにすべき主な理由は、コールバックを待つことなく、クライアント上でデータを操作することができるようにするためです。そして、このことの問題点は、多くのケースにおいて、サーバーとのラウンドトリップを待つ必要があることです。例としてはユニーク性のバリデーションがあげられます。これは確認のためにサーバーへのアクセスが必要になります。Volt では、データの読み込みは「どれがリアクティブに監視されているか」によって決定されます。したがって、save などは promise を返し、結果を待つ必要があればそれを利用して待つことができます。IcedCoffeeScript のような Opal の機能拡張を追加し、async/await スタイルのシンタックスを提供することで、promise の API を透過的、かつ同期的に扱えるようにすることを予定しています。(まだ対応中の段階です)。Meteor アプリケーションでよく見られることとして、クライアント側で mini-mongo を利用するのをやめて、すべてを Meteor のメソッドで処理する、というものです。これは mongo のデータの扱いに難しい問題があることを示していると思います。もう1つは、読み込み時にどのデータが必要を判断するのが難しいため、大量のデータがクライアントに送信されるということです。Volt の方法が完璧だと言うつもりはありませんが、Meteor のやり方の問題のいくつかは解決することができると思っています。

## その他

その他の異なる点をざっとあげます。いくつか小さな違いがあります。

- Meteor と比較して、 Volt は規約やファイルの構成をより多く利用します。
- Volt のルーターは完全に異なるものになっています。
- Volt は VC から $11MM の融資を受けていません :-)

さらに多くの違いはありますが、現時点で頭に入れておいてほしいものは以上です。
