# CloudFormation-Test
CloudFormation sample template.  
CloudFormationテンプレートです。無料利用枠の範囲内で作成を行います。  
構成は、
VPC内に配置した1台のALB、2台のEC2、1台のRDS（MySQL）となります。

## Diagram
![CloudFormation_Diagram drawio](https://user-images.githubusercontent.com/91016271/152478149-0341d384-cc36-4d75-af98-0b05cfe5ac37.jpg)

## Contents
### CFntestVPC.yml
VPC、インターネットゲートウェイ、ルートテーブル、サブネットを作成します。
### CFntestSG.yml
セキュリティグループを作成します。内容は、EC2用、RDS用、ALB用の3つです。
RDSのセキュリティグループは、EC2からのトラフィックのみ受け付けます。
### CFntestEC2_IAM.yml
EC2用のIAMロールと2台のEC2インスタンスを作成します。  
AMIはSSMのパブリックパラメータから、常に最新のLinux2を取得します。  
UserDataはMySQLをインストールする内容です。
### CFntestRDS.yml
RDS for MySQLを作成します。  
シングル構成のため、障害時は自動バックアップから復旧を行います。
### CFntestELB.yml
ALBを作成します。  
ターゲットは、'CFntestEC2_IAM.yml'で作成した2台のEC2です。
### CloudFormation_ServiceRole.json
CloudFormationテンプレートではなく、IAMポリシーのテンプレートです。
スタック作成時にユーザーの権限がない場合に使用します。  
**使い方**  
① 当ポリシーと"PowerUserAccess"の2つからなるCloudFormation用のサービスロールを作成します。  
② スタック作成時にユーザーの権限がない場合、サービスロールを付与します。  
ユーザーに余分な権限を与えることなく、CloudFormationの作成が可能になります。  
**参考**  
[【AWS】Cloudformationとサービスロール](https://yoshinori-satoh.com/blog/2019-01-19-cloudformation_servicerole/#%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%83%AD%E3%83%BC%E3%83%AB%E4%BD%9C%E6%88%90%E6%89%8B%E9%A0%86)
