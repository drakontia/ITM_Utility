2.2.2. Middleware設計 Netcool\/OMNIbus
--------------------------------------------

2.2.2.1. 導入パラメーター
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ITNMインストーラーを利用してインストールを実施するため、2.2.3.1. 導入パラメーター（ITNM）に記載

2.2.2.2. 共通項目設計
^^^^^^^^^^^^^^^^^^^^^^^^^
共通項目一覧

.. csv-table::
  :header-rows: 1

  項目名,設定ファイル,設定対象,設定ファイルパス,備考
  インターフェース,インターフェースファイル,"全UNIX, Linuxサーバー",$NCHOME/etc/omni.dat,Netcool/OMNIbusの各種モジュールの通信宛先設定

(a) インターフェース設計
""""""""""""""""""""""""""

※ インターフェース名の最大長は11文字。

.. csv-table::
  :header-rows: 1

	インターフェース名,Pri/Bkup,通信宛先,ポート,SSL,備考
	NCOMS_XXXV,Primary,4100,NO,ObjectServer Virtual Interface
	Backup,4100,NO
	NCOMS_XXX1,Primary,4100,NO,Primary ObjectServer
	DENCO_XXX2,Primary,4100,NO,Secondary ObjectServer
	BIGW_XXX2,Primary,4300,NO,Bi-Gateway
	UNIGW_XXX1,Primary,4350,NO,Uni-Gateway
	UNIGW_XXX2,Primary,4350,NO,Uni-Gateway
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)
	PA\_XXXX,Primary,4200,NO,Process Agent(XXXサーバー)

2.2.2.3. ObjectServer設計
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ObjectServer設定項目一覧

.. csv-table::
  :header-rows: 1

  項目名,設定ファイル,設定対象,設定ファイルパス,備考
  プロパティ(Primary),propsファイル,,$OMNIHOME/etc/NCOMS_XXX1.props,Primary ObjectServerの稼働設定
  プロパティ(Sedondary),propsファイル,,$OMNIHOME/etc/NCOMS_XXX2.props,Secondary ObjectServerの稼働設定
  イベントデータベース列定義,alerts.status,,,Administratorコンソールから設定を変更

.. note::
  ※ プロパティ設定は、明示的に指定しない設定値はデフォルトの値が適用される（デフォルト値は、propsファイル内にコメントとして記載）

