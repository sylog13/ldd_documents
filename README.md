# ldd_documents
Documents about ldd, or linux kernel or background knowledges.


## ldd
 - lwn net Linux Device Drivers:  
[Linux Device Drivers 3rd Edition](https://lwn.net/Kernel/LDD3/)

 - basic knowledges about linux device driver:    
[basic knowledges](https://tldp.org/LDP/lkmpg/2.6/html/lkmpg.html#AEN121)

 - A Dive Into Kbuild:  
[Kbuild system](https://events19.linuxfoundation.org/wp-content/uploads/2017/11/A-Dive-into-Kbuild-Cao-Jin-Fujitsu.pdf)

 - USB
 [USB Debugging Tips](https://elinux.org/images/1/17/USB_Debugging_and_Profiling_Techniques.pdf)

 - Device Tree
[Device Tree Usage](https://elinux.org/Device_Tree_Usage)

 - timer, hrtimer
최근 SPI를 사용하는 장치를 브링업하면서 슬레이브 장치에서 지원하는 SPI frame format이 표준을 따르지 않아서  
직접 GPIO로 SPI 파형을 만들었는데, msec단위로 sleep을 하게되서 성능상 좋지 않은 점이 있는 것 같다. 그리고 KHz까지만 구현이 가능하다는 단점도 있다.  
Application 동작 시에도 자주 SPI통신을 해야하는데, 멀티 스레딩 환경에서는 스케줄링에 의해 영향을 받을 수도 있다.  
그래서 SPI통신 파형을 만드는 부분을 Kernel Space로 옮기면, 타이머와 인터럽트를 활용하면 되고,  
인터럽트는 실행 권한이 높기 때문에 스케줄링에 영향을 받지 않는다.  
(커널 드라이버 단에서도 gpio-spi.c라는 드라이버가 존재하지만, sleep함수를 사용하고 있다.)  
timer로 먼저 작성해보고, hrtimer로 구현을 해보려고 한다.  
hrtimer는 nanosec까지 지원이 되서, MHz까지 클럭을 만들 수 있을 것으로 보인다.  
다른 업무를 하면서 틈틈히 진행해야하는 일이어서 유용한 자료들을 검색해봤다.  
timer, hrtimer에 대한 전반적인 개념 이해  
[](https://yohda.tistory.com/entry/%EB%A6%AC%EB%88%85%EC%8A%A4-%EC%BB%A4%EB%84%90-Timer-high-resolution-timer)
[](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=moony6463&logNo=10187067717)
[](https://velog.io/@quine/%ED%83%80%EC%9D%B4%EB%A8%B8-%EB%93%9C%EB%9D%BC%EC%9D%B4%EB%B2%84timer-hrtimer)
hrtimer 관련 자료  
[](https://stackoverflow.com/questions/54777424/using-hrtimers-in-the-linux-kernel)
[](https://docs.kernel.org/timers/hrtimers.html)
timer 관련 자료  
[](https://embetronicx.com/tutorials/linux/device-drivers/using-high-resolution-timer-in-linux-device-driver/)
  

# Embedded systems
yocto or device protocol or init framework..

## u-boot
### u-boot uImage source file format  
최근에 FPGA와 연결된 ARM시스템을 개발하면서 u-boot에서 flash memory partition내의 image 를 파싱하는 부분까지 따라가보게 됬다.  
(디버깅 지옥이었다...)  
chip vendor마다 다르지만 내가 사용 중인 Agilex5 HPS는 u-boot부팅 마지막 단에서 boot.scr.uimg파티션을 읽고 fpga 디자인을 load하는 부분이 존재한다.  
boot.scr.uimg는 u-boot script인데, 이 스크립트에서는 loadm명령을 실행하게 된다.  
https://www.rocketboards.org/foswiki/Documentation/SingleImageBoot

이런 바이너리 데이터를 읽는데 사용하는 인터페이스로 FIT 인터페이스를 사용한다.  
이와 관련된 공식문서는 아래와 같다.  
https://docs.u-boot.org/en/v2023.07.02/usage/fit/howto.html


## yocto
yocto하면 빌드하고 이미지만 만들면(명령만 치면) 되는 걸로 사람들도 있지만,  
정상적인 개발을 하는 사람들이라면, 알아야하는 배경지식들이 있다.  
application을 빌드하기 위한 레시피 작성법이라든가,  
Library를 만들고 레시피를 작성하고 Application과 의존성을 정의한다든가,  
kernel, u-boot을 매번 git url을 통해 fetch, compile하지 않고 회사용 레이어에 코드가 디렉토리로 추가되게한다든가.  
칩 제조사의 레이어를 상속과 유사한 방법으로 이용해서 회사 제품(보드)만을 위한 레이어를 직접 작성한다든가.  
이런 실무에서 자주 쓰이는 use case들이 있고, 진입장벽이 꽤 높은 편이다.  
udemy강의도 보고 amazon에서 책도 구매해서 보고 해봤는데, 아래 자료도 꽤 좋은 내용인 것 같다.  
인간의 뇌는 종이처럼 평면적인 구조가 아니기 때문에, 다양한 자료를 보면서 입체적으로 이해를 넓혀가는 것이 중요한 것 같다.  
https://bootlin.com/doc/training/yocto/yocto-slides.pdf


