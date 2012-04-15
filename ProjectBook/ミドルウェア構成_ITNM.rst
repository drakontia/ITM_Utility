2.2.3. Middleware設計 ITNM
--------------------------------

2.2.3.1. 導入パラメーター
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

(a) Primary ITNM, ObjectServer, TIP, MySQL
"""""""""""""""""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

	項目,内容,備考
	Netcool導入ディレクトリ名,/opt/IBM/netcool
	インストール・タイプ,カスタム・インストール
	インストール・オプション（サーバーの数）,マルチサーバーのインストール
	インストール・オプション（デフォルト値）,設定をカスタマイズする
	FIPS準拠,指定しない
	インストールするコンポーネント,イベント管理ソフトウェアのインストール,Netcool/OMNIbusの導入
	ユーザー・コンソールのインストール,Netcool WebGUIの導入
	Network Managerコア・コンポーネントのインストール,ITNMの導入
	トポロジー・データベースのインストール,MySQLの導入
	ObjectServer名,NCOMS_XXX1
	ObjectServerポート,4100,※デフォルト値
	TIPディレクトリー名,/opt/IBM/tip
	TIP HTTPポート,16310,※デフォルト値
	TIP管理者アカウント名,tipadmin,※デフォルト値
	使用する認証サービス,ObjectServer
	NMドメイン名,NCNET_XXX1
	ディスカバリー,なし
	MySQLデータベース・ポート,3306
	MySQL管理者アカウント名,root,※デフォルト値
	MySQLデータベース・アカウント名,ncim,※デフォルト値

(b) Secondary ITNM, ObjectServer, TIP
"""""""""""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

	項目,内容,備考
	Netcool導入ディレクトリ名,/opt/IBM/netcool
	インストール・タイプ,カスタム・インストール
	インストール・オプション（サーバーの数）,マルチサーバーのインストール
	インストール・オプション（デフォルト値）,設定をカスタマイズする
	FIPS準拠,指定しない
	インストールするコンポーネント,イベント管理ソフトウェアのインストール,Netcool/OMNIbusの導入
	ユーザー・コンソールのインストール,Netcool WebGUIの導入
	Network Managerコア・コンポーネントのインストール,ITNMの導入
	ObjectServer名,DENCO_XXX2
	ObjectServerポート,4100,※デフォルト値
	TIPディレクトリー名,/opt/IBM/tip
	TIP HTTPポート,16310,※デフォルト値
	TIP管理者アカウント名,tipadmin,※デフォルト値
	使用する認証サービス,ObjectServer
	NMドメイン名,DENET_XXX2
	ディスカバリー,なし
	トボロジー・データベースの選択,既存のMySQLデータベース
	テーブルの作成,作成しない
	MySQLデータベース・サーバー・ホスト名,rmntnca0
	MySQLデータベース・ポート,3306
	MySQL管理者アカウント名,root,※デフォルト値
	MySQLデータベース・アカウント名,ncim,※デフォルト値

2.2.3.2. ProcessControl設計
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ProcessControl設定項目一覧

.. csv-table::
  :header-rows: 1

	項目名,設定ファイル,設定対象,設定ファイルパス,備考
	サービス設定(Primary) cfgファイル,$OMNIHOME/etc/precision/CtrlServices.NCNET_XXX1.cfg,ITNM稼働設定
	サービス通信設定(Primary) cfgファイル,$OMNIHOME/etc/precision/ServiceData.cfg,ITNMプロセス通信設定
	OMNIbus起動制御設定(Primary) スクリプト,$OMNIHOME/precision/bin/nco_control.sh,PA制御スクリプト
	サービス設定(Secondary) cfgファイル,$OMNIHOME/etc/precision/CtrlServices.DENET_XXX2.cfg,ITNM稼働設定
	サービス通信設定(Secondary) cfgファイル,$OMNIHOME/etc/precision/ServiceData.cfg,ITNMプロセス通信設定
	OMNIbus起動制御設定(Secondary) スクリプト,$OMNIHOME/precision/bin/nco_control.sh,PA制御スクリプト

