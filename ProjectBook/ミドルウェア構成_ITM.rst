2.2.4. Middleware設計 ITM
-------------------------------

2.2.4.1. TEMS設計
^^^^^^^^^^^^^^^^^^^^^

(a) TEMS導入パラメーター
"""""""""""""""""""""""""""

.. csv-table::
    :header-rows: 1

    項目,内容,備考
    ITM導入ディレクトリ名,/opt/IBM/ITM
    Encription Key,IBMTivoliMonitoringEncryptionKey

(b) TEMS構成パラメーター
"""""""""""""""""""""""""""

.. csv-table::
    :header-rows: 1

	項目,内容,備考
	ハブまたはリモート [1=\*LOCAL、2=\*REMOTE] ,\*LOCAL (=Hub)
	TEMS ホスト名,Hub_XXXXXX0,TEMS名
	ネットワーク・プロトコル 1,ip.pipe
	IP.PIPE ポート番号,1918
	KDC_PARTITION の名前,null
	KDC_PARTITIONFILE のパスと名前,$CANDLEHOME/tables/Hub_XXXXXX0/partition.txt
	ホット・スタンバイ TEMS ホスト名,0,[0= なし]
	「オプションの 1 次ネットワーク名」,XXXXXX0
	セキュリティー: ユーザーを検証しますか? ,はい,[1= はい、2= いいえ]
	LDAPセキュリティー: LDAPでユーザーを検証しますか?,いいえ,[1= はい、2= いいえ]
	Tivoli Event Integration Facility?,はい,[1= はい、2= いいえ]
	EIF サーバー?,XXXXXX0
	EIF ポート?,5529
	ワークフロー・ポリシー/Tivoli Emitter Agent イベント転送,転送する,[1=しない、2= する]
	ユーザーアクセス,update : sysadmin@Hub_XXXXXX0,セキュリティ設定

(c) TEMS環境変数
""""""""""""""""""

.. csv-table::
    :header-rows: 1

    設定ファイル,追加設定値,備考
    $CANDLEHOME/config/ms.ini,なし
    $CANDLEHOME/config/rmntnca1_ms_rmntnca0.ini,なし
    $CANDLEHOME/config/rmntnca2_ms_rmntnca0.ini,"""$CANDLEHOME/config/rmntnca1_ms_rmntnca0.ini""をコピーして作成",HA構成設定

(d) アプリケーションサポート
""""""""""""""""""""""""""""""

  :有効化するアプリケーションサポート: lz, nt, ux, ul, um, a2, a3, a4, a5, a6

2.2.4.2. TEPS設計
^^^^^^^^^^^^^^^^^^^^^

(a) TEPS導入パラメーター
""""""""""""""""""""""""""

.. csv-table::
    :header-rows: 1

    項目,内容,備考
    ITM導入ディレクトリ名,/opt/IBM/ITM
    Encription Key,IBMTivoliMonitoringEncryptionKey

(b) TEPS構成パラメーター
""""""""""""""""""""""""""

.. csv-table::
    :header-rows: 1

	項目,内容,備考
	"""ITM用の共通イベント・コンソール"" 設定を編集しますか?",いいえ,[1= はい、2= いいえ]
	このエージェントは TEMS に接続しますか?,はい,[1= はい、2= いいえ]
	TEMS ホスト名,XXXXXX0
	ネットワーク・プロトコル1,ip.pipe
	IP.PIPE ポート番号,1918
	KDC_PARTITION の名前を入力します,0,[0= なし]
	「オプションの 1 次ネットワーク名」,XXXXXX0
	TEP クライアントの SSL を使用可能にします,いいえ,[1= はい、2= いいえ]
	DB2 インスタンス名,db2inst1
	DB2 管理 ID,db2inst1
	TEPS DB2 データベース名,TEPS
	TEPS DB2 データベースのログイン ID,itmuser
	ウェアハウスとして DB2 または Oracleを使用していますか?,D,"[D=DB2, J=JDBC]"
	ウェアハウスのデータベース名,WAREHOUS
	ウェアハウスのユーザー ID,itmuser
	LDAP セキュリティー: LDAP でユーザーを検証しますか?,いいえ,[1= はい、2= いいえ]

(c) TEPS環境変数
""""""""""""""""""

.. csv-table::
    :header-rows: 1

    設定ファイル,追加設定値,備考
    $CANDLEHOME/config/cq.ini,KFW_CMW_SET_TEC_SEV=Y,イベントの重大度マッピング設定
    $CANDLEHOME/aix536/cq/bin/lnxenv,KFW_INTERFACE_cnps_HOST=rmntnca0,HA構成設定

