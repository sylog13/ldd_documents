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



## Embedded systems
yocto or device protocol or init framework..

## u-boot
u-boot uImage source file format  
최근에 FPGA와 연결된 ARM시스템을 개발하면서 u-boot에서 flash memory partition내의 image 를 파싱하는 부분까지 따라가보게 됬다.  
(디버깅 지옥이었다...)  
chip vendor마다 다르지만 내가 사용 중인 Agilex5 HPS는 u-boot부팅 마지막 단에서 fpga load를 수행하거나 boot.scr.uimg파티션을 읽는 부분이 존재한다.  
fit_xxx 이라는 함수에서 했던 것 같은데, 정확하지는 않다.  
이때 사용하는 인터페이스가 FIT 인터페이스를 사용한다.  
u-boot에서는 image를 읽어들이는 인터페이스라고 하는데 공식 문서에 관련된 설명이 있다.  
읽어두면 다른 칩 개발 시에도 도움이 될 듯 하다.  
https://github.com/lentinj/u-boot/blob/master/doc/uImage.FIT/source_file_format.txt


