ITM6.1インストールパラメーター,,,,,,
,,,,,,
,ITM導入構成（Windows),,,,,
,設定項目,設定値,デフォルト値,要件シートNo,監視システム設計書,注意点
,導入ディレクトリ,C:\IBM\ITM,C:\IBM\ITM,,,
,Encryption Key,IBMTivoliMonitoringEncryptionKey,IBMTivoliMonitoringEncryptionKey,,,"32文字の文字列(TEMS,TEPS,TEMA共通)"
,導入モジュール,,,,,
,ProgramFolder,ITM Tivoli Monitoring,ITM Tivoli Monitoring,,,
,SetupType,,,,,
,,,,,,
,Windows OS Agent構成,,,,,
,設定項目,設定値,デフォルト値,要件シートNo,監視システム設計書,注意点
,Connection must pass through firewall,□　,□　,,,
,Protocol,"Protocol 1 : IP.PIPE
Protocol 2 :
Protocol 3 :","□IP.PIPE
□IP.UDP
□IP.SPIPE
□SNA",,,
,TEMS(ホスト名orIP),,,,,
,TEMSポート,,1918,,3.5.3 通信設計,


,ITM導入構成（Linux),,,,,
,設定項目,設定値（デフォルト値）,設定値（デフォルト値）,要件シートNo,監視システム設計書,注意点
,導入ディレクトリ,,/opt/IBM/ITM,,,
,Encryption Key,,IBMTivoliMonitoringEncryptionKey,,,"32文字の文字列(TEMS,TEPS,TEMA共通)"
,Product packages,,,,,
,,,,,,
,Linux OS Agent構成,,,,,
,設定項目,設定値（デフォルト値）,設定値（デフォルト値）,要件シートNo,監視システム設計書,注意点
,TEMS(ホスト名orIP),,,,,
,Protocol,"Protocol 1 : IP.PIPE
Protocol 2 :
Protocol 3 :","□IP.PIPE
□IP
□IP.SPIPE
□SNA",,,
,TEMSポート,,1918,,3.5.3 通信設計,
,KDC_PARTITION,,,,,NAT越通信を行う場合、マッピングを指定します
,secondary TEMS,,,,,
,Optional Primary Network Name,,,,,複数NICがある場合、Agentが使用するNICのIPラベルを設定します


,ITM導入構成（UNIX）,,,,,
,設定項目,設定値（デフォルト値）,設定値（デフォルト値）,要件シートNo,監視システム設計書,注意点
,導入ディレクトリ,,/opt/IBM/ITM,,,
,Encryption Key,,IBMTivoliMonitoringEncryptionKey,,,"32文字の文字列(TEMS,TEPS,TEMA共通)"
,Product packages,,,,,
,,,,,,
,UNIX OS Agent構成,,,,,
,設定項目,設定値（デフォルト値）,設定値（デフォルト値）,要件シートNo,監視システム設計書,注意点
,TEMS(ホスト名orIP),,,,,
,Protocol,"Protocol 1 : IP.PIPE
Protocol 2 :
Protocol 3 :","□IP.PIPE
□IP
□IP.SPIPE
□SNA",,,
,TEMSポート,,1918,,3.5.3 通信設計,
,KDC_PARTITION,,,,,NAT越通信を行う場合、マッピングを指定します
,secondary TEMS,,,,,
,Optional Primary Network Name,,,,,複数NICがある場合、Agentが使用するNICのIPラベルを設定します
