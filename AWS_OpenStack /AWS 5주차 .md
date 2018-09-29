
# AWS 

### 블루프린트를 사용하여 가상 서버 시작하기 

### *장점*

1. AWS에 인프라를 설명하는 일관된 방법이다. 
2. 종속성을 처리할 수 있다.
3. 복제할 수 있다. 
4. 커스터마이징 할 수 있다. 
5. 테스트 할 수 있다. 
6. 업데이트 할 수 있다. 
7. 인간의 실수를 최소화 한다. 
8. 인프라에 대한 문서가 된다. 
9. 공짜다. 

### AWS에 인프라를 설명하는 일관된 방법이다. 
* 인프라 구축에 스크립트를 사용하면 모든 사람이 문제를 
	다르게 해결한다. 
* 따라서 새로운 개발자와 운영자는 기존 코드가 무엇을 <br>하는지에 대해 시간을 소비함 
* CloudFormation 템플릿은 정의하는 명확한 언어이다. 

### 종속성을 처리할 수 있다.
* 복잡한 인프라를 스크립트로 설정하지 말것. 
* 종속성의 지옥에서 벗어나지 못할 것이다. 

### 복제할 수 있다.
* 테스트 환경과 운영환경이 동일하다면 복제할 수 있다.
* CloudFormation을 사용하여 동일한 인프라를 생성하고 동기화를 유지할 수 있다. 

### 커스터마이징 할 수 있다.
* CloudFormation에 사용자 정의 매개변수를 삽입하는 형태로 템플릿을 원하는대로 변경할 수 있다. 

### 테스트 할 수 있다.
* 템플릿에서 인프라를 만들 수 있기만 하면 인프라를 테스트 하고, 다시 종료하면 된다. 

### 업데이트 할 수 있다. 
* CloudFormation은 업데이트 지원을 통해 템플릿의 변경된 부분을 파악하고 인프라에 가능한 부드럽게 변경사항을 적용한다. 

### 인프라에 대한 문서가 된다. 
* CloudFormation 템플릿은 JSON 형태다.
* 코드로 취급할 수 있다. 
* 버전관리 프로그램을 사용할 수 있다. 

## CloudFormation 템플릿 해부하기 
1. Format Version

	* 최신 템플릿 포맷 버전은 2010-09-09이며 현재 유일한 유효 값입니다.
		* JSON 
	
		```
		"AWSTemplateFormatVersion" : "2010-09-09"
		```
		* YAML
	
		```
		AWSTemplateFormatVersion: "2010-09-09"	
		```
2. Description 

	* 무엇에 대한 탬플릿인지에 대한 설명입니다. 
		* JSON
	
		```
  	  "Description" : 
  		  	"Here are some details about the template."
  	  ```
		* YAML 
	
		```
  		  Description: >
  				Here are some
  				details about
  				the template.
   	 ```
    
3. Parameters
	* 파라미터를 통해 스택을 생성하거나 업데이트 할 때마다 템플릿에 사용자 지정 값을 입력할 수 있습니다. 

	* JSON

		```
		"Parameters" : {
  "InstanceTypeParameter" : {
    "Type" : "String",
    "Default" : "t2.micro",
    "AllowedValues" : ["t2.micro", "m1.small", "m1.large"],
    "Description" : "Enter t2.micro, m1.small, or m1.large. Default is t2.micro."
 		 }
}
		```
	* YAML

		```
		Parameters: 
  			InstanceTypeParameter: 
    			Type: String
   				Default: t2.micro
    			AllowedValues: 
      				- t2.micro
      				- m1.small
      				- m1.large
    			Description: Enter t2.micro, m1.small, or m1.large. Default is t2.micro.
		```
		
