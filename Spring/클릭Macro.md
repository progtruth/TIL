## 온라인서비스 자동 버튼클릭 Macro

- 시작하고 5초 후부터 2초 간격으로 버튼 클릭만 수행
- 종료기능 없음. (직접 서비스 접속해서 2초안에 종료해야함)
- `sleep(1000)` : 1초동안 대기

```java
import java.awt.Robot;
import java.awt.event.InputEvent;

public class Macro {

    public static void main(String[] args) throws InterruptedException {
        Thread.sleep(5000);
        while (true) {
            clickMouse(0, 0);
        }
    }

    public static void clickMouse(int mx, int my) {
        try {
          Robot robot = new Robot();
          
          Thread.sleep(2000); // 2초간격
          
          robot.mousePress(InputEvent.BUTTON1);

          Thread.sleep(100);

          robot.mouseRelease(InputEvent.BUTTON1);
          
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
 
```

