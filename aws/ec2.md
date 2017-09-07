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

![img3](https://github.com/JaemTech/jaemtech.github.io/blob/master/img/ec2/3.png?raw=true)

> 인스턴스의 자세한 설정에서 Subnet이 기존의 서버에 맞는 Subnet ID인지 확인 하기 바랍니다. 

![img4](https://github.com/JaemTech/jaemtech.github.io/blob/master/img/ec2/4.png?raw=true)

> Storage 추가는 사이즈를 늘릴 일이 없다면 그대로 둡니다

![img5](https://github.com/JaemTech/jaemtech.github.io/blob/master/img/ec2/5.png?raw=true)

> 생성시 ssh Permission key는 기존에 공통으로 쓰는 키가 존재한다면 그것을 이용 하세요

![img6](https://github.com/JaemTech/jaemtech.github.io/blob/master/img/ec2/9.png?raw=true)

### 3. Security Group Inbound Rule 설정 하기 

> Security Group의 Inbound는 방화벽을 통과하게 허용하는 IP 주소와 Port 번호를 지정합니다. 

> API 서버의 경우 외부적으로 서버를 공개하지 않아도 될때 VPC 내 Subnet IP 만 허용하는 것을 추천합니다. 

> 외부적으로 공개해야 하는 Application 서버의 경우는 해당하는 허용 IP를 모두 허용해야 하므로 실행한 서버의 Port만 제한해서 사용해야 합니다. 

> 공통적으로 ssh 접속을 위한 22번 포트는 회사내 IP 대역과 주로 작업하는 IP대역만 추가하는 것을 권장 합니다 

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://jaemtech.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
                            