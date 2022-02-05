# CloudFormation-Test
CloudFormation sample template.  
CloudFormationテンプレートです。無料利用枠の範囲内で作成を行います。  
構成は、VPC内に配置した1台のALB、2台のEC2、1台のRDS（MySQL）となります。  
番号は、スタック作成時の適用順です。

## Diagram
![CFntest_Diagram remake2](https://user-images.githubusercontent.com/91016271/152636647-99cd0f76-ddc3-4733-acf1-2af5b444cf7b.png)

## Contents
### ① network/CFntestVPC.yml
VPC、インターネットゲートウェイ、ルートテーブル、サブネットを作成します。
### ② network/CFntestSG.yml
セキュリティグループを作成します。内容は、EC2用、RDS用、ALB用の3つです。  
RDSのセキュリティグループは、EC2からのトラフィックのみ受け付けます。
### ③ iam/CFntestIAM.yml
EC2用のIAMロールを作成します。  
S3,cloudwatch,ssm,RDSのポリシーを付与します。
### ④ compute/CFntestEC2.yml
2台のEC2インスタンスを作成します。  
AMIはSSMのパブリックパラメータから、常に最新のLinux2を取得します。  
UserDataはMySQLをインストールする内容です。
### ⑤ compute/CFntestRDS.yml
RDS for MySQLを作成します。  
シングル構成のため、障害時は自動バックアップから復旧を行います。
### ⑥ compute/CFntestELB.yml
ALBを作成します。  
ターゲットは、'CFntestEC2_IAM.yml'で作成した2台のEC2です。
### IAMPolicy_Template/CloudFormation_ServiceRole.json
CloudFormationテンプレートではなく、IAMポリシーのテンプレートです。  
スタック作成時にユーザーの権限がない場合に使用します。  
**使い方**  
① 当ポリシーと"PowerUserAccess"の2つからなるCloudFormation用のサービスロールを作成します。  
② スタック作成時にユーザーの権限がない場合、サービスロールを付与します。  
ユーザーに余分な権限を与えることなく、CloudFormationの作成が可能になります。  
**参考**  
[【AWS】Cloudformationとサービスロール](https://yoshinori-satoh.com/blog/2019-01-19-cloudformation_servicerole/#%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%83%AD%E3%83%BC%E3%83%AB%E4%BD%9C%E6%88%90%E6%89%8B%E9%A0%86)
