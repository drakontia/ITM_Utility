ITM6.1インストールパラメーター,,,,,,,
,,,,,,,
,OS構成,,,,,,
,設定項目,,設定値,,,,
,コンピューターのホスト名  ,,,,,,
,IP アドレス  ,,,,,,
,,,,,,,
,ITM導入構成,,,,,,
,設定項目,,設定値,デフォルト値,要件シートNo,監視システム設計書,注意点
,導入ディレクトリ,,,C:\IBM\ITM,,,
,Encryption Key,,,IBMTivoliMonitoringEncryptionKey,,,"32文字の文字列(TEMS,TEPS,TEMA共通)"
,導入モジュール（アプリケーション・サポート・データを追加するエージェント等）,,,,,,
,Agent Deployment（デプロイメント・デポに追加するエージェント ）,,,,,,
,ProgramFolder（プログラム・フォルダーの名前）,,,ITM Tivoli Monitoring,,,
？,SetupType,,,,,,
,,,,,,,,,
,,,,,,,,,
,TEMS構成,,,,,,,,
,設定項目,,設定値,デフォルト値/選択,要件シートNo,監視システム設計書,注意点,, 
,TEMS Type,,"□Hub
□Remote",,,"3.2 管理サーバー構成
3.5.1 コンポーネント配置",,,
,TEC Event Integration Facility,,□　,,,4.3.1 TECとのイベント連携,TEC連携を行う場合に選択します,, 
,Disable Workflow Policy/Tivoli Emitter Agent Event Forwarding,,□　,,,,,,
,TEMS Name,,,,,,,, 
,Protocol,,"Protocol 1 : IP.PIPE
Protocol 2 :
Protocol 3 :","□IP.PIPE
□IP.UDP
□IP.SPIPE
□SNA",,3.5.3 通信設計,,,
,Configure Hot Standby TEMS,,□,,,"3.2 管理サーバー構成
3.5.1 コンポーネント配置",Hot Standbyを構成しない場合は、チェックしません。また、構成する場合でも、HUB TEMS導入時には行なわず、後で設定変更により構成を行います,,
,Hot Standby Hubホスト名orIP,,,,,,,,
,Hot Standby Hubポート,,,1918,,,,,
,TECServerLocation,,,,,,TECサーバーのホスト名を指定します,,
,TECPortNumber,,,5529,,3.5.3 通信設計,TECサーバーの受信ポートにあわせます.。TECサーバーがUNIXでポートマッパーが稼動している場合、0を指定します。,,
,TEMS Location,,"□on this computer
□on a different computer","□on this computer
□on a different computer",,,,,
,Connection must pass through firewall,,□　,,,3.5.3 通信設計,,,
,Protocol,,"Protocol 1 : IP.PIPE
Protocol 2 :
Protocol 3 :","□IP.PIPE
□IP.UDP
□IP.SPIPE
□SNA",,3.5.3 通信設計,
,TEMS名(ホスト名orIP),,,,,,
,TEMSポート,,,1918,,3.5.3 通信設計,
,,,,,,,
,Lang Support適用,,,,,,
,設定項目,,設定値,デフォルト値,要件シートNo,監視システム設計書,注意点
,ITM6.1導入ディレクトリ,,C:\IBM\ITM,C:\IBM\ITM,,,
,LangPack,,□Japanese Language Pack,□Japanese Language Pack,,,
,,,,,,,
,,,,,,,
,,,,,,,
