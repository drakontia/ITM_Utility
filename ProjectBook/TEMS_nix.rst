ITM6.1インストールパラメーター,,,,,,,
,,,,,,,
,OS構成,,,,,,
,設定項目,,設定値,,,,
,コンピューターのホスト名  ,,,,,,
,IP アドレス  ,,,,,,
,,,,,,,
,ITM導入構成,,,,,,
,設定項目,,設定値,デフォルト値,要件シートNo,監視システム設計書,注意点
,導入ディレクトリ,,,/opt/IBM/ITM,,,
,Encryption Key,,,IBMTivoliMonitoringEncryptionKey,,,"32文字の文字列(TEMS,TEPS,TEMA共通)"
,導入モジュール（アプリケーション・サポート・データを追加するエージェント等）,,,,,,
,,,,,,,
,,,,,,,
,TEMS構成,,,,,,
,設定項目,,設定値,デフォルト値,要件シートNo,監視システム設計書,注意点,, 
,TEMS Type,,"□Hub
□Remote","□Hub
□Remote",,"3.2 管理サーバー構成
3.5.1 コンポーネント配置",,,
,agent connect through a firewall,,"□Yes
□No","□Yes
□No",,,,, 
,TEMS Name,,,,,,,, 
,Protocol,,"Protocol 1 : IP.PIPE
Protocol 2 :
Protocol 3 :","□IP.PIPE
□IP
□SNA",,3.5.3 通信設計,,,
,KDC_PARTITION,,,,,,NAT環境の場合設定,,
,Primary Network Interface(NIC インターフェース名)  ,,,,,3.5.3 通信設計,複数NICがある場合、TEMSが使用するNICのIPラベルを設定,,
,Configure Hot Standby TEMS,,□,,,,,,
,Hot Standby Hubホスト名orIP,,,,,,,,
,Hot Standby Hubポート,,,1918,,,,,
,TEMS Location,,"□on this computer
□on a different computer","□on this computer
□on a different computer",,,,,
,Connection must pass through firewall,,□　,,,,,,
,TEMSポート,,,1918,,3.5.3 通信設計,,,
,,,,,,,,,
,,,,,,,,,
,,,,,,,,,
,,