(a) サービス設定(Primary)
"""""""""""""""""""""""""""""

.. note:: 変更箇所のみ記述する

設定記述凡例::

  insert into services.inTray(
  "serviceName, servicePath, domainName, argList, dependsOn, retryCount"
  ) values (
    <サービス名>
    <ディレクトリ名>
    <ドメイン名>
    [ <引数1>, <引数2>, <引数3>, （略）, <引数X> ],
    [ <前提サービス名1>, <前提サービス名2>,（略）,<前提サービス名X> ],
    <再試行回数>
  }

.. csv-table::
  :header-rows: 1

	サービス名,変更区分,設定項目,設定値,備考
	nco_p_ncpmonitor,引数変更,ディレクトリ名,$NCHOME/probes/$PLATFORM
	ドメイン名,$PRECISION_DOMAIN
	引数1,-domain
	引数2,$PRECISION_DOMAIN
	引数3,-server
	引数4,NCOMS_XXXV,接続インターフェース名を変更
	引数5,-debug
	引数6,0
	引数7,-messagelevel
	引数8,warn
	再試行回数,5
	ncp_ncogate,引数変更,ディレクトリ名,$PRECISION_HOME/platform/$PLATFORM/bin
	ドメイン名,$PRECISION_DOMAIN
	引数1,-domain
	引数2,$PRECISION_DOMAIN
	引数3,-server
	引数4,NCOMS_XXXV,接続インターフェース名を変更
	引数5,-latency
	引数6,100000
	引数7,-debug
	引数8,0
	引数9,-messagelevel
	引数10,warn
	引数11,-backup,引数追加
	前提サービス名1,nco_f_amos
	再試行回数,5
	mysql.server,サービス削除,-,-,エントリー削除（コメント化）
	ncp_virtualdomain,新規追加,ディレクトリ名,$PRECISION_HOME/platform/$PLATFORM/bin
	ドメイン名,$PRECISION_DOMAIN
	引数1,-domain
	引数2,NCNET_XXX1
	引数3,-virtualDomain
	引数4,NCNET_XXXV
	引数5,-backupDomain
	引数6,DENET_XXX2
	引数7,-latency
	引数8,60000
	引数9,-debug
	引数10,0
	前提サービス名1,ncp_model
	再試行回数,5

(b) サービス通信設定(Primary)
"""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

  設定内容
  SERVICE: MulticastService DOMAIN: ANY_PRECISION_DOMAIN ADDRESS: xxx.xxx.xxx.xxx PORT: 33000
  SERVICE: ncp_config DOMAIN: NCNET_XXX1 ADDRESS: xxx.xxx.xxx.xxx PORT: 7968 SERVERNAME: XXXXXX DYNAMIC: NO
  SERVICE: Helper DOMAIN: NCNET_XXX1 ADDRESS: xxx.xxx.xxx.xxx PORT: 54775 SERVERNAME: XXXXXX DYNAMIC: NO
  SERVICE: NCP.VIRTUALDOMAIN.QUERY DOMAIN: NCNET_XXX1 ADDRESS: xxx.xxx.xxx.xxx PORT: 54795 SERVERNAME: XXXXXX DYNAMIC: NO
  SERVICE: ncp_disco.831594 DOMAIN: NCNET_XXX1 ADDRESS: xxx.xxx.xxx.xxx PORT: 54797 SERVERNAME: XXXXXX DYNAMIC: NO

(c) OMNIbus起動制御設定(Primary)
""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

	設定項目,内容,備考
	set_edited_vars,NCHOME=/opt/IBM/netcool,Set directory for $NCHOME
	OMNIHOME=/opt/IBM/netcool/omnibus,Set directory for $OMNIHOME
	"NCO_PA=""PA_XXX1""",Set Process Agent's name
	SECURE=Y,Y/N run Process Agent in secure mode
	NETCOOL_LICENSE_FILE=27000@XXXXXX
	ITNM_CONTROL_FUNCS=/opt/IBM/netcool/precision/bin/itnm_control_functions.sh
	NCO_PERL_CMD=__NCO_PERL_CMD__
	NCO_PERL_SCRIPT=__NCO_PERL_SCRIPT__
	export NCHOME OMNIHOME NCO_PA NETCOOL_LICENSE_FILE
	MTTRAPD_PROBE=nco_p_mttrapd
	MTTRAPD_PROBE_LOCATION=$OMNIHOME/probes
	OMNI_DAT=${NCHOME}/etc/omni.dat

OMNIbus起動制御設定スクリプトの中で、PAの起動を行う次の行について記述を修正::
  修正前
  ${OMNIHOME}/bin/nco_pad -name ${NCO_PA} $stacksize -authenticate PAM -secure > /dev/null 2> /dev/null &
  修正後
  ${OMNIHOME}/bin/nco_pad -name ${NCO_PA} $stacksize -secure > /dev/null 2> /dev/null &

