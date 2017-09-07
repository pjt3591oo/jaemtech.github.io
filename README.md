# jaemtech.github.io
Jaem 기술 블로그
주소 : http://jaemtech.github.io/
# 블로그 포스팅 방법

#### 1. Posting 파일 경로 생성

1. ```git clone https://github.com/JaemTech/jaemtech.github.io.git``` 후 포스팅 카테고리를 폴더로 생성
  
  > ```mkdir aws``` 후 kinesis.md 파일 aws 폴더 내에서 작성
  
2. github에서 `upload file`로 직접 경로 설정 후 작성 해도 됩니다. 

#### 2. index.md 에 링크 경로 설정
  
  해당 파일 경로를 카테고리 안에 추가 합니다. '['//포스팅 글 제목']''('//링크경로')' 로 추가 해주시면 됩니다.  

  > ```[kinesis로 로그 수집기 만들기] (https://jaemtech.github.io/aws/kinesis)``` 추가 
  
#### 3. Disqus 댓글 창 달기

포스팅 md 파일 하단에 아래 코드를 삽입합니다
```javascript
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
                            
```
