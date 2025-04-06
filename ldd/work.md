# Work

 - timer, hrtimer  
최근 SPI를 사용하는 장치를 브링업하면서 슬레이브 장치에서 지원하는 SPI frame format이 표준을 따르지 않아서  
직접 GPIO로 SPI 파형을 만드는 App을 구현했었다. msec단위로 sleep을 하게되서 성능상 좋지 않은 점도 있고  
KHz까지만 구현이 가능하다는 단점도 있어서 성능 개선 작업을 구상하고 있다.  
Application runtime에 빈번하게 SPI통신을 해야하는데, 멀티 스레딩 환경에서는 스케줄링에 의해 영향을 받을 수도 있다는 점도 있다.  
그래서 SPI통신 파형을 만드는 부분을 Kernel Space로 옮기면, 타이머와 인터럽트를 활용하면 되고, 인터럽트는 실행 권한이 높기 때문에 스케줄링에 영향을 받지 않는다.  
(커널 드라이버 단에서도 gpio-spi.c라는 드라이버가 존재하지만, sleep함수를 사용하고 있다.)  
timer로 먼저 작성해보고, hrtimer로 구현을 해보려고 한다. hrtimer는 nanosec까지 지원이 되서, MHz까지 클럭을 만들 수 있을 것으로 보인다.  
다른 업무를 하면서 틈틈히 진행해야하는 일이어서 유용한 자료들을 미리 검색해봤다.  
   - timer, hrtimer에 대한 전반적인 개념 이해(블로그 자료)  
    [Timer-high-resolution-timer](https://yohda.tistory.com/entry/%EB%A6%AC%EB%88%85%EC%8A%A4-%EC%BB%A4%EB%84%90-Timer-high-resolution-timer)  
    [BBB hrtimer](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=moony6463&logNo=10187067717)  
    [timer hrtimer관련 간단한 자료](https://velog.io/@quine/%ED%83%80%EC%9D%B4%EB%A8%B8-%EB%93%9C%EB%9D%BC%EC%9D%B4%EB%B2%84timer-hrtimer)  
   - hrtimer 관련 자료  
    [hrtimer stackoverflow](https://stackoverflow.com/questions/54777424/using-hrtimers-in-the-linux-kernel)  
    [hrtimer 커널 공식 문서](https://docs.kernel.org/timers/hrtimers.html)  
    [hrtimer example](https://embetronicx.com/tutorials/linux/device-drivers/using-high-resolution-timer-in-linux-device-driver/)  