4. Resources
	* 필수 Resources 섹션은 Amazon EC2 인스턴스 또는 Amazon S3 버킷 등 스택에 포함할 AWS 리소스를 선언합니다. 
	* 자원은 여러분이 기술할 수 있는 가장 작은 블록이다. 가상 서버, 로드 밸런서 일랙스틱 IP주소 등을 예로 들 수 있습니다.

 	* JSON	
 	
 		```
 		"Resources" : {
    "Logical ID" : {
        "Type" : "Resource type",
        "Properties" : {
            Set of properties
       		 }
  		  }
}
 		``` 
 	* YAML
 	
 		```
 		Resources:
  			Logical ID:
    			Type: Resource type
   				Properties:
      				Set of properties

 		```
 	* Logical ID 
 		> 논리적 ID는 영숫자(A-Za-z0-9)여야 하며 템플릿 내에서 고유해야 합니다. 논리적 이름을 사용하여 템플릿의 다른 부분에 있는 리소스를 참조합니다. 예를 들어 Amazon Elastic Block Store 볼륨을 Amazon EC2 인스턴스로 매핑하려는 경우 논리적 ID를 참조하여 인스턴스와 블록 스토어를 연결할 수 있습니다.
논리적 ID 외에도, 특정 리소스에는 EC2 인스턴스 ID 또는 S3 버킷 이름 같은 해당 리소스에 대해 실제 할당된 이름인 물리적 ID도 지정됩니다. 물리적 ID를 사용하여 AWS CloudFormation 템플릿 외부에 있는 리소스를 식별할 수 있지만, 리소스가 생성된 후에만 가능합니다. 예를 들면, MyEC2Instance의 논리적 ID를 EC2 인스턴스 리소스에 지정할 수 있지만, AWS CloudFormation에서 인스턴스 생성 시 AWS CloudFormation은 물리적 ID(예: i-28f9ba55)를 자동으로 생성하여 인스턴스에 할당합니다. 이 물리적 ID를 사용하면 Amazon EC2 콘솔에서 인스턴스를 식별하고 인스턴스의 속성(예: DNS 이름)을 볼 수 있습니다. 사용자 지정 이름을 지원하는 리소스의 경우, 고유한 이름(물리적 ID)를 할당하면 리소스를 보다 신속하게 식별할 수 있습니다. 예를 들면 로그를 저장하는 S3 버킷에 MyPerformanceLogs 이름을 지정할 수 있습니다. 자세한 내용은 이름 유형 단원을 참조하십시오.

	* 예제 
		* 다음 예제에서는 리소스 선언을 보여줍니다. 이 예제에서는 두 가지 리소스를 정의합니다. MyInstance 리소스는 MyQueue 리소스를 UserData 속성의 일부로서 포함합니다.

	* JSON 

		```
		 "Resources" : {
    		"MyInstance" : {
        		"Type" : "AWS::EC2::Instance",
        		"Properties" : {
            		"UserData" : {
                		"Fn::Base64" : {
                    		"Fn::Join" : [ "", [ "Queue=", { "Ref" : "MyQueue" } ] ]
                 } },
            "AvailabilityZone" : "us-east-1a",
            "ImageId" : "ami-20b65349"
        		}
    		},
    
    		"MyQueue" : {
        		"Type" : "AWS::SQS::Queue",
        		"Properties" : {
        		}
   		    }
		}     

		```

	* YAML

		```
		Resources: 
  			MyInstance: 
    			Type: "AWS::EC2::Instance"
    			Properties: 
      			UserData: 
        			"Fn::Base64":
         			 !Sub |
            			Queue=${MyQueue}
      			AvailabilityZone: "us-east-1a"
      			ImageId: "ami-20b65349"
  MyQueue: 
    		Type: "AWS::SQS::Queue"
    		Properties: {}
		
		```  

5. Outputs 
	* 선택적 Outputs 섹션은 다른 스택으로 가져오거나(교차 스택 참조를 생성하기 위해), 응답으로 반환하거나(스택 호출을 설명하기 위해), 또는 AWS CloudFormation 콘솔에서 볼 수 있는 출력 값을 선언합니다. 
	* 출력은 매개변수와 일치할 것 같지만 그렇지 않습니다. 출력은 EC2 서버의 공용 이름과 같이 템플릿에서 뭔가를 반환합니다. 
 		
	* JSON
	 
		```
		"Outputs" : {
  			"Logical ID" : {
    			"Description" : "Information about the value",
   				 "Value" : "Value to return",
    			 "Export" : {
     				 "Name" : "Value to export"
    			}
  			}
}
		``` 
	* YMAL 

		```
		Outputs:
  			Logical ID:
    			Description: Information about the value
    			Value: Value to return
    			Export:
      				Name: Value to export
		```
		
		