(d) サービス設定(Secondary),※ 変更分のみ
""""""""""""""""""""""""""""""""""""""""""""""

設定記述凡例::

  insert into services.inTray(
  "serviceName, servicePath, domainName, argList, dependsOn, retryCount"
  ) values (
    <サービス名>
    <ディレクトリ名>
    <ドメイン名>
    [ <引数1>, <引数2>, <引数3>, （略）, <引数X> ],
    [ <前提サービス名1>, <前提サービス名2>,（略）,<前提サービス名X> ],
    <再試行回数>
  }

.. csv-table::
  :header-rows: 1

	サービス名,変更区分,設定項目,設定値,備考
	ncp_model,引数変更,ディレクトリ名,$PRECISION_HOME/platform/$PLATFORM/bin
	ドメイン名,$PRECISION_DOMAIN
	引数1,-domain
	引数2,$PRECISION_DOMAIN
	引数3,-latency
	引数4,100000
	引数5,-debug
	引数6,0
	引数7,-messagelevel
	引数8,warn
	引数9,-backup,引数追加
	前提サービス名1,ncp_config
	前提サービス名2,ncp_store
	前提サービス名3,ncp_class
	再試行回数,5
	nco_p_disco,サービス削除,-,-,エントリーを削除（コメント化）
	ncp_poller,引数変更,ディレクトリ名,$PRECISION_HOME/platform/$PLATFORM/bin
	ドメイン名,$PRECISION_DOMAIN
	引数1,-domain
	引数2,$PRECISION_DOMAIN
	引数3,-latency
	引数4,100000
	引数5,-debug
	引数6,0
	引数7,-messagelevel
	引数8,warn
	引数9,-primaryDomain,引数追加
	引数10,NCNET_XXX1,引数追加
	前提サービス名1,nco_p_ncpmonitor
	前提サービス名2,ncp_ncogate
	再試行回数,5
	nco_p_ncpmonitor,引数変更,ディレクトリ名,$NCHOME/probes/$PLATFORM
	ドメイン名,$PRECISION_DOMAIN
	引数1,-domain
	引数2,$PRECISION_DOMAIN
	引数3,-server
	引数4,NCOMS_XXXV,接続インターフェース名を変更
	引数5,-debug
	引数6,0
	引数7,-messagelevel
	引数8,warn
	再試行回数,5
	ncp_ncogate,引数変更,ディレクトリ名,$PRECISION_HOME/platform/$PLATFORM/bin
	ドメイン名,$PRECISION_DOMAIN
	引数1,-domain
	引数2,$PRECISION_DOMAIN
	引数3,-server
	引数4,NCOMS_XXXV,接続インターフェース名を変更
	引数5,-latency
	引数6,100000
	引数7,-debug
	引数8,0
	引数9,-messagelevel
	引数10,warn
	引数11,-backup,引数追加
	前提サービス名1,nco_f_amos
	再試行回数,5
	mysql.server,サービス削除,-,-,エントリー削除（コメント化）
	ncp_virtualdomain,新規追加,ディレクトリ名,$PRECISION_HOME/platform/$PLATFORM/bin
	ドメイン名,$PRECISION_DOMAIN
	引数1,-domain
	引数2,DENET_XXX2
	引数3,-virtualDomain
	引数4,NCNET_XXXV
	引数5,-primaryDomain
	引数6,NCNET_XXX1
	引数7,-latency
	引数8,60000
	引数9,-debug
	引数10,0
	前提サービス名1,ncp_model
	再試行回数,5

(e) サービス通信設定(Secondary)
"""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

  設定内容
  SERVICE: MulticastService DOMAIN: ANY_PRECISION_DOMAIN ADDRESS: xxx.xxx.xxx.xxx PORT: 33000
  SERVICE: NCP.VIRTUALDOMAIN.QUERY DOMAIN: NCNET_XXX1 ADDRESS: xxx.xxx.xxx.xxx PORT: 54795 SERVERNAME: XXXXXX DYNAMIC: NO
  SERVICE: ncp_config DOMAIN: DENET_XXX2 ADDRESS: xxx.xxx.xxx.xxx PORT: 7968 SERVERNAME: XXXXXX DYNAMIC: NO

(f) OMNIbus起動制御設定(Secondary)
""""""""""""""""""""""""""""""""""""""
set_edited_vars

