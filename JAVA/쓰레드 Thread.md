## 1. 프로세스와 쓰레드

- 프로세스: 실행 중인 프로그램. 실행하면 OS로부터 실행에 필요한 자원(메모리)를 할당받아 프로세스가 된다.

<br>

프로세스는 프로그램을 수행하는 데 필요한 데이터와 메모리 드의 자원 그리고 쓰레드로 구성되어 있으며 프로세스의 자원을 이용해서 실제로 수행하는 것이 `쓰레드`

<br>

모든 프로세스에는 최소한 하나 이상의 쓰레드가 존재하며, 둘 이상의 쓰레드를 가진 프로세스를 `멜티쓰레드 포르세스(multi-threaded process)` 라고 한다.

<br>

![picture](https://github.com/JaeYeon33/TIL/blob/main/JAVA/image/Untitled%2025.png?raw=true)

<br>

하나의 프로세스가 가질 수 있는 쓰레드의 개수는 제한되어 있지 않으나 쓰레드가 작업을 수행하는데 개별적인 메모리 공간(호출스택)을 필요로 하기 때문에 프로세스의 메모리 한계에 따라 생성할 수 있는 쓰레드의 수가 결정된다. 실제로는 프로세스의 메모리 한계에 다다를 정도로 많은 쓰레드를 생성하는 일은 없을 것이니 걱정하지 않아도 된다.

<br>

<br>

### 멀티태스킹과 멀티쓰레딩

현재 사용하고 있는 윈도우나 유닉스를 포함한 대부분의 OS는 멀티태스킹(multi-tasking, 다중작업)을 지원하기 떄문에 여러 개의 프로세스가 동시에 실행될 수 있다.

<br>

마찬가지로 멀티쓰레딩은 하나의 프로세스 내에서 여러 쓰레드가 동시에 작업을 수행하는 것.

CPU의 코어가 한 번에 단 하나의 작업만 수행할 수 있으므로, 실제로 동시에 처리되는 작업의 개수는 코어의 개수와 일치한다.

<br>

But, 처리해야하는 쓰레드의 수는 언제나 코어의 개수보다 훨씬 많기 때문에 각 코어가 아주 짧은 시간 동안 여러 작업을 번갈아 가며 수행함으로써 여러 작업들이 모두 동시에 수행되는 것처럼 보이게 한다.

<br>

그래서 프로레스의 성능이 단순히 쓰레드의 개수에 비례하는 것은 아니며, 하나의 쓰레드를 가진 프로세스 보다 두 개의 쓰레드를 가진 프로세스가 오히려 더 낮은 성능을 보일 수도 있다.

<br>

<br>

### 멀티쓰레딩의 장단점

**장점**

1. CPU의 사용률을 향상시킨다.
2. 자원을 보다 효율적으로 사용할 수 있다.
3. 사용자에 대한 응답성이 향상된다.
4. 작업이 분리되어 코드가 간결해진다.

> 쓰레드를 가변 프로세스, 즉 경량 프로세스(LWP, light-weight process)라고 부르기도 한다.
> 

<br>

**단점**

멀티쓰레드 프로세스는 여러 쓰레드가 같은 프로세스 내에서 자원을 공유하면서 작업을 하기 때문에 발생할 수 있는 동기화(synchronization), 교착상태(deadlock)와 같은 문제들을 고려해서 프로그래밍 해야 한다.

<br>

<br>

<br>

## 2. 쓰레드의 구현과 실행

**쓰레드를 구현하는 방법**

1. Thread클래스를 상속
2. Runnable인터페이스를 구현
    - 재사용성(reusability)이 높고 코드의 일관성(consistency)을 유지할 수 있기 때문에 객체지향적인 방법

<br>

Thread를 상속받으면 다른 클래스를 상속받을 수 없기 때문에 Runnable인터페이스를 구현하는 방법이 일반적

```java
// 1. Thread를 상속
class MyThread extends Thread {
		public void run() { /* 작업 내용 */ } // Thread클래스의 run()을 오버라이딩
}
```

```java
// 2. Runnable인터페이스를 구현
class MyThread implements Runnable {
		public void run() { /* 작업 내용 */ } // Runnable인터페이스의 run()을 구현
}
```

```java
class ThreadEx1 {
    public static void main(String[] args) {
        ThreadEx1_1 t1 = new ThreadEx1_1();

        ThreadEx1_2 r = new ThreadEx1_2();
        Thread t2 = new Thread(r);  // 생성자 Thead(Runnable taret)

        t1.start();
        t2.start();
    }
}

class ThreadEx1_1 extends Thread {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(getName());  // 조상인 Thread의 getname()을 호출
        }
    }
}

class ThreadEx1_2 implements Runnable {
    public void run() {
        for (int i = 0; i < 5; i++) {
            // Thread.currentThread() - 현재 실행중인 Thread를 반환
            System.out.println(Thread.currentThread().getName());
        }
    }
}
```

<br>

쓰레드의 이름은 생성자나 메서드를 통해서 지정 또는 변경할 수 있다.

```java
Thread(Runnable target, String name)
Thread(String name)
void setName(String name)
```

<br>

<br>

### 쓰레드의 실행 - start()

쓰레드를 생성했다고 자동으로 실행되는 것은 아니다.

```java
t1.start(); // 쓰레드 t1을 실행
t2.start(); // 쓰레드 t2를 실행
```

<br>

`start()` 가 호출되었다고 실행되는 것이 아니다. 일단 실행대기 상태에 있다가 자신의 차례가 되어야 실행된다. 실행대기중인 쓰레드가 하나도 없으면 곧바로 실행상태가 된다.

<br>

한 번 실행이 종료된 쓰레드는 다시 실행할 수 없다. 즉, 하나의 쓰레드에 대해 `start()` 가 한 번만 호출될 수 있다는 뜻.

<br>

그래서 쓰레드의 작업을 한 번 더 수행해야 한다면 새로운 쓰데르르 생성한 후에 `start()` 를 호출.

하나의 쓰레드에 대해 `start()` 를 두 번 이상 호출하면 `IllegalThradStateException` 이 발생.

```java
Thread1_1 t1 = new ThreadEx1_1();
t1.start();
t1.start(); // 예외발생
```

```java
ThreadEx1_1 t1 = new ThreadEx1_1();
t1.start();
t1 = new ThreadEx1_1(); // 다시 생성
t1.start(); // OK
```

<br>

<br>

<br>

## 3. start()와 run()

<img src="https://github.com/JaeYeon33/TIL/blob/main/JAVA/image/Untitled%2026.png?raw=true" alt="picture" style="zoom:67%;" />

<br>

main()메서드에서 run()을 호출하는 것은 생성된 쓰레드를 실행시키는 것이 아니라 단순히 클래스에 선언된 메서드를 호출하는 것.

<br>

- start(): 새로운 쓰레드가 작업을 실행하는데 필요한 호출스택(call stack)을 생성한 다음에 run()을 호출해서, 생성된 호출스택에 run()이 첫 번째로 올라가게 한다.

<br>

모든 쓰레드는 독립적인 작업을 수행하기 위해 자신만의 호출스택을 필요로 하기 때문에, 새로운 쓰레드를 생성하고 실행시킬 때마다 새로운 호출스택이 생성되고 쓰레드가 종료되면 작업에 사용된 호출스택은 소멸된다.

<br>

![picture](https://github.com/JaeYeon33/TIL/blob/main/JAVA/image/Untitled%2027.png?raw=true)

1. main메서드에서 쓰레드의 start()를 호출
2. start()는 새로운 쓰레드를 생성, 쓰레드가 작업하는데 사용될 호출스택을 생성
3. 새로 생성된 호출스택에 run()이 호출되어, 쓰레드가 독립된 공간에서 작업을 수행
4. 이제는 호출스택이 2개이므로 스케줄러가 정한 순서에 의해서 번갈아 가면서 실행

<br>

<br>

### main쓰레드

main메서드의 작업을 수행하는 것도 쓰레드이며, 이를 main쓰레드라고 한다.

프로그램을 실행하면 기본적으로 하나의 쓰레드를 생성하고, 그 쓰레드가 main메서드를 호출해서 작업이 수행되도록 하는 것

> 실행 중인 사용자 쓰레드가 하나도 없을 때 프로그램을 종료된다.
> 

<br>

<br>

<br>

## 4. 싱글쓰레드와 멀티쓰레드

![picture](https://github.com/JaeYeon33/TIL/blob/main/JAVA/image/Untitled%2028.png?raw=true)

<br>

두 개의 쓰레드로 작업한 시간이 싱글쓰레드로 작업한 시간보다 더 걸린다.

→ 쓰레드간의 작업전환(context switching)에 시간이 걸리기 때문.

<br>

> 프로세스 또는 쓰레드 간의 작업 전환을 ‘컨텍스트 스위칭(context switching)’이라고 한다.
> 

싱글 코어에서 단순히 CPU만을 사용하는 계산작업이라면 오히려 멀티쓰레드보다 싱글쓰레드로 프로그래밍하는 것이 더 효율적.

<br>

<br>

<br>

## 5. 쓰레드의 우선순위

쓰레드는 우선순위(priority)라는 속성(멤버변수)을 가지고 있다. 이 우선순위의 값에 따라 쓰데르가 얻는 실행시간이 달라진다. 쓰레드가 수행하는 작업의 중요도에 따라 쓰레드의 우선순위를 서로 다르게 지정하여 특정 쓰레드가 더 많은 작업시간을 갖도록 할 수 있다.

<br>

<br>

### 쓰레드의 우선순위 정하기

```java
void setPriority(int newPrioirty) // 쓰레드의 우선순위를 지정한 값으로 변경한다.
int  getPriority                  // 쓰레드의 우선순위를 반환한다.

public static final int MAX_PRIORITY  = 10 // 최대우선순위
public static final int MIN_PRIORITY  = 1  // 최소우선순위
public static final int NORM_PRIORITY = 5  // 보통우선순위
```

<br>

쓰레드가 가질 수 있는 우선순위 범위는 1~10이며 숫자가 높을수록 우선순위가 높다.

쓰레드의 우선순위는 쓰레드를 생성한 쓰레드로부터 상속받는다는 것이다. main메서드를 수행하는 쓰레드는 우선순위가 5이므로 main메서드내에서 생성하는 쓰레드의 우선순위는 자동적으로 5가 된다.

<br>

![picture](https://github.com/JaeYeon33/TIL/blob/main/JAVA/image/Untitled%2029.png?raw=true)

<br>

멀티코어라 해도 OS마다 다른 방식으로 스케쥴링하기 때문에, 어떤 OS에서 실행하느냐에 따라 다른 결과를 얻을 수 있다. 자바는 쓰레드가 우선순위에 따라 어떻게 다르게 처리되어야 하는지에 대해 강제하지 않으므로 쓰레드의 우선순위와 관련된 구현이 JVM마다 다를 수 있기 때문이다.

<br>

쓰레드에 우선순위를 부여하는 대신 작업에 우선순위를 두어 `PriorityQueue` 에 저장해 놓고, 우선순위가 높은 작업이 먼저 처리되도록 하는 것이 나을 수 있다.

<br>

<br>

<br>

## 6. 쓰레드 그룹(Thread Group)

쓰레드 그룹은 서로 관련된 쓰레드를 그룹으로 다루기 위한 것. 폴더를 생성해서 관련된 파일드을 함께 넣어서 관리하는 것처럼 쓰레드 그룹을 생성해서 쓰레드를 그룹으로 묶어서 관리할 수 있다.

<br>

폴더 안에 폴더를 생성할 수 있듯이 쓰레드 그룹에 다른 쓰레드 그룹을 포함 시킬 수 있다.

쓰레드 그룹은 보안상의 이유로 도입된 개념으로, 자신이 속한 쓰레드 그룹이나 하위 쓰레드 그룹은 변경할 수 있지만 다른 쓰레드 그룹의 쓰레드를 변경할 수는 없다.

<br>

<br>

### ThreadGroup의 생성자와 메서드

| 생성자 / 메서드 | 설명 |
| --- | --- |
| ThreadGroup(String name) | 지정된 이름의 새로운 쓰레드 그룹을 생성. |
| ThreadGroup(ThreadGroup parent, String name) | 지정된 쓰레드 그룹에 포함되는 새로운 쓰레드 그룹을 생성. |
| int activeCount() | 쓰레드 그룹에 포함된 활성상태에 있는 쓰레드의 수를 반환. |
| int activeGroupCount() | 쓰레드 그룹에 포함된 활성상태에 있는 쓰레드 그룹의 수를 반환. |
| void checkAccess() | 현재 실행중인 쓰레드가 쓰레드 그룹을 변경할 권한이 있는지 체크. 만일 권한이 없다면 SecurityException을 발생. |
| void destroy() | 쓰레드 그룹과 하위 쓰레드 그룹까지 모두 삭제한다. 단, 쓰레드 그룹이나 하위 쓰레드 그룹이 비어있어야 한다. |
|int enumerate(Thread[] list)<br />int enumerate(Thread[] list, boolean recurse)<br />int enumerate(ThreadGroup[] list, boolean recurse)| 쓰레드 그룹에 속한 쓰레드 또는 하위 쓰레드 그룹의 목록을 지정된 배열에 담고 그 개수를 반환.<br />두 번째 매개변수인 recurse의 값을 true로 하면 쓰레드 그룹에 속한 하위 쓰레드 그룹에 쓰르데 그룹까지 배열에 담는다. |
| int getMaxPriority() | 쓰레드 그룹의 최대우선순위를 반환 |
| String getName() | 쓰레드 그룹의 이름을 반환 |
| ThreadGroup getParent() | 쓰레드 그룹의 상위 쓰레드그룹을 반환 |
| void interrupt() | 쓰레드 그룹에 속한 모든 쓰레드그룹을 반환 |
| boolean isDaemon() | 쓰레드 그룹이 데몬 쓰레드그룹인지 확인 |
| boolean isDestroyed() | 쓰레드 그룹이 삭제되었는지 확인 |
| void list() | 쓰레드 그룹에 속한 쓰레드와 하위 쓰레드그룹에 대한 정보를 출력 |
| boolean parentOf(ThreadGroup g) | 지정된 쓰레드 그룹의 상위 쓰레드 그룹인지 확인 |
| void setDaemon(boolean deamon) | 쓰레드 그룹을 데몬 쓰레드그룹으로 설정/해제 |
| void setMaxPriority(int pri) | 쓰레드 그룹의 최대우선순위를 설정 |

<br>

<br>

쓰레드를 쓰레드 그룹에 포함시키려면 Thread생성자를 이용.

```java
Thread(ThreadGroup group, String name)
Thread(ThreadGroup group, Runnable target)
Thread(ThreadGroup group, Runnable target, String name)
Thread(ThreadGroup group, Runnable target, String name, long stackSize)
```

<br>

<br>

자바 어플리케이션이 실행되면, JVM은 main과 system이라는 쓰레드 그룹을 만들고 JVM운영에 필요한 쓰레드들을 생성해서 이 쓰레드 그룹에 포함시킨다. 

```java
ThreadGroup getThreadGroup()                  // 쓰레드 자신이 속한 쓰레드 그룹을 반환
void uncaughtException(Thread t, Throwable e) // 쓰레드 그룹의 쓰레드가 처리되지 않은 예외에 의해
// 실행이 종료되었을 때, JVM에 이 메서드가 자동적으로 호출된다.
```

<br>

```java
new Thread(grp1, r, "th1").sgtart();
```

```java
Thread th1 = new Thread(grp1, r, "th1");
th1.start();
```

<br>

쓰레드 그룹을 지정하지 않은 쓰레드는 자동적으로 자동적으로 main쓰레드 그룹에 속하게 된다.

<br>

<br>

<br>

## 7. 데몬 쓰레드(daemon thread)

데몬 쓰레드는 다른 일반 쓰레드의 작업을 돕는 보조적인 역할을 수행하는 쓰레드이다.

일반 쓰레드가 모두 종료되면 데몬 쓰레드는 강제적으로 자동 종료된다.

→ 데몬 쓰레드는 일반 쓰레드의 보조역할을 수행하므로 일반 쓰레드가 모두 종료되고 나면 데몬 쓰레드의 존재의 의미가 없기 때문.

```java
boolean isDaemon()         // 쓰레드가 데몬인지 확인한다.
                           // 데몬 쓰레드이면 true를 반환.
void setDaemon(boolean on) // 쓰레드를 데몬 쓰레드로 또는 사용자 쓰레드로 변경.
                           // 매개변수 on의 값을 true로 지정하면 데몬 쓰레드가 된다.
```

<br>

<br>

<br>

## 8. 쓰레드의 실행제어

### 쓰레드의 스케쥴링과 관련된 메서드

| 메서드 | 설명 |
| --- | --- |
| static void sleep(long millis)
static void sleep(long millis, int nanos) | 지정되 ㄴ시간(천분의 일초 단위)동안 쓰레드를 일시정지시킨다. 지정한 시간이 지나고 나면, 자동적으로 다시 실행대기상태가 된다. |
| void join()
void join(long millis)
void join(long millis, int nanos) | 지정된 시간동안 쓰레드가 실행되도록 한다. 지정된 시간이 지나거나 작업이 종료되면 join()을 호출한 쓰레드로 다시 돌아와 실행을 계속한다. |
| void interrupt() | sleep()이나 join()에 의해 일시정지상태인 쓰레드를 꺠워서 실행대기 상태로 만든다. 해당 쓰레드에서는 interruptedException이 발생함으로써 일시정지상태를 벗어나게 한다. |
| void stop() | 쓰레드를 즉시 종료시킨다. |
| void suspend() | 쓰레드를 일시정지시킨다. resume()을 호출하면 다시 실행대기상태가 된다. |
| vois resume() | suspend()에 의해 일시정지상태에 있는 쓰레드를 실행대기상태로 만든다. |
| static void yield() | 실행 중에 자신에게 주어진 실행시간을 다른 쓰레드에게 양보(yield)하고 자신은 실행대기상태가 된다. |

<br>

<br>

> resume(), stop(), suspend()는 쓰레드를 교착상태(dead-lock)로 만들기 쉽기 때문에 deprecated되었다.

<br>

<br>

### 쓰레드의 상태

| 상태 | 설명 |
| --- | --- |
| NEW | 쓰레드가 생성되고 아직 start()가 호출되지 않은 상태 |
| RUNNABLE | 실행 중 또는 실행 가능한 상태 |
| BLOCKED | 동기화블럭에 의해서 일시정지된 상태(lock이 풀릴 때까지 기다리는 상태) |
| WAITING,
TIMED_WAITING | 쓰레드의 작업이 종료되지는 않았지만 실행가능하지 않은(unrunnable) 일시정지 상태. TIMED_WAITING은 일시정지시간이 지정된 경우를 의미 |
| TERMINATED | 쓰레드의 작업이 종료된 상태 |

<br>

<br>

<img src="https://github.com/JaeYeon33/TIL/blob/main/JAVA/image/Untitled%2030.png?raw=true" alt="picture" style="zoom:67%;" />

1. 쓰레드를 생성하고 start()를 호출하면 바로 실행되는 것이 아니라 실행대기열에 저장되어 자신의 잧례가 될 때까지 기다려야 한다. 실행대기열은 큐(queue)와 같은 구조로 먼저 실행대기열에 들어온 쓰레드가 먼저 실행된다.
2. 실행대기상태에 있다가 자신의 차례가 되면 실행상태가 된다.
3. 주어진 실행시간이 다되거나 yield()를 만나면 다시 실행대기상태가 되고 다음 차례의 쓰레드가 실행상태가 된다.
4. 실행 중에 suspend(), sleep(), wait(), join(), I/O block에 의해 일시정지상태가 될 수 있다. I/O block은 입출력작업에서 발생하는 지연상태를 말한다. 사용자의 입력을 기다리는 경우를 예로 들 수 있는데, 이런 경우 일시정지 상태에 있다가 사용자가 입력을 마치면 다시 실행대기 상태가 된다.
5. 지정된 일시정지시간이 다되거나(time-out), Notify(), resume(), interrupt()가 호출되면 일시 정지상태를 벗어나 다시 실행대기열에 저장되어 자신의 차례를 기다리게 된다.
6. 실행을 모두 마치거나 stop()이 호출되면 쓰레드는 소멸된다.

<br>

<br>

### sleep(long millis) - 일정시간동안 쓰레드를 멈추게 한다.

```java
static void sleep(long millis)
static void sleep(long millis, int nanos)
```

<br>

<br>

### interrupt()와 interrupted() - 쓰레드의 작업을 취소

- interrupt(): 쓰레드에게 작업을 멈추라고 요청한다. But, 강제로 종료시키지는 못한다. 그저 interrupted 상태(인스턴스 변수)를 바꾸는 것일 뿐
- interrupted(): 쓰레드에 대해 interrupt()가 호출되었는지 알려준다.
    - interrupt() → 호출되지 않았으면 false
    - interrupt() → 호출되었으면 true


<br>

```java
Thread th = new Thread();
th.start();
	...
th.inerrupt(); // 쓰레드 th에 interrupt()를 호출

class MyThread extends Thread {
		public void run() {
				while (!interrupted()) { // interrupted()의 결과가 false인 동안 반복
						...
				}
		}
}
```

```java
void interrupt()             // 쓰레드의 interrupted상태를 false에서 true로 변경.
boolean isInterrupted()      // 쓰레드의 interrupted상태를 반환.
static boolean interrupted() // 현재 쓰레드의 interrupted상태를 반환 후, False로 변경.
```

<br>

<br>

### suspend(), resume(), stop()

- suspend(): sleep()처럼 쓰레드를 멈추게 한다.
    - suspend()에 의해 정지된 쓰레드는 resume()을 호출해야 다시 실행대기 상태가 된다.
    - sleep()은 호출되는 즉시 쓰레드가 종료된다.


<br>

<u>suspend(), resume(), stop()은 쓰레드의 실행을 제어하는 가장 손쉬운 방법이지만, suspend()와 stop()이 교착상태(deadlock)을 일으키기 쉽게 작성되어있으므로 사용이 권장되지 않는다.</u>

<br>

그래서 이 메서드들은 모두 `deprecated` 되어 있다.

<br>

<br>

### yield() - 다른 쓰레드에게 양보한다.

yield()는 쓰레드 자신에게 주어진 실행시간을 다음 차례의 쓰레드에게 양보(yield)한다.

Ex) 스케쥴러에 의해 실행시간을 할당받은 쓰레드가 0.5초의 시간동안 작업한 상태에서 yield()가 호출되면, 나머지 0.5초는 포기하고 다시 실행대기상태가 된다.

<br>

<br>

### join() - 다른 쓰레드의 작업을 기다린다.

쓰레드 자신이 하던 작업을 잠시 멈추고 다른 쓰레드가 지정된 시간동안 작업을 수행하도록 할 때 join()을 사용

```java
void join()
void join(long millis)
void join(long millis, int nanos)
```

<br>

시간을 지정하지 않으면, 해당 쓰레드가 작업으 모두 마칠 때까지 기다리게 된다. 작업 중에 다른 쓰레드의 작업이 먼저 수행되어야할 필요가 있을 때 join()을 사용.

```java
try {
		th1.join(); // 현재 실행중인 쓰레드가 쓰레드 th1의 작업이 끝날때까지 기다린다.
} catch (InterruptedException e) { }
```

 

<br>

<br>

<br>

## 9. 쓰레드의 동기화

한 쓰레드가 진행 중인 작업을 다른 쓰레드가 간섭하지 못하도록 막는 것을 쓰레드의 동기화(synchronization)이라고 한다.

<br>

### 1) synchronized를 이용한 동기화

```java
// 1. 메서드 전체를 임계 영역으로 지정
public synchronized void caclSum() {
		// ...
}

// 2. 특정한 영역을 임계 영역으로 지정
synchronized(객체의 참조변수) {
		// ...
}
```

<br>

<br>

**첫 번째 방법**

쓰레드는 `synchronized` 메서드가 호출된 시점부터 해당 메서드가 포함된 객체의 lock을 얻어 작업을 수행하다가 메서드가 종료되면 lock을 반환

<br>

<br>

**두 번째 방법**

참조변수는 락을 걸고자하는 객체를 참조하는 것이어야 한다. 이 블럭을 synchronized블럭이라고 부르며, 이 블럭의 영역 안으로 들어가면서부터 쓰레드는 지정된 객체의 lock을 얻고, 이블럭을 벗어나면 lock을 반환

<br>

두 방법 모두 lock의 획득과 반납이 모두 자동적으로 이루어지므로 우리가 해야 할 일은 그저 임계 영역만 설정해주는 것뿐이다.

<br>

<br>

### 2) wait()과 notify()

동기화된 임계 영역의 코드를 수행하다가 작업을 더 이상 진행할 상황이 아니면 `wait()` 를 호출하여 쓰레드가 락을 반납하고 기다리게 한다. 그러면 다른 쓰레드가 락을 얻어 해당 객체에 대한 작업을 수행할 수 있게 한다. 나중에 작업을 진행할 수 있는 상황이 되면 `notify()` 를 해출해서, 작업을 중단했던 쓰레드가 다시 락을 얻어 작업을 진행할 수 있게 한다.

<br>

`wait()` 과 `notify()` 는 특정 객체에 대한 것이므로 Object클래스에 정의되어 있다.

```java
void wait()
void wait(long timeout)
void wait(long timeout, int nanos)
void notify()
void notifyAll()
```

<br>

`wait()` 은 `notify()` 또는 `notifyAll()` 이 호출될 때까지 기다리지만, 매개변수가 있는 `wait()` 은지정된 시간동안만 기다린다. 지정된 시간이 지난 후에 자동적으로 `notify()` 가 호출되는 것과 같다.

<br>

<br>

**wait(), notify(), notifyAll()**

- Object에 정의되어 있다.
- 동기화 블록(synchronized블록)내에서만 사용할 수 있다.
- 보다 효율적인 동기화를 가능하게 한다.

<br>

<br>

### 3) Lock과 Condition을 이용한 동기화

synchronized블럭으로 동기화를 하면 자동적으로 lock이 잠기고 풀리기 때문에 편리하다. 심지어 synchronized블럭 내에서 예외가 발생해도 lock은 자동적으로 풀린다.

But, 때로는 같은 메소드 내에서만 lock을 걸 수 있다는 제약이 불편하기도 하다.

```java
ReentrantLock          // 재진입이 가능한 lock. 가장 일반적인 배타 lock.
ReentrantReadWriteLock // 읽기에는 공유적이고, 쓰기에는 배타적인 lock.
StampedLock            // ReentranReadWriteLock에 낙관적인 lock의 기능을 추가
```

StampedLock: 무조건 읽기 lock을 걸지 않고, 쓰기와 읽기가 충돌할 때만 쓰기가 끝난 후에 읽기 lock을 거는 것이다.

<br>

가장 일반적인 StampedLock을 이용한 낙관적 읽기의 예이다.

```java
int getBalance() {
		long stamp = lock.tryOptimisticRead(); // 낙관적 읽기 lock을 건다.

		int curBalance = this.balance; // 공유 데이터인 balance을 읽어온다.

		if (!lock.validate(stamp)) {  // 쓰기 lock에 의해 낙관적 읽기 lock이 풀렸는지 확인
				stamp = lock.readLock();  // lock이 풀렸으면, 읽기 lock을 얻으려고 기다린다.

				try {
						curBalance = this.balance; // 공유 데이터를 다시 읽어온다.
				} finally {
						lock.unlockRead(stamp);    // 읽기 lock을 푼다.
				}
		}

		return curBalance; // 낙관적 일ㄹ끼 lock이 풀리지 않았으면 곧바로 읽어온 값을 반환
}
```

<br>

<br>

### ReentrantLock의 생성자

```java
ReentrantLock()
ReentrantLock(boolean fair)
```

<br>

생정자의 매개변수를 true로 주면, lock이 풀렸을 때 가장 오래 기다린 쓰레드가 lock을 획득할 수 있게, 즉 공정(fair)하게 처리한다.

But, 공정하게 처리하려면 어떤 쓰레드가 가장 오래 기다렸는지 확인하는 과정을 거칠 수 밖에 없으므로 성능은 떨어진다.

대부분의 경우 굳이 공정하게 처리하지 않아도 문제가 되지 않으므로 공정함보다 성능을 선택한다.

```java
void lock()        // lock을 잠근다.
void unlock()      // lock을 해지한다.
boolean isLocked() // lock이 잠겼는지 확인한다.
```

<br>

synchronized블럭은 자동적으로 lock의 잠금과 해제가 관리된다.

But, ReentrantLock과 같은 lock클래스들은 수동으로 lock을 잠그고 해지해야 한다.

<br>

**How?**

→ 메서드를 호출하기만 하면 된다.

<br>

```java
synchronized(lock) {
		// 임계 영역
}
```

```java
lock.lock();
// 임계영역
lock.unlock();
```

<br>

임계 영역 내에서 예외가 발생하거나 return문으로 빠져 나가게 되면 lock이 풀리지 않을 수 있으므로 unlock()은 try - finally문으로 감싸는게 일반적

```java
lock.lock();
try {
		// 임계영역
} finally {
		lock.unlock();
}	
```

<br>

- `tryLock()`: lock()과 달리, 다른 쓰레드에 의해 lock이 걸려 있으면 lock을 얻으려고 기다리지 않는다. or 지정된 시간만큼만 기다린다. lock을 얻으면 true를 반환, 얻지 못하면 false

```java
boolean tryLock()
boolean tryLock(long timeout, TimeUnit unit) throws InterruptedExdception
```

<br>

lock(): lock을 얻을 때까지 쓰레드를 블락(block)시키므로 쓰레드의 응답성이 나빠질 수 있다. 응답성이 중요한 경우, tryLock()을 이용해서 지정된 시간동안 lock을 얻지 못하면 다시 작업을 시도할 것인지 포기할 것인지를 사용자가 결정할 수 있게 하는 것이 좋다.

<br>

InterruptedException: 지정된 시간 동안 lock을 얻으려고 기다리는 중에 interrupt()에 의해 작업을 취소될 수 도 있도록 코드를 작성할 수 있다는 뜻.

<br>

<br>

### ReentrantLock과 Condition

Condition은 이미 생성된 lock으로부터 newCondition()을 호출해서 생성한다.

```java
private ReentrantLock = new ReentrantLock(); // lock을 생성

// lock으로 condition을 생성
private Condition forCook = lock.newCondition();
private Condition forCust = lock.newCondition();
```

<br>

두 개의 Condition을 생성했다.

한 개는 요리사 쓰레드를 위한 것, 다른 한 개는 손님 쓰레드를 위한 것

wait() & notify() → Condition의 await() & signal() 사용

<br>

<br>

**wait() & notify()와 await() & signal의 비교**

| Object | Condition |
| --- | --- |
| void wait() | void await()<br />void awaitUninterruptibly() |
| void wait(long timeout) | boolean await(long time, TimeUnit unit)<br />long    awaitNanos(long nanosTimeout)<br />long    awaitNanos(long nanosTimeout) |
| void notify() | void signal() |
| void notifyAll() | void signalAll() |

<br>

<br>

**wait() & notify() → Condition의 await() & signal() 사용**

```java
public void add(String dish) {
		lock.lock();

		try {
				while (dishes.size() >= MAX_FOOD) {
						String name = Thread.currentThread().getName();
						System.out.println(name + " is waiting");
						try {
								forCook.await(); // wait();  COOK쓰레드를 기다리게 한다.
						} catch (InterruptedException e) {}
				}
				dishes.add(dish);
				forCust.signal(); // notify();  기다리고 있는 CUST를 깨우기 시작.
				System.out.println("Dishes:" + dishes.toString());
		} finally {
				lock.unlock();
		}
}
```

<br>

<br>

### 4) volatile

![picture](https://github.com/JaeYeon33/TIL/blob/main/JAVA/image/Untitled%2031.png?raw=true)

<br>

멀티 코어 프로세서는 각각의 코어가 별도의 캐시를 가지고 있다.
그리고 각각의 코어는 메모리에서 읽어온 값을 캐시에 저장하고 읽어서 작업을 수행하는데,
같은값을 다시 읽을때는 먼저 캐시를 확인 후 없을때만 메모리에서 읽어온다.

<br>

그러다보니 위 그림처럼 코어의 캐시에 저장된 값과 메모리에 저장된 값이 불일치하는 경우가 생길 수 있다.
그래서 나온게 volatile 이라는 키워드이고 해당 키워드를 이용해 이러한 문제를 해결한다.

<br>

<br>

**Q.어떻게?**

**A.valatile 키워드를 붙일 경우 코어가 변수의 값을 캐시가 아닌 메모리에서 읽는다.**

<br>

<br>

### Happendes-Before

JDK 5부터 volatile 키워드는 단순히 해당 변수의 작업(읽기/쓰기)의 원자성만 보장하는 것이 아니라 이 스레드가 volatile 변수를 수정하기 전에 수정한 모든 변수들이 함께 메모리에 저장(flushed)된다.

volatile 변수를 메모리에서 읽을때도 마찮가지로 다른 모든 변수들도 같이 읽어들여진다.

<br>

<br>

### 주의점

volatile 키워드는 변수의 작업(읽기/쓰기)를 원자화 하는 것이지 동기화 하는 것은 아니다.

즉 동기화가 필요할 때 volatile이 synchronized 블럭을 대체할 수 없다는 의미이다.

```java
volatile long balance;

synchronized int getBalance() {
		return balance;
}

synchronized void withdraw(int money){
		if(balance >= money) {
				balance -= money;
		}
}
```

<br>

⇒ balance 변수가 volatile로 작업을 원자화 했으니 getBalance의 synchronized 키워드가 불필요해보일 수 있다. 하지만 해당 키워드로 동기화를 하지 않을 경우 특정 스레드에서 withdraw()가 호출되어 lock이 걸리고 로직이 처리되는 중에도 getBalance() 가 호출이 가능해질 수 있다.

<br>

<br>

### 참고 - synchronized

*synchronized  를 사용해도 volatile과 동일한 효과를 얻을 수 있다.*

*스레드가 synchronized 블럭으로 들어갈 때와 나올 때, 캐시와 메모리간의 동기화가 이뤄지면서 값의 불일치가 해소되기 때문이다.*

<br>

<br>

### 참고 - valatile로 long과 douvle 원자화

JVM은 데이터를 4Byte단위로 처리하기에 int와 int보다 작은 타입들은 한번에 읽고 쓸 수 있다. 이 말은 하나의 명령어로 읽기/쓰기가 가능하다는 의미인데, 그럼 4Byte이상인 타입은 어떻게 해야 할까?

<br>

long이나 double과 같이 크기가 8Byte인 변수는 하나의 명령어로 읽기/쓰기를 할 수 없기 때문에 변수의 값을 읽는 과정에 다른 스레드가 끼어들 수 있다. 그럼 어떻게 해결을 해야할까?

<br>

이 때, volatile을 사용하면 된다.volatile은 해당 변수에 대한 읽기/쓰기를 원자화 하는데, 이는 작업을 더 이상 나눌 수 없게 한다는 의미가되고 작업 중간에 다른 스레드에서 끼어들 수 없게 된다는 의미가 된다.

<br>

<br>

### 정리

- 캐시가 아닌 메모리에서 값을 읽도록 하는 키워드
- 변수의 값을 작성할 때 메모리에까지 작성해 변수의 값 불일치를 막는다.
- 멀티 쓰레드 환경에서 작업(읽기/쓰기)의 원자성이 보장되야 할 경우 valatile이 적절.

<br>

<br>

### 5) fork & join 프레임워크

JDK1.7부터 추가되었다. 이 프레임워크는 하나의 작업을 작은 단위로 나눠서 여러 쓰레드가 동시에 처리하는 것을 쉽게 만들어 준다.

```java
RecursiveAction // 반환값이 없는 작업을 구현할 때 사용
RecursiveTask   // 반환값이 있는 작업을 구현할 때 사용
```

<br>

두 클래스 모두 `compute()` 라는 추상 메서드를 가지고 있는데, 상속을 통해 이 추상 메서드를 구현하기만 하면 된다.

<br>

<br>

### compute()의 구현

compute()를 구현할 때는 수행할 작업 외에도 작업을 어떻게 나눌 것인가에 대해서도 알려줘야 한다.

```java
public Long compute() {
		long size = to - from + 1; // from <=i <= to

		if (size <= 5)     // 더할 숫자가 5개 이하이면
				return sum();  // 숫자의 합을 반환. sum()은 from부터 to까지의 수를 더해서 반환.

		long half = (from+to) / 2;

		SumTask leftSum = new SumTask(from, half);
		SumTask frightSum = new SumTask(half + 1, to);

		leftSUm.fork(); // 작업(leftSum)을 작업 큐에 넣는다.

		return rightSum.compute() + leftSum.join();
}
```

<br>

<br>

### 다른 쓰레드 작업 훔쳐오기

![picture](https://github.com/JaeYeon33/TIL/blob/main/JAVA/image/Untitled%2032.png?raw=true)

<br>

<br>

### fork()와 join()

```java
fork() // 해당 작업을 쓰레드 풀의 작업 큐에 넣는다. 비동기 메서드
join() // 해당 작업의 수행이 끝날 때까지 기다렸다가, 수행이 끝나면 그 결과를 반환.
```

```java
public Long compute() {
		...
		SumTask leftSum = new SumTask(from, half);
		SumTask rightSum = new SumTask(half + 1, to);
		leftSum.fork(); // 비동기 메서드. 호출 후 결과를 기다리지 않는다.

		return rightSum.compute() + leftSum.join(); // 동기 메서드. 호출결과를 기다린다.
}
```