## 첫 템플릿 작성하기 
	
> 개발팀에 가상서버를 제공하려고 요청받고 몇달 뒤 개발팀에 더 많은 CPU용량이 필요하다고 가정하자. <br>
> CLI와 SDK를 사용할 수도 있지만 이것을 사용하면 인스턴스를 중지하고 난 다음에 다시 실행해야 한다. <br> 
> CloudFormation을 사용하여 InstanceType 속성을 변경하고 템플릿을 업데이트하면 간단히 실행할 수 있다.



* 다음 예제를 작성해 보자. (<- 이것은 설명입니다)
 
```
{
	"AWSTemplateFormatVersion": "2010-09-09",
	"Description": "AWS in Action: chapter 4",
	"Parameters": {
		"KeyName": { <- 어떤 키를 사용할지 정의한다. 
			"Description": "Key Pair name",
			"Type": "AWS::EC2::KeyPair::KeyName",
			"Default": "mykey"
		},
		"VPC": { <- 가상 사설 클라우드 
			"Description": "Just select the one and only default VPC",
			"Type": "AWS::EC2::VPC::Id"
		},
		"Subnet": {
			"Description": "Just select one of the available subnets",
			"Type": "AWS::EC2::Subnet::Id"
		},
		"InstanceType": { <- 인스턴스 유형을 정의한다. 
			"Description": "Select one of the possible instance types",
			"Type": "String",
			"Default": "t2.micro",
			"AllowedValues": ["t2.micro", "t2.small", "t2.medium"]
		}
	},
	"Mappings": {
		"EC2RegionMap": {
			"ap-northeast-1": {"AmazonLinuxAMIHVMEBSBacked64bit": "ami-cbf90ecb"},
			"ap-southeast-1": {"AmazonLinuxAMIHVMEBSBacked64bit": "ami-68d8e93a"},
			"ap-southeast-2": {"AmazonLinuxAMIHVMEBSBacked64bit": "ami-fd9cecc7"},
			"eu-central-1": {"AmazonLinuxAMIHVMEBSBacked64bit": "ami-a8221fb5"},
			"eu-west-1": {"AmazonLinuxAMIHVMEBSBacked64bit": "ami-a10897d6"},
			"sa-east-1": {"AmazonLinuxAMIHVMEBSBacked64bit": "ami-b52890a8"},
			"us-east-1": {"AmazonLinuxAMIHVMEBSBacked64bit": "ami-1ecae776"},
			"us-west-1": {"AmazonLinuxAMIHVMEBSBacked64bit": "ami-d114f295"},
			"us-west-2": {"AmazonLinuxAMIHVMEBSBacked64bit": "ami-e7527ed7"}
		}
	},
	"Resources": {
		"SecurityGroup": {
			"Type": "AWS::EC2::SecurityGroup",
			"Properties": {
				"GroupDescription": "My security group",
				"VpcId": {"Ref": "VPC"},
				"SecurityGroupIngress": [{
					"CidrIp": "0.0.0.0/0",
					"FromPort": 22,
					"IpProtocol": "tcp",
					"ToPort": 22
				}]
			}
		},
		"Server": { <- 최소한의 EC2 인스턴스를 정의한다. 
			"Type": "AWS::EC2::Instance",
			"Properties": {
				"ImageId": {"Fn::FindInMap": ["EC2RegionMap", {"Ref": "AWS::Region"},
						     "AmazonLinuxAMIHVMEBSBacked64bit"]},
				"InstanceType": {"Ref": "InstanceType"},
				"KeyName": {"Ref": "KeyName"},
				"SecurityGroupIds": [{"Ref": "SecurityGroup"}],
				"SubnetId": {"Ref": "Subnet"}
			}
		}
	},
	"Outputs": { <- EC2 인스턴스의 공개 이름을 반환한다. 
		"PublicName": {
			"Value": {"Fn::GetAtt": ["Server", "PublicDnsName"]},
			"Description": "Public name (connect via SSH as user ec2-user)"
		}
	}
}
```