(a) ObjectServerプロパティ設定(Primary)
"""""""""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

  設定項目,デフォルト値,設定値,備考
  Connections,30,1000,ObjectServerへの最大接続数
  Iduc.ListeningPort,0,33201,ObjectServerからAEN/Gatewayへの通信のサーバー側ポート
  PA.Name,NCO_PA,PA\_XXXX,ObjectServerから直接接続されるPAのインターフェース名
  PA.Password,(暗号化して記載）,(暗号化して記載）,暗号化には、$OMNIHOME/bin/nco_g_cryptコマンドを利用
  PA.UserName,ncopa,ObjectServerがPAに接続する際のユーザー名（認証情報）
  Sec.ExternalAuthentication,none,PAM,ユーザー認証方式（AIXユーザーPAM認証）

(b) ObjectServerプロパティ設定(Secondary)
"""""""""""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

  設定項目,デフォルト値,設定値,備考
  BackupObjectServer,FALSE,TRUE,起動時にSecondaryサーバーとして稼働を開始する
  Connections,30,1000,ObjectServerへの最大接続数
  Iduc.ListeningPort,0,33201,ObjectServerからAEN/Gatewayへの通信のサーバー側ポート
  PA.Name,NCO_PA,PA\_XXXX,ObjectServerから直接接続されるPAのインターフェース名
  PA.Password,(暗号化して記載）,(暗号化して記載）,暗号化には、$OMNIHOME/bin/nco_g_cryptコマンドを利用
  PA.UserName,ncopa,ObjectServerがPAに接続する際のユーザー名（認証情報）
  Sec.ExternalAuthentication,none,PAM,ユーザー認証方式（AIXユーザーPAM認証）

(c) イベントデータベース列定義(Primary)
"""""""""""""""""""""""""""""""""""""""""""

イベントデータベース(alerts.status）の列（フィールド）をデフォルトから以下の内容で変更を行う。
フィールドは、正側のObjectServerと副側のObjectServerで同じ定義を共有する。

【ObjectServer IBM SO標準カスタマイズ】

.. csv-table::
  :header-rows: 1

	変更内容,フィールド名,データタイプ,データ長,主キー,変更不可,空値不可,フィールド使用用途
	データ長変更,Summary,VarChar,4096,NO,NO,NO,障害メッセージ
	新規追加,TECHostname,VarChar,64,NO,NO,NO,EIF Probe(tec_db_update.sql）により追加されるスロット
	新規追加,TECFQHostname,VarChar,64,NO,NO,NO,EIF Probe(tec_db_update.sql）により追加されるスロット
	新規追加,TECDate,VarChar,64,NO,NO,NO,EIF Probe(tec_db_update.sql）により追加されるスロット
	新規追加,TECRepeatCount,int,-,NO,NO,NO,EIF Probe(tec_db_update.sql）により追加されるスロット
	新規追加,TECStatus,VarChar,64,NO,NO,NO,EIF Probe(tec_db_update.sql）により追加されるスロット
	新規追加,TECServerHandle,VarChar,64,NO,NO,NO,EIF Probe(tec_db_update.sql）により追加されるスロット
	新規追加,TECEventHandle,VarChar,64,NO,NO,NO,EIF Probe(tec_db_update.sql）により追加されるスロット
	新規追加,TECDateReception,VarChar,64,NO,NO,NO,EIF Probe(tec_db_update.sql）により追加されるスロット
	新規追加,LocalTertObj,VarChar,255,NO,NO,NO,SNMP Probe(NcKL-advcorr.sql）により追加されるスロット
	新規追加,LocalObjRelate,int,-,NO,NO,NO,SNMP Probe(NcKL-advcorr.sql）により追加されるスロット
	新規追加,RemoteTertObj,VarChar,255,NO,NO,NO,SNMP Probe(NcKL-advcorr.sql）により追加されるスロット
	新規追加,RemoteObjRelate,int,-,NO,NO,NO,SNMP Probe(NcKL-advcorr.sql）により追加されるスロット
	新規追加,CorrScore,int,-,NO,NO,NO,SNMP Probe(NcKL-advcorr.sql）により追加されるスロット
	新規追加,CauseType,int,-,NO,NO,NO,SNMP Probe(NcKL-advcorr.sql）により追加されるスロット
	新規追加,AdvCorrCauseType,int,-,NO,NO,NO,SNMP Probe(NcKL-advcorr.sql）により追加されるスロット
	新規追加,AdvCorrServerName,VarChar,64,NO,NO,NO,SNMP Probe(NcKL-advcorr.sql）により追加されるスロット
	新規追加,AdvCorrServerSerial,int,-,NO,NO,NO,SNMP Probe(NcKL-advcorr.sql）により追加されるスロット
	新規追加,ApplId,VarChar,32,NO,NO,NO,GSMA_EIF Rules使用のために追加するスロット
	新規追加,MsgId,VarChar,13,NO,NO,NO,GSMA_EIF Rules使用のために追加するスロット
	新規追加,BSM_Identity,VarChar,64,NO,NO,NO,GSMA_EIF Rules ITMイベント受信用(situation_originスロット）
	新規追加,ITMDisplayItem,VarChar,129,NO,NO,NO,GSMA_EIF Rules ITMイベント受信用(situation_displayitemスロット）
	新規追加,ITMStatus,VarChar,1,NO,NO,NO,GSMA_EIF Rules ITMイベント受信用(situation_statusスロット）
	新規追加,ITMTime,VarChar,23,NO,NO,NO,GSMA_EIF Rules ITMイベント受信用(situation_timeスロット）
	新規追加,ITMHostname,VarChar,64,NO,NO,NO,GSMA_EIF Rules ITMイベント受信用(cms_hostnameスロット）
	新規追加,ITMPort,VarChar,16,NO,NO,NO,GSMA_EIF Rules ITMイベント受信用(cms_portスロット）
	新規追加,ITMIntType,VarChar,1,NO,NO,NO,GSMA_EIF Rules ITMイベント受信用(integration_typeスロット）
	新規追加,ITMSitType,VarChar,1,NO,NO,NO,GSMA_EIF Rules ITMイベント受信用(situation_typeスロット）
	新規追加,ITMThruNode,VarChar,64,NO,NO,NO,GSMA_EIF Rules ITMイベント受信用(situation_thrunodeスロット）
	新規追加,ITMResetFlag,VarChar,1,NO,NO,NO,GSMA_EIF Rules ITMイベント受信用(master_reset_flagスロット）
	新規追加,ComponentType,VarChar,32,NO,NO,NO,[CEIM]障害のカテゴリタイプ
	新規追加,Component,VarChar,32,NO,NO,NO,[CEIM]障害のカテゴリ名
	新規追加,SubComponent,VarChar,32,NO,NO,NO,[CEIM]障害のサブカテゴリ名
	新規追加,InstanceId,VarChar,256,NO,NO,NO,[CEIM]障害のインスタンス種別
	新規追加,InstanceValue,VarChar,64,NO,NO,NO,[CEIM]障害のインスタンス値
	新規追加,InstanceSituation,VarChar,128,NO,NO,NO,[CEIM]障害の状況
	新規追加,CustomerCode,VarChar,3,NO,NO,NO,ObjectServerシステム環境識別子
	新規追加,SubAccount,VarChar,32,NO,NO,NO,ObjectServer環境種別名称

【自動化用追加フィールド】

.. csv-table::
  :header-rows: 1

	変更内容,フィールド名,データタイプ,データ長,主キー,変更不可,空値不可,フィールド使用用途
	新規追加,ZProcessState,int,-,NO,NO,NO,イベント自動化処理の状態を管理する
	0:,未処理
	1:,情報付加完了
	2:,自動化処理完了
	3:,自動化処理対象外
	新規追加,ZFwdState,int,-,NO,NO,NO,オペレーターコンソールへの表示状態を管理する
	0:,事前処理前
	1:,事前処理完了（表示可能）
	3:,要監視対応状態（表示中）
	11:,対応完了（イベント重複抑止状態）
	新規追加,ZBkupState,int,-,NO,NO,NO,イベントのロギング状態を管理する
	0:,特になし
	1:,バックアップ待ち
	3:,バックアップ完了
	新規追加,CorrellatedNode,VarChar,64,NO,NO,NO,イベントに関連するホスト名を格納する
	本フィールドもNodeと同じ基準で監視抑止条件となるSAN Switchや
	NW Switchのポートダウンに含まれ、接続されている機器の
	計画停止作業時の監視抑止設定により同時に監視が抑止される
	新規追加,SuppressStart,time,-,NO,NO,NO,非監視イベントの開始時刻を指定するためのフィールド
	新規追加,SuppressEnd,time,-,NO,NO,NO,非監視イベントの最遅終了時刻を指定するためのフィールド
	新規追加,SDMSupportGroup,VarChar,64,NO,NO,NO,イベントの通知先連絡グループ名
	新規追加,SDMAction,VarChar,1024,NO,NO,NO,オペレーターによる一次対応方法
	新規追加,SDM_SupportGroup,VarChar,64,NO,NO,NO,AENポップアップ用一時変数（イベントの通知先連絡グループ名）
	新規追加,SDM_Action,VarChar,1024,NO,NO,NO,AENポップアップ用一時変数（オペレーターによる一次対応方法）
	新規追加,SDMOperaterGroup,VarChar,64,NO,NO,NO,イベント表示先オペレーターグループ名

2.2.2.4. Bi-Gateway設計
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Bi-Gateway設定項目一覧

.. csv-table::
  :header-rows: 1

  項目名,設定ファイル,設定対象,設定ファイルパス,備考
  プロパティ,propsファイル,$OMNIHOME/etc/BIGW_XXX2.props,Bi-Gatewayの稼働設定
  マップファイル,mapファイル,$OMNIHOME/etc/BIGW_XXX2.map,データベース同期マッピング設定
  レプリケーション定義（正）,tblrep.defファイル,$OMNIHOME/etc/BIGW_XXX2.objectservera.tblrep.def,同期テーブル設定（正⇒副）
  レプリケーション定義（副）,tblrep.defファイル,$OMNIHOME/etc/BIGW_XXX2.objectserverb.tblrep.def,同期テーブル設定（副⇒正）

(a) Bi-Gatewayプロパティ
"""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

  設定項目,デフォルト値,設定値,備考
  MessageLevel,warn,デフォルト値
  MessageLog,$OMNIHOME/log/NCO_GATE.log,$OMNIHOME/log/BIGW_XXX2.log
  Name,NCO_GATE,BIGW_XXX2
  PropsFile,$OMNIHOME/etc/NCO_GATE.props,$OMNIHOME/etc/BIGW_XXX2.props
  Gate.MapFile,$OMNIHOME/gates/objserv_bi/objserv_bi.map,$OMNIHOME/etc/BIGW_XXX2.map
	Gate.Mapper.ForwardHistoricJournals,FALSE,TRUE
	Gate.ObjectServerA.Server,NCOMS,NCOMS_XXX1
	Gate.ObjectServerA.TblReplicateDefFile,$OMNIHOME/gates/objserv_bi/,$OMNIHOME/etc/BIGW_XXX2.objectservera.tblrep.def
	objserv_bi.objectservera.tblrep.def
	Gate.ObjectServerB.Server,NCOMS,DENCO_XXX2
	Gate.ObjectServerB.TblReplicateDefFile,$OMNIHOME/gates/objserv_bi/,$OMNIHOME/etc/BIGW_XXX2.objectserverb.tblrep.def
	objserv_bi.objectserverb.tblrep.def
	Gate.Resync.Master,ObjectServerA
	Gate.Resync.Type,NORMAL,UPDATE

(b) Bi-Gatewayマップファイル
"""""""""""""""""""""""""""""""

設定記述凡例::

  CREATE MAPPING <マップ名>
  {
    <同期先フィールド名> = @<同期元フィールド名>
  }

.. csv-table::
  :header-rows: 1

	マップ名,同期先フィールド名,同期元フィールド名,同期先フィールド名,同期元フィールド名,同期先フィールド名,同期元フィールド名
	"StatusMap （追加分のみ）",TECHostname,TECHostname,AdvCorrServerSerial,AdvCorrServerSerial,InstanceId,InstanceId
	TECFQHostname,TECFQHostname,ApplId,ApplId,InstanceValue,InstanceValue
	TECDate,TECDate,MsgId,MsgId,InstanceSituation,InstanceSituation
	TECRepeatCount,TECRepeatCount,BSM_Identity,BSM_Identity,CustomerCode,CustomerCode
	TECStatus,TECStatus,ITMDisplayItem,ITMDisplayItem,SubAccount,SubAccount
	TECServerHandle,TECServerHandle,ITMStatus,ITMStatus,ZProcessState,ZProcessState
	TECEventHandle,TECEventHandle,ITMTime,ITMTime,ZFwdState,ZFwdState
	TECDateReception,TECDateReception,ITMHostname,ITMHostname,ZBkupState,ZBkupState
	LocalTertObj,LocalTertObj,ITMPort,ITMPort,CorrellatedNode,CorrellatedNode
	LocalObjRelate,LocalObjRelate,ITMIntType,ITMIntType,SDMSupportGroup,SDMSupportGroup
	RemoteTertObj,RemoteTertObj,ITMSitType,ITMSitType,SDMAction,SDMAction
	RemoteObjRelate,RemoteObjRelate,ITMThruNode,ITMThruNode,SDM_SupportGroup,SDM_SupportGroup
	CorrScore,CorrScore,ITMResetFlag,ITMResetFlag,SDM_Action,SDM_Action
	CauseType,CauseType,ComponentType,ComponentType,SDMOperaterGroup,SDMOperaterGroup
	AdvCorrCauseType,AdvCorrCauseType,Component,Component
	AdvCorrServerName,AdvCorrServerName,SubComponent,SubComponent
	AdminEventSupMap,Node,Node,Component,Component,SubComponent,SubComponent
	Summary,Summary,eventId,eventId
	AdminFrontMsgMap,Node,Node,Component,Component,SubComponent,SubComponent
	MsgId,MsgId,Summary,Summary,SDMSupportGroup,SDMSupportGroup
	SDMAction,SDMAction
	UserEventSupMap,Node,Node,Component,Component,SubComponent,SubComponent
	Summary,Summary,Cause,Cause,eventId,eventId
	SuppressStart,SuppressStart,SuppressEnd,SuppressEnd

(c) レプリケーション定義（正）
""""""""""""""""""""""""""""""""

設定記述凡例::

  REPLICATE ALL FROM TABLE '<テーブル名>' USING MAP '<マップ名>';

.. csv-table::
  :header-rows: 1

  テーブル名,マップ名,備考
  admin_settings.suppress_event,AdminEventSupMap,恒久監視抑止テーブル
  admin_settings.front_msg,AdminFrontMsgMap,オペレーター対応表示用テーブル
  user_settings.suppress_event,UserEventSupMap,監視抑止機能制御テーブル

(d) レプリケーション定義（副）
""""""""""""""""""""""""""""""""

設定記述凡例::
  REPLICATE ALL FROM TABLE '<テーブル名>' USING MAP '<マップ名>';

.. csv-table::
  :header-rows: 1

  テーブル名,マップ名,備考
  admin_settings.suppress_event,AdminEventSupMap,恒久監視抑止テーブル
  admin_settings.front_msg,AdminFrontMsgMap,オペレーター対応表示用テーブル
  user_settings.suppress_event,UserEventSupMap,監視抑止機能制御テーブル

2.2.2.5. Uni-Gateway設計
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Uni-Gateway設定項目一覧
【サーバーホスト名】

.. csv-table::
  :header-rows: 1

  項目名,設定ファイル,設定対象,設定ファイルパス,備考
  プロパティ,propsファイル,$OMNIHOME/etc/UNIGW_XXX1.props,Uni-Gatewayの稼働設定
  マップファイル,mapファイル,$OMNIHOME/etc/UNIGW_XXX1map,データベース同期マッピング設定
  レプリケーション定義,tblrep.defファイル,$OMNIHOME/etc/UNIGW_XXX1.reader.tblrep.def,同期テーブル設定

【サーバーホスト名】

.. csv-table::
  :header-rows: 1

  項目名,設定ファイル,設定対象,設定ファイルパス,備考
  プロパティ,propsファイル,$OMNIHOME/etc/UNIGW_XXX2.props,Uni-Gatewayの稼働設定
  マップファイル,mapファイル,$OMNIHOME/etc/UNIGW_XXX2map,データベース同期マッピング設定
  レプリケーション定義,tblrep.defファイル,$OMNIHOME/etc/UNIGW_XXX2.reader.tblrep.def,同期テーブル設定

(a) Uni-Gatewayプロパティ
""""""""""""""""""""""""""""

【サーバーホスト名】

.. csv-table::
  :header-rows: 1

	設定項目,デフォルト値,設定値,備考
	MessageLevel,warn,warn,ObjectServerをバックアップとして構成する。
	MessageLog,$OMNIHOME/log/NCO_GATE.log,$OMNIHOME/log/UNIGW_XXX1.log,ゲートウェイログの出力先。
	Name,NCO_GATE,UNI_GATE_name,ゲートウェイ名。
	PropsFile,$OMNIHOME/etc/NCO_GATE.props,$OMNIHOME/etc/UNIGW_XXX1props,ゲートウェイプロパティファイル
	Gate.MapFile,$OMNIHOME/gates/objserv_uni/objserv_uni.map,$OMNIHOME/etc/UNIGW_XXX1.map,ゲートウェイマップファイル
	Gate.Mapper.ForwardHistoricJournals,FALSE,TRUE,ジャーナルエントリの転送。
	Gate.Reader.Server,NCOMS,NCOMS_XXX1,Focal Object Server名
	Gate.Reader.TblReplicateDefFile,$OMNIHOME/gates/objserv_uni/objserv_uni.reader.tblrep.def,$OMNIHOME/etc/UNIGW_XXX1.objectservera.tblrep.def,プライマリObject Serverテーブル複製ファイル
	Gate.Writer.Server,NCOMS,NCOMS_ROMV,共同監視virutal Object Server名
	Gate.Writer.CommonNames,NCOMS,"NCOMS_ROM1,NCOMS_ROM2",共同監視Primary/Secondary Object Server名
	Gate.Writer.Username,root,gsmagateway,共同監視Primary/Secondary Object Serverユーザー名
	Gate.Writer.Password,N/A,DLFBBCGDBBEJGLCEHCAOCE,共同監視Primary/Secondary Object Serverパスワード
	Gate.Resync.Type,NORMAL,NORMAL,イベント同期タイプ

【サーバーホスト名】

.. csv-table::
  :header-rows: 1

	設定項目,デフォルト値,設定値,備考
	MessageLevel,warn,warn,ObjectServerをバックアップとして構成する。
	MessageLog,$OMNIHOME/log/NCO_GATE.log,$OMNIHOME/log/UNIGW_XXX2.log,ゲートウェイログの出力先。
	Name,NCO_GATE,UNI_GATE_name,ゲートウェイ名。
	PropsFile,$OMNIHOME/etc/NCO_GATE.props,$OMNIHOME/etc/UNIGW_XXX2.props,ゲートウェイプロパティファイル
	Gate.MapFile,$OMNIHOME/gates/objserv_uni/objserv_uni.map,$OMNIHOME/etc/UNIGW_XXX2.map,ゲートウェイマップファイル
	Gate.Mapper.ForwardHistoricJournals,FALSE,TRUE,ジャーナルエントリの転送。
	Gate.Reader.Server,NCOMS,NCOMS_XXX2,Focal Object Server名
	Gate.Reader.TblReplicateDefFile,$OMNIHOME/gates/objserv_uni/objserv_uni.reader.tblrep.def,$OMNIHOME/etc/UNIGW_XXX2.objectservera.tblrep.def,プライマリObject Serverテーブル複製ファイル
	Gate.Writer.Server,NCOMS,NCOMS_ROMV,共同監視virutal Object Server名
	Gate.Writer.CommonNames,NCOMS,"NCOMS_ROM1,NCOMS_ROM2",共同監視Primary/Secondary Object Server名
	Gate.Writer.Username,root,gsmagateway,共同監視Primary/Secondary Object Serverユーザー名
	Gate.Writer.Password,N/A,DLFBBCGDBBEJGLCEHCAOCE,共同監視Primary/Secondary Object Serverパスワード
	Gate.Resync.Type,NORMAL,NORMAL,イベント同期タイプ

(b) Uni-Gatewayマップファイル
""""""""""""""""""""""""""""""""

設定記述凡例::

  CREATE MAPPING <マップ名>
  {
    <同期先フィールド名> = \@<同期元フィールド名>
  }

.. csv-table::
  :header-rows: 1

	マップ名,同期先フィールド名,同期元フィールド名,同期先フィールド名,同期元フィールド名,同期先フィールド名,同期元フィールド名,同期先フィールド名,同期元フィールド名
	"StatusMap （追加分のみ）",Identifier,'@Identifier' ON INSERT ONLY,TaskList,'@TaskList',Country,'@Country',SupportOrg,'@SupportOrg'
	Node,'@Node' ON INSERT ONLY,NmosSerial,'@NmosSerial',CustomerCode,'@CustomerCode',TECDate,'@TECDate'
	NodeAlias,'@NodeAlias' ON INSERT ONLY NOTNULL '@Node',NmosObjInst,'@NmosObjInst',HWType,'@HWType',TECFQHostname,'@TECFQHostname'
	Manager,'@Manager' ON INSERT ONLY,NmosCauseType,'@NmosCauseType',IBMManaged,'@IBMManaged',TECRepeatCount,'@TECRepeatCount'
	Agent,'@Agent' ON INSERT ONLY,NmosDomainName,'@NmosDomainName',ITMDisplayItem,'@ITMDisplayItem',TicketGroup,'@TicketGroup'
	AlertGroup,'@AlertGroup' ON INSERT ONLY,NmosEntityId,'@NmosEntityId',ITMEventData,'@ITMEventData',TicketNumber,'@TicketNumber'
	AlertKey,'@AlertKey' ON INSERT ONLY,NmosManagedStatus,'@NmosManagedStatus',ITMHostname,'@ITMHostname',SDMOperatorGroup,'@SDMOperatorGroup'
	Severity,'@Severity',LocalNodeAlias,'@LocalNodeAlias' ON INSERT ONLY,ITMIntType,'@ITMIntType'
	Summary,'@Summary',LocalPriObj,'@LocalPriObj' ON INSERT ONLY,ITMPort,'@ITMPort'
	StateChange,'@StateChange',LocalSecObj,'@LocalSecObj' ON INSERT ONLY,ITMResetFlag,'@ITMResetFlag'
	FirstOccurrence,'@FirstOccurrence' ON INSERT ONLY,LocalRootObj,'@LocalRootObj' ON INSERT ONLY,ITMSitType,'@ITMSitType'
	LastOccurrence,'@LastOccurrence',RemoteNodeAlias,'@RemoteNodeAlias' ON INSERT ONLY,ITMStatus,'@ITMStatus'
	InternalLast,'@InternalLast',RemotePriObj,'@RemotePriObj' ON INSERT ONLY,ITMThruNode,'@ITMThruNode'
	Poll,'@Poll' ON INSERT ONLY,RemoteSecObj,'@RemoteSecObj' ON INSERT ONLY,ITMTime,'@ITMTime'
	Type,'@Type' ON INSERT ONLY,RemoteRootObj,'@RemoteRootObj' ON INSERT ONLY,InstanceId,'@InstanceId'
	Tally,'@Tally',X733EventType,'@X733EventType' ON INSERT ONLY,InstanceSituation,'@InstanceSituation'
	Class,'@Class' ON INSERT ONLY,X733ProbableCause,'@X733ProbableCause' ON INSERT ONLY,InstanceValue,'@InstanceValue'
	Grade,'@Grade' ON INSERT ONLY,X733SpecificProb,'@X733SpecificProb' ON INSERT ONLY,LifecycleState,'@LifecycleState'
	Location,'@Location' ON INSERT ONLY,X733CorrNotif,'@X733CorrNotif' ON INSERT ONLY,MachineType,'@MachineType'
	OwnerUID,'@OwnerUID',URL,'@URL' ON INSERT ONLY,MsgId,'@MsgId'
	OwnerGID,'@OwnerGID',ExtendedAttr,'@ExtendedAttr' ON INSERT ONLY,OSType,'@OSType'
	Acknowledged,'@Acknowledged',ServerName,'@ServerName' ON INSERT ONLY,OutsideServiceHours,'@OutsideServiceHours'
	Flash,'@Flash',ServerSerial,'@ServerSerial' ON INSERT ONLY,ResourceId,'@ResourceId'
	EventId,'@EventId' ON INSERT ONLY,ApplId,'@ApplId',ResourceType,'@ResourceType'
	ExpireTime,'@ExpireTime' ON INSERT ONLY,BSM_Identity,'@BSM_Identity',ResourceUsage,'@ResourceUsage'
	ProcessReq,'@ProcessReq',ChangeNumber,'@ChangeNumber',SrcSyncNeeded,'@SrcSyncNeeded'
	SuppressEscl,'@SuppressEscl',CIClass,'@CIClass',SrcSyncParms,'@SrcSyncParms'
	Customer,'@Customer' ON INSERT ONLY,CIName,'@CIName',SrcSyncSerial,'@SrcSyncSerial'
	Service,'@Service' ON INSERT ONLY,CINameField,'@CINameField',SrcSyncServer,'@SrcSyncServer'
	PhysicalSlot,'@PhysicalSlot' ON INSERT ONLY,ClearedBy,'@ClearedBy',SrcSyncType,'@SrcSyncType'
	PhysicalPort,'@PhysicalPort' ON INSERT ONLY,Component,'@Component',SubAccount,'@SubAccount'
	PhysicalCard,'@PhysicalCard' ON INSERT ONLY,ComponentType,'@ComponentType',SubComponent,'@SubComponent'

(c) レプリケーション定義
""""""""""""""""""""""""""

設定記述凡例::

  REPLICATE ALL FROM TABLE '<テーブル名>' USING MAP '<マップ名>' FILTER WITH '<条件式>' AFTER IDUC DO '<転送後処理>;

.. csv-table::
  :header-rows: 1

  テーブル名,マップ名,条件式,転送後処理,備考
  alerts.status,StatusMap,ZFwdState=1 AND Severity>=1,ZFwdState=3,アラート情報テーブル

2.2.2.6. ProcessAgent設計
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. csv-table::
  :header-rows: 1

  項目名,設定ファイル,設定対象,設定ファイルパス,備考
  設定ファイル(Primary）,confファイル,$OMNIHOME/etc/nco_pa.conf,Process Agentの稼働設定
  設定ファイル(Backup）,confファイル,$OMNIHOME/etc/nco_pa.conf,Process Agentの稼働設定

(a) Primaryサーバー Process Agent設定ファイル
"""""""""""""""""""""""""""""""""""""""""""""""

nco_processセクション（Process Agentのプロセス制御設定）

設定記述凡例::

  nco_process '<プロセス名>'
  {
    Command '<コマンド>' run as <実行UID>
    Host,=,'<ホスト名>'
    Managed,=,<管理設定>
    RestartMsg,=,${NAME} running as ${EUID} has been restored on ${HOST}.
    AlertMsg,=,${NAME} running as ${EUID} has died on ${HOST}.
    RetryCoung,=,<再試行回数>
    ProcessType,=,<プロセスタイプ>
  }

.. csv-table::
  :header-rows: 1

	プロセス名,設定項目,設定値
	MasterObjectServer,コマンド,$OMNIHOME/bin/nco_objserv -name NCOMS_XXX1 -pa PA\_XXXX
	実行UID,0
	ホスト名
	管理設定,True
	再試行回数,0
	プロセスタイプ,PaPA\_AWARE
	Mttrapd,コマンド,"$OMNIHOME/probes/nco_p_mttrapd -propsfile ""$OMNIHOME/probes/props/mttrapd.props"""
	実行UID,0
	ホスト名
	管理設定,True
	再試行回数,0
	プロセスタイプ,PaNOT_PA\_AWARE
	TivoliEifProbe,コマンド,"$OMNIHOME/probes/nco_p_tivoli_eif -propsfile ""$OMNIHOME/probes/props/tivoli_eif.props"""
	実行UID,221
	ホスト名
	管理設定,True
	再試行回数,0
	プロセスタイプ,PaNOT_PA\_AWARE
	SyslogProbe,コマンド,$OMNIHOME/probes/nco_p_syslog -propsfile $OMNIHOME/probes/props/syslog.props
	実行UID,221
	ホスト名
	管理設定,True
	再試行回数,0
	プロセスタイプ,PaNOT_PA\_AWARE

nco_serviceセクション（Process Agentで制御するプロセスの依存関係の設定）

設定記述凡例::

  nco_service '<サービス名>'
  {
    ServiceType,=,<サービスタイプ>
    ServiceStart,=,<起動方法>
    process '<プロセス名1>' NONE
    process '<プロセス名2>' '<前提プロセス名2>'
  }

.. csv-table::
  :header-rows: 1

	サービス名,設定項目,設定値,設定項目,設定値
	Core,サービスタイプ,Master
	起動方法,Auto
	プロセス名1,MasterObjectServer
	プロセス名2,Mttrapd,前提プロセス名2,MasterObjectServer
	プロセス名3,TivoliEifProbe,前提プロセス名3,MasterObjectServer
	プロセス名4,SyslogProbe,前提プロセス名4,MasterObjectServer

nco_securityセクション（他のProcess Agentからのアクセス許可リスト）

設定記述凡例::

  nco_security
  {
    host '<ホスト名1>'
  }

.. csv-table::
  :header-rows: 1

	設定項目,設定値
	ホスト名1
	ホスト名2

nco_routingセクション（他のProcess Agentへのアクセス認証設定）

設定記述凡例::

  nco_routing
  {
    host '<ホスト名1>' '<PA名1>' '<ユーザー名1>' '<パスワード1>'
  }

.. csv-table::
  :header-rows: 1

	設定項目,設定値,設定項目,設定値,設定項目,設定値,設定項目,設定値
	ホスト名1,PA名1,PA\_,ユーザー名1,ncopa,パスワード1,(暗号化して記載)
	ホスト名2,PA名2,PA\_,ユーザー名2,ncopa,パスワード2,(暗号化して記載)
	ホスト名3,PA名3,PA\_,ユーザー名3,ncopa,パスワード3,(暗号化して記載)
	ホスト名4,PA名4,PA\_,ユーザー名4,ncopa,パスワード4,(暗号化して記載)
	ホスト名5,PA名5,PA\_,ユーザー名5,ncopa,パスワード5,(暗号化して記載)
	ホスト名6,PA名6,PA\_,ユーザー名6,ncopa,パスワード6,(暗号化して記載)
	ホスト名7,PA名7,PA\_,ユーザー名7,ncopa,パスワード7,(暗号化して記載)
	ホスト名8,PA名8,PA\_,ユーザー名8,ncopa,パスワード8,(暗号化して記載)
	ホスト名9,PA名9,PA\_,ユーザー名9,ncopa,パスワード9,(暗号化して記載)
	ホスト名10,PA名10,PA\_,ユーザー名10,ncopa,パスワード10,(暗号化して記載)
	ホスト名11,PA名11,PA\_,ユーザー名11,ncopa,パスワード11,(暗号化して記載)
	ホスト名12,PA名12,PA\_,ユーザー名12,ncopa,パスワード12,(暗号化して記載)
	ホスト名13,PA名13,PA\_,ユーザー名13,ncopa,パスワード13,(暗号化して記載)
	ホスト名14,PA名14,PA\_,ユーザー名14,ncopa,パスワード14,(暗号化して記載)
	ホスト名15,PA名15,PA\_,ユーザー名15,ncopa,パスワード15,(暗号化して記載)
	ホスト名16,PA名16,PA\_,ユーザー名16,ncopa,パスワード16,(暗号化して記載)
	ホスト名17,PA名17,PA\_,ユーザー名17,ncopa,パスワード17,(暗号化して記載)
	ホスト名18,PA名18,PA\_,ユーザー名18,ncopa,パスワード18,(暗号化して記載)
	ホスト名19,PA名19,PA\_,ユーザー名19,ncopa,パスワード19,(暗号化して記載)
	ホスト名20,PA名20,PA\_,ユーザー名20,ncopa,パスワード20,(暗号化して記載)
	ホスト名21,PA名21,PA\_,ユーザー名21,ncopa,パスワード21,(暗号化して記載)

(b) Secondaryサーバー Process Agent設定ファイル
"""""""""""""""""""""""""""""""""""""""""""""""""

nco_processセクション（Process Agentのプロセス制御設定）

設定記述凡例::

  nco_process '<プロセス名>'
  {
    Command '<コマンド>' run as <実行UID>
    Host = '<ホスト名>'
    Managed = <管理設定>
    RestartMsg = ${NAME} running as ${EUID} has been restored on ${HOST}.
    AlertMsg = ${NAME} running as ${EUID} has died on ${HOST}.
    RetryCoung = <再試行回数>
    ProcessType = <プロセスタイプ>
  }

.. csv-table::
  :header-rows: 1

	プロセス名,設定項目,設定値
	MasterObjectServer,コマンド,$OMNIHOME/bin/nco_objserv -name DENCO_XXX2 -pa PA\_XXXX
	実行UID,0
	ホスト名
	管理設定,True
	再試行回数,0
	プロセスタイプ,PaPA\_AWARE
	Mttrapd,コマンド,$OMNIHOME/probes/nco_p_mttrapd -propsfile $OMNIHOME/probes/props/mttrapd.props
	実行UID,0
	ホスト名
	管理設定,True
	再試行回数,0
	プロセスタイプ,PaNOT_PA\_AWARE
	TivoliEifProbe,コマンド,$OMNIHOME/probes/nco_p_tivoli_eif -propsfile $OMNIHOME/probes/props/tivoli_eif.props
	実行UID
	ホスト名
	管理設定,True
	再試行回数,0
	プロセスタイプ,PaNOT_PA\_AWARE
	SyslogProbe,コマンド,$OMNIHOME/probes/nco_p_syslog -propsfile $OMNIHOME/probes/props/syslog.props
	実行UID
	ホスト名
	管理設定,True
	再試行回数,0
	プロセスタイプ,PaNOT_PA\_AWARE
	BiGateway,コマンド,$OMNIHOME/bin/nco_g_objserv_bi -name BIGW_UE02 -propsfile $OMNIHOME/etc/BIGW_XXX2
	実行UID
	ホスト名
	管理設定,True
	再試行回数,0
	プロセスタイプ,PaNOT_PA\_AWARE

nco_serviceセクション（Process Agentで制御するプロセスの依存関係の設定）

設定記述凡例::

  nco_service '<サービス名>'
  {
    ServiceType = <サービスタイプ>
    ServiceStart = <起動方法>
    process '<プロセス名1>' NONE
    process '<プロセス名2>' '<前提プロセス名2>'
  }

.. csv-table::
  :header-rows: 1

	サービス名,設定項目,設定値,設定項目,設定値
	Core,サービスタイプ,Master
	起動方法,Auto
	プロセス名1,MasterObjectServer
	プロセス名2,Mttrapd,前提プロセス名2,MasterObjectServer
	プロセス名3,TivoliEifProbe,前提プロセス名3,MasterObjectServer
	プロセス名4,SyslogProbe,前提プロセス名4,MasterObjectServer
	プロセス名5,BiGateway,前提プロセス名5,MasterObjectServer

nco_securityセクション（他のProcess Agentからのアクセス許可リスト）

設定記述凡例::

  nco_security
  {
    host '<ホスト名1>'
  }

.. csv-table::
  :header-rows: 1

	設定項目,設定値
	ホスト名1
	ホスト名2

nco_routingセクション（他のProcess Agentへのアクセス認証設定）

設定記述凡例::

  nco_routing
  {
    host '<ホスト名1>' '<PA名1>' '<ユーザー名1>' '<パスワード1>'
  }

.. csv-table::
  :header-rows: 1

	設定項目,設定値,設定項目,設定値,設定項目,設定値,設定項目,設定値
	ホスト名1,PA名1,PA\_,ユーザー名1,ncopa,パスワード1,(暗号化して記載)
	ホスト名2,PA名2,PA\_,ユーザー名2,ncopa,パスワード2,(暗号化して記載)
	ホスト名3,PA名3,PA\_,ユーザー名3,ncopa,パスワード3,(暗号化して記載)
	ホスト名4,PA名4,PA\_,ユーザー名4,ncopa,パスワード4,(暗号化して記載)
	ホスト名5,PA名5,PA\_,ユーザー名5,ncopa,パスワード5,(暗号化して記載)
	ホスト名6,PA名6,PA\_,ユーザー名6,ncopa,パスワード6,(暗号化して記載)
	ホスト名7,PA名7,PA\_,ユーザー名7,ncopa,パスワード7,(暗号化して記載)
	ホスト名8,PA名8,PA\_,ユーザー名8,ncopa,パスワード8,(暗号化して記載)
	ホスト名9,PA名9,PA\_,ユーザー名9,ncopa,パスワード9,(暗号化して記載)
	ホスト名10,PA名10,PA\_,ユーザー名10,ncopa,パスワード10,(暗号化して記載)
	ホスト名11,PA名11,PA\_,ユーザー名11,ncopa,パスワード11,(暗号化して記載)
	ホスト名12,PA名12,PA\_,ユーザー名12,ncopa,パスワード12,(暗号化して記載)
	ホスト名13,PA名13,PA\_,ユーザー名13,ncopa,パスワード13,(暗号化して記載)
	ホスト名14,PA名14,PA\_,ユーザー名14,ncopa,パスワード14,(暗号化して記載)
	ホスト名15,PA名15,PA\_,ユーザー名15,ncopa,パスワード15,(暗号化して記載)
	ホスト名16,PA名16,PA\_,ユーザー名16,ncopa,パスワード16,(暗号化して記載)
	ホスト名17,PA名17,PA\_,ユーザー名17,ncopa,パスワード17,(暗号化して記載)
	ホスト名18,PA名18,PA\_,ユーザー名18,ncopa,パスワード18,(暗号化して記載)
	ホスト名19,PA名19,PA\_,ユーザー名19,ncopa,パスワード19,(暗号化して記載)
	ホスト名20,PA名20,PA\_,ユーザー名20,ncopa,パスワード20,(暗号化して記載)
	ホスト名21,PA名21,PA\_,ユーザー名21,ncopa,パスワード21,(暗号化して記載)

2.2.2.7. Tivoli EIF Probe設計
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. csv-table::
  :header-rows: 1

	項目名,設定ファイル,設定対象,設定ファイルパス,備考
	プロパティ(Primary) propsファイル,$OMNIHOME/probes/props/tivoli_eif.props,EIF Probeの稼働設定
	ルール(Primary) rulesファイル,$OMNIHOME/probes/GSMA/rules/gsma_jp_eif.rules,イベント処理プログラム
	プロパティ(Backup) propsファイル,$OMNIHOME/probes/props/tivoli_eif.props,EIF Probeの稼働設定
	ルール(Primary) rulesファイル,$OMNIHOME/probes/GSMA/rules/gsma_jp_eif.rules,イベント処理プログラム

(a) Primaryサーバー EIF Probeプロパティ設定
"""""""""""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

	設定項目,デフォルト値,設定値,備考
	Server,'NCOMS','NCOMS_XXX1',Primary接続先ObjectServer名
	ServerBackup,'DENCO_XXX2',Secondary接続先ObjectServer名
	MessageLevel,'warn',デフォルト値,Probeログレベル（デフォルト値）
	MessageLog,'$OMNIHOME/log/tivoli_eif.log',デフォルト値,Probeログ出力先（デフォルト値）
	NetworkTimeout,0,300,サーバーの停止判断秒数
	PollServer,30,0,Probe自動Failback無効
	Inactivity,600,0,不使用時の自動停止無効
	AutoSAF,0,1,再起動時の自動キャッシュ開始
	StoreAndForward,1,2,イベントキャッシュ機能On
	RollSAFInterval,90,デフォルト値,Failover時のイベント再送期間
	portNumber,9999,5529,EIFイベント受信用ポート
	RulesFile,'$OMNIHOME/probes/aix5/tivoli_eif.rules','$OMNIHOME/probes/GSMA/rules/gsma_jp_eif.rules',ルールファイルパス

(b) Primaryサーバー ルール設定
""""""""""""""""""""""""""""""""

ルールファイル構成

.. csv-table::
  :header-rows: 1

	ファイル名,ファイルパス,備考
	基本ルール,$OMNIHOME/probes/GSMA/rules/gsma_jp_eif.rules
	初期処理ルール,$OMNIHOME/probes/GSMA/rules/custom/gsma_custom_init.include,初期処理（配列定義・環境名定義）
	クラス名テーブル,$OMNIHOME/probes/GSMA/rules/CEIM,CEIM（イベントクラス）定義
	特殊クラス処理ルール,$OMNIHOME/probes/GSMA/rules/custom/gsma_custom_unique_class.include,特殊クラス処理
	最終処理ルール,$OMNIHOME/probes/GSMA/rules/custom/gsma_custom_final.include,最終処理ルール（名前解決ルール呼出）
	名前解決ルール,$OMNIHOME/probes/GSMA/rules/custom/NameResolve.include,イベントホスト名名前解決処理

.. note:: 各ルールは別途成果物として管理

(c) Secondaryサーバー EIF Probeプロパティ設定
"""""""""""""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

	設定項目,デフォルト値,設定値,備考
	Server,'NCOMS','NCOMS_XXX1',Primary接続先ObjectServer名
	ServerBackup,'DENCO_XXX2',Secondary接続先ObjectServer名
	MessageLevel,'warn',デフォルト値,Probeログレベル（デフォルト値）
	MessageLog,'$OMNIHOME/log/tivoli_eif.log',デフォルト値,Probeログ出力先（デフォルト値）
	NetworkTimeout,0,300,サーバーの停止判断秒数
	PollServer,0,デフォルト値,Probe自動Failback無効
	Inactivity,600,0,不使用時の自動停止無効
	AutoSAF,0,1,再起動時の自動キャッシュ開始
	StoreAndForward,1,2,イベントキャッシュ機能On
	RollSAFInterval,90,デフォルト値,Failover時のイベント再送期間
	portNumber,9999,5529,EIFイベント受信用ポート
	RulesFile,'$OMNIHOME/probes/aix5/tivoli_eif.rules','$OMNIHOME/probes/GSMA/rules/gsma_jp_eif.rules',ルールファイルパス

(d) Secondaryサーバー ルール設定
""""""""""""""""""""""""""""""""""

ルールファイル構成

.. csv-table::
  :header-rows: 1

	ファイル名,ファイルパス,備考
	基本ルール,$OMNIHOME/probes/GSMA/rules/gsma_jp_eif.rules
	初期処理ルール,$OMNIHOME/probes/GSMA/rules/custom/gsma_custom_init.include,初期処理（配列定義・環境名定義）
	クラス名テーブル,$OMNIHOME/probes/GSMA/rules/CEIM,CEIM（イベントクラス）定義
	特殊クラス処理ルール,$OMNIHOME/probes/GSMA/rules/custom/gsma_custom_unique_class.include,特殊クラス処理
	最終処理ルール,$OMNIHOME/probes/GSMA/rules/custom/gsma_custom_final.include,最終処理ルール（名前解決ルール呼出）
	名前解決ルール,$OMNIHOME/probes/GSMA/rules/custom/NameResolve.include,イベントホスト名名前解決処理

.. note:: 各ルールは別途成果物として管理

2.2.2.8. SNMP Trapd Probe設計
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. csv-table::
  :header-rows: 1

	項目名,設定ファイル,設定対象,設定ファイルパス,備考
	プロパティ(Primary) propsファイル,$OMNIHOME/probes/props/mttrapd.props,SNMP Probeの稼働設定
	ルール(Primary) rulesファイル,$OMNIHOME/probes/NcKL/rules/snmptrap.rules,イベント処理プログラム
	プロパティ(Backup) propsファイル,$OMNIHOME/probes/props/mttrapd.props,SNMP Probeの稼働設定
	ルール(Primary) rulesファイル,$OMNIHOME/probes/NcKL/rules/snmptrap.rules,イベント処理プログラム

(a) Primaryサーバー SNMP Probeプロパティ設定
""""""""""""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

	設定項目,デフォルト値,設定値,備考
	Server,'NCOMS_XXX1','NCOMS_XXX1',Primary接続先ObjectServer名
	ServerBackup,'DENCO_XXX2','DENCO_XXX2',Secondary接続先ObjectServer名
	MessageLevel,'warn',デフォルト値,Probeログレベル（デフォルト値）
	MessageLog,'$OMNIHOME/log/mttrapd.log',デフォルト値,Probeログ出力先（デフォルト値）
	QuietOutput,1,デフォルト値,出力のOID表記は略記しない
	NetworkTimeout,0,300,サーバーの停止判断秒数
	PollServer,0,デフォルト値,Probe自動Failback無効
	AutoSAF,0,1,再起動時の自動キャッシュ開始
	StoreAndForward,1,2,イベントキャッシュ機能On
	RollSAFInterval,90,デフォルト値,Failover時のイベント再送期間
	RulesFile,'$OMNIHOME/probes/aix5/mttapd.rules','$OMNIHOME/probes/NcKL/rules/snmptrap.rules',ルールファイルパス
	PeerHost,'rmntnca2',Probeペアの相手ホスト
	Peerport,9999,39999,Probeペアとの確認ポート
	BeatInterval,2,デフォルト値,Probeペアとの生存確認間隔秒
	Mode,'standard','master',Probeペアとの関係（Primary）

(b) Primaryサーバー ルール設定
""""""""""""""""""""""""""""""""

ルールファイル構成

.. csv-table::
  :header-rows: 1

	ファイル名,ファイルパス,備考
	基本ルール,$OMNIHOME/probes/NcKL/rules/snmptrap.rules
	Generic重大度定義,$OMNIHOME/probes/NcKL/rules/snmptrap.sev.lookup,Genericトラップの重大度設定ファイル
	GenericCEIM定義,$OMNIHOME/probes/NcKL/rules/snmptrap.ceim.lookup,GenericトラップのCEIM設定ファイル
	Specificトラップルール,$OMNIHOME/probes/NcKL/rules/include-snmptrap/<MIBNAME>.rules,Specificトラップのイベント生成ルール
	Specific重大度定義,$OMNIHOME/probes/NcKL/rules/include-snmptrap/<MIBNAME>.sev.lookup,Specificトラップの重大度設定ファイル
	SpecificCEIM定義,$OMNIHOME/probes/NcKL/rules/include-snmptrap/<MIBNAME>.ceim.lookup,SpecificトラップのCEIM設定ファイル

.. note:: 各ルールは別途成果物として管理

(c) Secondaryサーバー SNMP Probeプロパティ設定
""""""""""""""""""""""""""""""""""""""""""""""""

.. csv-table::
  :header-rows: 1

	設定項目,設定値,設定値,備考
	Server,'NCOMS','NCOMS_XXX1',Primary接続先ObjectServer名
	ServerBackup,'DENCO_XXX2',Secondary接続先ObjectServer名
	MessageLevel,'warn',デフォルト値,Probeログレベル（デフォルト値）
	MessageLog,'$OMNIHOME/log/mttrapd.log',デフォルト値,Probeログ出力先（デフォルト値）
	QuietOutput,1,デフォルト値,出力のOID表記は略記しない
	NetworkTimeout,0,300,サーバーの停止判断秒数
	PollServer,0,デフォルト値,Probe自動Failback無効
	AutoSAF,0,1,再起動時の自動キャッシュ開始
	StoreAndForward,1,2,イベントキャッシュ機能On
	RollSAFInterval,90,デフォルト値,Failover時のイベント再送期間
	RulesFile,'$OMNIHOME/probes/aix5/mttrapd.rules','$OMNIHOME/probes/NcKL/rules/snmptrap.rules',ルールファイルパス
	PeerHost,'rmntnca1',Probeペアの相手ホスト
	Peerport,9999,39999,Probeペアとの確認ポート
	BeatInterval,2,デフォルト値,Probeペアとの生存確認間隔秒
	Mode,'standard','slave',Probeペアとの関係（Primary）

(d) Secondaryサーバー ルール設定
""""""""""""""""""""""""""""""""""

ルールファイル構成

.. csv-table::
  :header-rows: 1

	ファイル名,ファイルパス,備考
	基本ルール,$OMNIHOME/probes/NcKL/rules/snmptrap.rules
	Generic重大度定義,$OMNIHOME/probes/NcKL/rules/snmptrap.sev.lookup,Genericトラップの重大度設定ファイル
	GenericCEIM定義,$OMNIHOME/probes/NcKL/rules/snmptrap.ceim.lookup,GenericトラップのCEIM設定ファイル
	Specificトラップルール,$OMNIHOME/probes/NcKL/rules/include-snmptrap/<MIBNAME>.rules,Specificトラップのイベント生成ルール
	Specific重大度定義,$OMNIHOME/probes/NcKL/rules/include-snmptrap/<MIBNAME>.sev.lookup,Specificトラップの重大度設定ファイル
	SpecificCEIM定義,$OMNIHOME/probes/NcKL/rules/include-snmptrap/<MIBNAME>.ceim.lookup,SpecificトラップのCEIM設定ファイル

.. note:: 各ルールは別途成果物として管理
