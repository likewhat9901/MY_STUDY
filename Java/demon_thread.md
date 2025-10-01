## Demon Thread 생성 및 활용
``` java
import java.util.concurrent.TimeUnit;

public class Main {
    public static void main(String[] args) {
        // 1. 주기적인 작업을 수행할 Runnable 객체 정의
        Runnable autoSaveTask = () -> {
            try {
                while (true) {
                    System.out.println("자동 저장 작업을 수행합니다.");
                    // 2. 5초 대기
                    TimeUnit.SECONDS.sleep(5); 
                }
            } catch (InterruptedException e) {
                // 스레드가 중단될 때 예외 처리
                Thread.currentThread().interrupt();
                System.out.println("자동 저장 스레드가 중단되었습니다.");
            }
        };

        // 3. 스레드 생성 및 데몬으로 설정
        Thread daemonThread = new Thread(autoSaveTask);
        daemonThread.setDaemon(true); 
        
        // 4. 스레드 시작
        daemonThread.start();
        
        System.out.println("메인 스레드 시작");

        // 메인 스레드가 20초 후 종료되도록 함
        try {
            TimeUnit.SECONDS.sleep(20);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        
        System.out.println("메인 스레드 종료");
    }
}
```
