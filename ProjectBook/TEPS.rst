ITM6.1インストールパラメーター,,,,,,,

,DB2構成,,,,,,
,設定項目,,設定値,デフォルト値,要件シートNo,監視システム設計書,注意点
,インストール･タイプ,,"□標準
□コンパクト
□カスタム","□標準
□コンパクト
□カスタム",,,通常は「標準」で導入します
,インストール・アクション,,"□このコンピューターにインストールする
□設定を応答ファイルに保管する","□このコンピューターにインストールする
□設定を応答ファイルに保管する",,,「このコンピューターにインストールする」を選択します
,導入ティレクトリ,,C:\IBM\SQLLIB,C:\IBM\SQLLIB,,,
,DB2 Adimnistrator ユーザー名,,db2admin,db2admin,,,DB2管理者名
,DB2 Adimnistrator ユーザーパスワード,,*******,*******,,,
,管理連絡先リスト,,"□ロケーション(ローカル）
□ロケーション(リモート)
□SMTP通知を使用可能にする","□ロケーション(ローカル）
□ロケーション(リモート)
□SMTP通知を使用可能にする",,,連絡先がない場合には、「ロケーション(ローカル) 」を選択します
,DB2インスタンス,,DB2 ,DB2 ,,,
,DB2ツール・カタログ,,"□DB2ツールカタログをこのコンピューターに準備する
□DB2ツールカタログを準備しない","□DB2ツールカタログをこのコンピューターに準備する
□DB2ツールカタログを準備しない",,,必要がなければ「DB2ツールカタログを準備しない」を選択します
,ヘルス・モニター通知の連絡先,,"□新規連絡先
□タスクをインストールの完了まで保留する","□新規連絡先
□タスクをインストールの完了まで保留する",,,ヘルス･モニターを使用しない場合は、「タスクをインストールの完了まで保留する」を選択します

,,,,,,,
,ITM導入構成,,,,,,
,設定項目,,設定値,デフォルト値,要件シートNo,監視システム設計書,注意点
,導入ディレクトリ,,,C:\IBM\ITM,,,
,Encryption Key,,,IBMTivoliMonitoringEncryptionKey,,,"32文字の文字列(TEMS,TEPS,TEMA共通)"
,導入モジュール,,□Tivoli Enterpirse Portal Server,,,,
,Agent Deployment,,,,,,
,ProgramFolder（プログラム・フォルダーの名前）,,,ITM Tivoli Monitoring,,,
,SetupType,,,,,,
,,,,,,,
,TEPS構成,,,,,,
,設定項目,,設定値,設定値,要件シートNo,監視システム設計書,注意点
,TEPSホスト名,,,,,,
,TEPS Data Source Name ,,,TEPS,,,
,TEPS DB Administrator名,,,db2admin,,,DB2管理者名
,TEPS DB Administratorパスワード,,,*******,,,
,DB User ID,,,TEPS,,,TEPS DBアクセス用ユーザーID
,DB User パスワード,,,*******,,,
,DatawarehouseのUser ID,,,ITMUser,,,TDWアクセス用のユーザーID
,Datawarehouseのパスワード,,,********,,,
,Connection must pass through firewall,,□　,,,3.5.3 通信設計,Firewall越管理がある場合に設定
,Protocol,,"Protocol 1 : IP.PIPE
Protocol 2 :
Protocol 3 :","□IP.PIPE
□IP.UDP
□IP.SPIPE
□SNA",,3.5.3 通信設計,通常はIP.PIPEを選択します。SSL通信を行う場合、IP.SPIPEを選択します。(GSKITの構成が必要)
,TEMS(ホスト名orIP),,,,,,IP.PIPE接続用
,TEMSポート,,,1918,,3.5.3 通信設計,
,,,,,,,