(d) TEPSEHostname
"""""""""""""""""""

    導入後、次のコマンドでTEPSEHostnameのUpdateを行う::

    TEPSEHostname設定値,XXXXXX0
    TEPSEHostname変更コマンド,# sh $CANDLEHOME/aix536/iw/scripts/updateTEPSEHostname.sh XXXXXX1 XXXXXX0

2.2.4.3. DB2設計
^^^^^^^^^^^^^^^^^^^^

(a) DB2導入パラメーター(Primary)
""""""""""""""""""""""""""""""""""""

.. csv-table::
    :header-rows: 1

	項目,内容,備考
	インストール・タイプ,標準
	インストール・ディレクトリ,/opt/IBM/db2/V9.5
	SAMP Base Component,インストールしない
	DASユーザー情報,dasusr1
	DB2インスタンスのセットアップ,作成する
	インスタンスパーティションオプション,単一パーティション・インスタンス
	DB2インスタンス所有者ユーザー情報,db2inst1
	fencedユーザーのユーザー情報,db2fenc1
	DB2ツールカタログ,準備しない
	通知のセットアップ,セットアップしない

(b) DB2導入パラメーター(Secondary)
""""""""""""""""""""""""""""""""""""""

.. csv-table::
    :header-rows: 1

	項目,内容,備考
	インストール・タイプ,標準
	インストール・ディレクトリ,/opt/IBM/db2/V9.5
	SAMP Base Component,インストールしない
	DASユーザー情報,dasusr1
	DB2インスタンスのセットアップ,作成しない
	fencedユーザーのユーザー情報,db2fenc1
	DB2ツールカタログ,準備しない
	通知のセットアップ,セットアップしない

(c) DB2構成ファイル
"""""""""""""""""""""

.. csv-table::
    :header-rows: 1

    ホスト名,ファイル名,設定値,備考
    XXXXXX1,/opt/IBM/db2/V9.5/profiles.reg,db2inst1,有効インスタンス名
    XXXXXX2,/opt/IBM/db2/V9.5/profiles.reg,db2inst1,有効インスタンス名
    XXXXXX0,/home/db2inst1/sqllib/db2nodes.cfg,0 XXXXXX1 0,インスタンス起動時の通信先設定
    XXXXXX0,/home/db2inst1/sqllib/db2nodes.cfg.XXXXXX1,0 XXXXXX1 0,HA構成用ファイル（切り替えスクリプトで使用）
    XXXXXX0,/home/db2inst1/sqllib/db2nodes.cfg.XXXXXX2,0 XXXXXX2 0,HA構成用ファイル（切り替えスクリプトで使用）

.. note:: これ以外に、副機の/etc/servicesへ、正機の/etc/servicesからDB2インスタンス用通信エントリーをコピーして追記する
          | /etc/servicesの設定値は、『本書2.2.1.4. その他設計 (d)/etc/services』を参照。

2.2.4.4. イベント転送設計
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. csv-table::
    :header-rows: 1

    項目名,設定ファイル,設定対象,設定ファイルパス,備考
    イベント転送設定,configファイル,XXXXXX0,$OMNIHOME/tables/Hub_rmntnca0/TECLIB/om_tec.config,イベント宛先・フィルター
    イベントマップ（全般）,mapファイル,XXXXXX0,$OMNIHOME/tables/Hub_rmntnca0/TECLIB/knrkos.map,イベントスロットマップ
    イベントマップ（追加）,mapファイル,XXXXXX0,$OMNIHOME/tables/Hub_rmntnca0/TECLIB/knrkos-add.map,イベントスロットマップ
    イベントマップ（UA）,mapファイル,XXXXXX0,$OMNIHOME/tables/Hub_rmntnca0/TECLIB/knrkua.map,イベントスロットマップ

(a) イベント転送設定
""""""""""""""""""""""

.. csv-table::
    :header-rows: 1

	設定項目,設定値,備考
	ServerLocation,XXXXXX0,イベント送信宛先ホスト名
	ServerPort,5529,イベント送信宛先ポート番号
	RetryInterval,5
	getport_total_timeout_usec,50500
	NO_UTF8_CONVERSION,YES
	ConnectionMode,co
	BufferEvents,YES
	BufEvtMaxSize,4096
	BufEvtPath,./TECLIB/om_tec.cache
	FilterMode,OUT
	Filter:Class,ITM_Generic;master_reset_flag='';
	Filter:Class,re:'ITM_.*';integration_type='U';

(b) イベントマップ
""""""""""""""""""""

イベントマップファイルで、各ITMの監視シチュエーションで生成されるイベントのスロットを設定する

.. note:: 各イベントマップは、別途成果物として管理
