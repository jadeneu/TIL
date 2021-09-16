Polling과 Interrrupt는 프로세서(CPU)와 입출력 장치(I/O device) 사이의 통신이다.

<br>


## ✅ Polling
polling은 하드웨어의 변화를 지속적으로 읽어 들이며 이벤트의 수행 여부를 주기적으로<br>
검사하여 해당 신호를 받았을때 이벤트를 실행하는 방식이다.

## ✅ Interrupt
Interrupt는 하드웨어의 변화를 감지하여 외부로부터의 입력을 CPU가 알아채는 방법이다.<br>
인터럽트는 이벤트를 수행하라는 IRQ를 받으면, 인터럽트 핸들러를 통해 ISR이 동작하게 된다.

<br>

## 
polling은 진짜 하염없이 기다리는 거고, interrupt는 IRQ라는 메시지를 준 후에 오는 친구를<br>
기다리는 거라고 볼 수 있다.<br>
폴링 방식은 CPU가 직접 일을 하기 때문에 입출력 처리 시간이 오래 걸린다.
##

<br><br>








# References
* https://kkhipp.tistory.com/155
* https://velog.io/@chy0428/polling%EA%B3%BC-interrupt%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90

<br><br><br>

