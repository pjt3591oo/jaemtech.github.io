# EC2 생성 가이드 

## 목차

- EC2란?
- EC2 생성 하기
- Security Group Inbound Rule 설정 하기 

### 1. EC란?
    
> AWS 에서 제공하는 서버 인스턴스 입니다. 레드햇 계열과 데비안 계열의 리눅스와 윈도우 서버까지 제공 합니다. 기본적으로 EC2 생성에는 컴퓨팅에 필요한 코어와 주 메모리     의 사이즈를 조절 할 수 있습니다. EBS라는 스토리지에 연결해 File System을 제공 받습니다. 생성시엔 ssh Permission key를 생성하거나 설정 하는데 이 키를 이용해     접근 가능 합니다. 자세한 사항은 아래 참고 사이트를 참조 하시기 바랍니다 
    
> Ref : [https://aws.amazon.com/ko/documentation/ec2/](https://aws.amazon.com/ko/documentation/ec2/)

### 2. EC2 생성 하기

> 먼저 EC를 생성하기 위해 `Launch Instance` 버튼을 누릅니다 

![img1](https://github.com/JaemTech/jaemtech.github.io/blob/master/img/ec2/1.png?raw=true)

> 원하는 AMI를 선택 합니다. 만약 기존의 백업한 AMI가 있다면 `My AMIs`를 선택하세요

![img2](https://github.com/JaemTech/jaemtech.github.io/blob/master/img/ec2/2.png?raw=true)

> 원하는 Instance Type을 선택 하세요. Free tier는 t2.micro 입니다 

![img3](https://github.com/JaemTech/jaemtech.github.io/blob/master/img/ec3/3.png?raw=true)
