# u-boot
## FIT, u-boot command  
최근에 FPGA와 연결된 ARM시스템을 개발하면서 u-boot에서 flash memory partition내의 image 를 파싱하는 부분까지 따라가보게 됬다.  
(디버깅 지옥이었다...)  
chip vendor마다 다르지만 내가 사용 중인 Agilex5 HPS는 u-boot부팅 마지막 단에서 boot.scr.uimg파티션을 읽고 fpga 디자인을 load하는 부분이 존재한다.  
boot.scr.uimg는 u-boot script인데, 이 스크립트에서는 loadm명령을 실행하게 된다.  
https://www.rocketboards.org/foswiki/Documentation/SingleImageBoot

이런 바이너리 데이터를 읽는데 사용하는 인터페이스로 FIT 인터페이스를 사용한다.  
이와 관련된 공식문서는 아래와 같다.  
https://docs.u-boot.org/en/v2023.07.02/usage/fit/howto.html