.. csv-table::
  :header-rows: 1

	内容,備考
	NCHOME=/opt/IBM/netcool,Set directory for $NCHOME
	OMNIHOME=/opt/IBM/netcool/omnibus,Set directory for $OMNIHOME
	"NCO_PA=""PA_XXX2""",Set Process Agent's name
	SECURE=Y,Y/N run Process Agent in secure mode
	NETCOOL_LICENSE_FILE=27000@XXXXXX
	ITNM_CONTROL_FUNCS=/opt/IBM/netcool/precision/bin/itnm_control_functions.sh
	NCO_PERL_CMD=__NCO_PERL_CMD__
	NCO_PERL_SCRIPT=__NCO_PERL_SCRIPT__
	export NCHOME OMNIHOME NCO_PA NETCOOL_LICENSE_FILE
	MTTRAPD_PROBE=nco_p_mttrapd
	MTTRAPD_PROBE_LOCATION=$OMNIHOME/probes
	OMNI_DAT=${NCHOME}/etc/omni.dat

OMNIbus起動制御設定スクリプトの中で、PAの起動を行う次の行について記述を修正::
  修正前
  ${OMNIHOME}/bin/nco_pad -name ${NCO_PA} $stacksize -authenticate PAM -secure > /dev/null 2> /dev/null &
  修正後
  ${OMNIHOME}/bin/nco_pad -name ${NCO_PA} $stacksize -secure > /dev/null 2> /dev/null &

2.2.3.3. MIB管理設計
^^^^^^^^^^^^^^^^^^^^^^^^

.. csv-table::
  :header-rows: 1

  MIBファイル保管ディレクトリ,$NCHOME/precision/mibs
  定義更新時実行コマンド,# . $NCHOME/env.sh
  (ncosysユーザーで実行) # ncp_mib -dryrun
  # ncp_mib -domain NCNET_XXXV -messagelog $NCHOME/log/precision/ncp_mib.log

2.2.3.4. Monitor Probe設計
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. csv-table::
  :header-rows: 1

	項目名,設定ファイル,設定対象,設定ファイルパス,備考
	プロパティ(Primary) propsファイル,$NCHOME/probes/aix5/nco_p_ncpmonitor.props,Monitor Probeの稼働設定
	ルール(Primary) rulesファイル,$NCHOME/probes/aix5/nco_p_ncpmonitor.rules,イベント処理プログラム
	プロパティ(Secondary) propsファイル,$NCHOME/probes/aix5/nco_p_ncpmonitor.props,Monitor Probeの稼働設定
	ルール(Secondary) rulesファイル,$NCHOME/probes/aix5/nco_p_ncpmonitor.rules,イベント処理プログラム

(a) Primaryサーバー Monitor Probeプロパティ設定
"""""""""""""""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

  設定項目,設定値,備考
  MessageLevel,'warn',Probeログレベル（デフォルト値）
  MessageLog,'$NCHOME/log/precision/nco_p_ncpmonitor.<domain>.log',Probeログ出力先（デフォルト値）
  PollServer,30

(b) Primaryサーバー ルール設定
""""""""""""""""""""""""""""""""

ルールファイル構成

.. csv-table::
  :header-rows: 1

  ファイル名,ファイルパス,備考
  基本ルール,$NCHOME/probes/aix5/nco_p_ncpmonitor.rules

(c) Secondaryサーバー Monitor Probeプロパティ設定
"""""""""""""""""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

  設定項目,設定値,備考
  MessageLevel,'warn',Probeログレベル（デフォルト値）
  MessageLog,'$NCHOME/log/precision/nco_p_ncpmonitor.<domain>.log',Probeログ出力先（デフォルト値）
  PollServer,30

(b) Secondaryサーバー ルール設定
""""""""""""""""""""""""""""""""""

ルールファイル構成

.. csv-table::
  :header-rows: 1

  ファイル名,ファイルパス,備考
  基本ルール,$NCHOME/probes/aix5/nco_p_ncpmonitor.rules

.. note:: 各ルールは別途成果物として管理

2.2.3.5. MySQL設計
^^^^^^^^^^^^^^^^^^^^^^

.. csv-table::
  :header-rows: 1

	起動停止制御,HAアプリケーションサーバースクリプトで実施
	アクセス設定,設定値,備考
	起動停止権限,%@root
	%@ncim
	localhost@root
	localhost@ncim
	XXXXX@root
	XXXXX@ncim
	XXXXX@root,設定追加
	DB接続権限,%@root
	%@ncim
	localhost@root
	localhost@ncim
	XXXXX@root
	XXXXX@ncim
	XXXXX@ncim,設定追加
	DBアクセス権限,%@root
	（全データベース）,%@ncim
	localhost@root
	localhost@ncim
  XXXXX@root
  XXXXX@ncim
  XXXXX@ncim,設定追加
