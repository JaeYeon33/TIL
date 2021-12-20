## 1. java.lang패키지

---

java.lang 패키지는 자바프로그래밍에 가장 기본이 되는 클래스들을 포함하고 있다. 그렇기 때문에 java.lang패키지의 클래스들은 import문 없이도 사용할 수 있게 되어 있다.

<br>

<br>

### 1) Object클래스

**Object 클래스의 메서드**

| Object클래스의 메서드                     | 설명                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| protected Object clone()                  | 객체 자신의 복사본을 반환한다.                               |
| public boolean equals(Object obj)         | 객체 자신과 객체 obj가 같은 객체인지 알려준다. (같으면 true) |
| protected void finalize()                 | 객체가 소멸될 때 가비지 컬렉터에 의해 자동적으로 호출된다. <br />이 때 수행되어야하는 코드가 있을 떄 오버라이딩한다. (거의 사용안함) |
| public Class getClass()                   | 객체 자신의 클래스 정보를 담고 있는 Class인스턴스를 반환한다. |
| public int hashCode()                     | 객체 자신의 해시코드를 반환한다.                             |
| public String toString()                  | 객체 자신의 정보를 문자열로 반환한다.                        |
| public void notify()                      | 객체 자신을 사용하려고 기다리는 쓰레드를 하나만 깨운다.      |
| public void notifyAll()                   | 객체 자신을 사용하려고 기다리는 모든 쓰레드를 깨운다.        |
| public void wait()                        | 다른 쓰레드가 notify()나 notifyAll()을 호출할 때까지 현재 쓰레드를 무한히 또는 <br />지정된 시간(timeout, nanos)동안 기다리게 한다.(timeout은 천 분의 1초, nanos는 10^9분의 1초) |
| public void wait(long timeout)            | 다른 쓰레드가 notify()나 notifyAll()을 호출할 때까지 현재 쓰레드를 무한히 또는 <br />지정된 시간(timeout, nanos)동안 기다리게 한다.(timeout은 천 분의 1초, nanos는 10^9분의 1초) |
| public void wait(long timeout, int nanos) | 다른 쓰레드가 notify()나 notifyAll()을 호출할 때까지 현재 쓰레드를 무한히 또는 <br />지정된 시간(timeout, nanos)동안 기다리게 한다.(timeout은 천 분의 1초, nanos는 10^9분의 1초) |



<br>

Object클래스는 멤버변수는 없고 오직 11개의 메서드만 가지고 있다. 이 메서드들은 기본 인스턴스가 가져야 할 기본적인 것들이다.

<br>

<br>

### quals(Object obj)

매개변수로 객체의 참조변수를 받아서 비교하여 그 결과를 boolean값으로 알려 주는 역할을 한다. 아래의 코드는 Object클래스에 정의되어 있는 equals메서드의 실제 내용이다.

```java
public boolean equals(Object obj) {
		return (this == obj);
}
```

위의 코드에서 알 수 있듯이 두 객체의 같고 다름을 참조변수의 값으로 판단한다.

<br>

<br>

### hasCode()

이 메서드는 해싱(hashing)기법에 사용되는 '해시함수(hash function)'를 구현한 것이다.

해싱은 데이터관리기법 중의 하나인데 다량의 데이터를 저장하고 검색하는데 유용하다.

해시함수는 찾고자하는 값을 입력하면, 그 값이 저장된 위치를 알려주는 해시코드(hashcode)를 반환한다.

일반적으로 해시코드가 같은 두 객체가 존재하는 것이 가능하지만, Object클래스에 정의된 hashCode메서드는 객체의 주소값으로 해시코드를 만들어 반환하기 때문에 32bit JVM에서는 서로 다른 두 객체는 결코 같은 해시코드를 가질 수 없었지만, 64bit JVM에서는 8byte 주소값으로 해시코드(4 byte)를 만들기 때문에 해시코드가 중복될 수 있다.

<br>

<br>

### 클래스의 인스턴스변수 값으로 객체의 같고 다름을 판단해야하는 경우라면?

⇒ equals메서드 뿐 만아니라 hashCode메서드도 적절히 오버라이딩해야 한다. 같은 객체라면 hashCode메서드를 호출했을 때의 결과값인 해시코드도 같아야 하기 때문

<br>

<br>

### toString()

이 메서드는 인스턴스에 대한 정보를 문자열(String)로 제공할 목적으로 정의된 것이다.

인스턴의 정보를 제공한다는 것은 대부분의 경우 인스턴스 변수에 저장된 값들을 문자열로 표현한다는 뜻이다.

Object클래스에 정의된 toString()은 아래와 같다.

```java
public String toString() {
		return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

<br>

클래스를 작성할 떄 toString()을 오버라이딩하지 않는다면, 위와 같은 내용이 그대로 사용될 것이다.

toString()을 호추하면 클래스이름에 16진수의 해시코드를 얻게 될 것이다.

<br>

<br>

### clone()

이 메서드는 자신을 복제하여 새로운 인스턴스를 생성하는 일을 한다. 어떤 인스턴스에 대해 작업을 할 때, 원래의 인스턴스는 보존하고 clone메서드를 이용해서 새로운 인스턴스를 생성하여 작업을 하면 작업이전의 값이 보존되므로 작업에 실패해서 원래의 상태로 되돌리거나 변경기 전의 값을 참고하는데 도움이 될 것이다.

<br>

Object클래스에 정의된 clone()은 단순히 인스턴스변수의 값만 복사하기 때문에 참조타입의 인스턴스 변수가 있는 클래스는 완전한 인스턴스 복제가 이루어지지 않는다.

<br>

<br>

### 공변 반환타입

JDK1.5부터 공변 반환타입(covariant return type)이라는 것이 추가되었는데, 오버라이딩할 때 조상 메서드의 반환타입을 자손 클래스의 타입으로 변경을 허용하는 것.

<br>

```java
	public Point clone() {  // 1. 반환타입을 Object에서 Point로 변경
		Object obj = null;
		try {
				obj = super.clone();
		} catch (CloneNotsupportedException e) {
		return (Point) obj;   // 2. Point 타입으로 형변환한다.
}
```

<br>

이처럼 '공변 반환타입'을 사용하면, 조상의 타입이 아닌, 실제로 반환되는 자손 객체의 타입으로 반환할 수 있어서 번거로운 형변환이 줄어든다는 장점이 있다.

```java
Point copy = (Point) original.clone();
```

```java
Point copy = original.clone();
```

<br>

<br>

### 얕은 복사와 깊은 복사

clone()은 단순히 객체에 저장된 값을 복제할 뿐, 객체가 참조하고 있는 객체까지 복제하지는 않는다. 객체배열을 clone()으로 복제하는 경우에는 원본과 복제본이 같은 객체를 공유하므로 완전한 복제라고 보기 어렵다. 이러한 복제(복사)를 `얕은 복사(shallow copy)` 라고 한다. 얕은 복사에서는 원본을 변경하면 복사본도 영향을 받는다.

원본이 참조고하고 있는 객체까지 복제하는 것을 `깊은 복사(deep copy)` 라고 하며, 깊은 복사에서는 원본과 복사본이 서로 다른 객체를 참조하기 때문에 원본의 변경이 복사본에 영향을 미치지 않는다.

```java
class Circle implements Cloneable {
		Point p;   // 원점 - 참조변수
		double r;  // 반지름

		Circle(Point p, double r) {
				this.p = p;
				this.r = r;
		}

		public Circle clone() {       // 얕은복사
				Object obj = null;

				try {
						obj = super.clone();  // 조상인 Object의 clone()을 호출한다.
				} catch (CloneNoutSupportedException e) {}

				return (Circle) obj;
		}
}
```

<br>

Circle인스턴스 c1을 생성하고, clone()으로 복제해서 c2를 생성하면,

```java
Circle c1 = new Circle(new Point(1, 1), 2.0);
Circle c2 = c1.clone();  // 얕은 복사
```

![picture](https://github.com/JaeYeon33/TIL/blob/main/JAVA/image/Untitled%2011.png?raw=true)



<br>

<br>

### getClass()

이 메서드는 자신이 속한 클래스의 Class객체를 반환하는 메서드이다. Class객체는 이름이 Class인 클래스의 객체이다.

```java
public final class Class implements ... {  // Class클래스
		...
}
```

Class객체는 클래스의 모든 정보를 담고 있으며, 클래스는 당 1개만 존재한다. 그리고 클래스 파일이 클래스 로더(ClsasLoader)에 의해서 메모리에 올라갈 떄, 자동으로 생성된다.

클래스 로더는 실행 시에 필요한 클래스를 동적으로 메모리에 로드하는 역할을 한다. 먼저 기존에 생성된 클래스 객체가 메모리에 존재하는지 확인하고, 있으면 객체의 참조를 반환하고 없으면 클래스 패스(classpath)에 지정된 경로를 따라서 클래스 파일을 찾는다.

못 찾으면 ClassNotFoundException이 발생하고, 찾으면 해당 클래스 파일을 읽어서 Class객체로 반환한다.

<br>

<br>

### ! 정리

파일 형태로 저장되어 있는 클래스를 읽어서 Class클래스에 정의된 형식으로 변환하는 것이다. 클래스 파일을 읽어서 사용하기 편한 형태로 저장해 놓은 것이 클래스 객체이다.

<br>

<br>

### Class객체를 얻는 방법

클래스의 정보가 필요할 때, Class객체에 대한 참조를 얻어와야 하는데, 방법은 여러가지 있다.

```java
Class cObj = new Card().getClass();  // 생성된 객체로 부터 얻는 방법
Class cObj = Card.class;             // 클래스 리터럴(*.class)로 부터 얻는 방법
Class cObj = Class.forName("Card");  // 클래스 이름으로 부터 얻는 방법
```

<br>

`forName` : 특정 클래스 파일, 데이터베이스 드라이버를 메모리에 올릴 때 주로 사용

Class객체를 이용하면 클래스에 정의된 멤버의 이름이나 개수 등, 클래스에 대한 모든 정보를 얻을 수 있기 때문에 Class객체를 통해서 객체를 생성하고 메서드를 호출하는 등 동적인 코드를 작성할 수 있다.

```java
Card c = new Card();                // new연산자를 이용해서 객체 생성
Card c = Card.class.newInstance();  // Class객체를 이용해서 객체 생성
```

<br>

<br>

### 2) String클래스

문자열을 위한 클래스를 제공한다. 문자열을 저장하고 이를 다루는데 필요한 메서드를 함께 제공한다.

<br>

<br>

### 변경 불가능한(immutable)클래스

String클래스에는 문자열을 저장하기 위해서 문자형 배열 참조변수(char[]) value를 인스턴스 변수로 정의해놓고 있다. 인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스변수(value)에 문자형 배열(char[])로 저장되는 것이다.

```java
public final class String implements java.io.Serializable, Comparable {
		private char[] value;
				...
}
```

<br>

한번 생성된 String인스턴스가 갖고 있는 문자열은 읽어 올 수만 있고, 변경할 수는 없다.

```java
String a = "a";
String b = "b";
				a = a + b;
```

덧셈연산자(+)를 사용해서 문자열을 결합하는 것은 매 연산 시 마다 새로운 문자열을 가진 String인스턴스가 생성되어 메모리공간을 차지하게 되므로 가능한 한 결합횟수를 줄이는 것이 좋다.

문자열간의 결합이나 추출 등 문자열을 다루는 작업이 많이 필요한 경우 : StringBuffer클래스

- 저장된 문자열은 변경이 가능하므로 StringBuffer인스턴스만으로도 문자열을 다루는 것이 가능

<br>

<br>

### 문자열의 비교

```java
String str1 = "abc";              // 문자열 리터럴 "abc"의 주소가 str1에 저장됨
String str2 = "abc";              // 문자열 리터럴 "abc"의 주소가 str2에 저장됨
String str3 = new String("abc");  // 새로운 String인스턴스를 생성
String str4 = new STring("abc");  // 새로운 String인스턴스를 생성
```

String클래스의 생성자를 이용한 경우에는 new연산자에 의해서 메모리할당이 이루어지기 때문에 항상 새로운 String인스턴스가 생성된다.

But, 문자열 리터럴은 이미 존재하는 것을 재사용

<br>

<br>

### 문자열 리터럴

자바 소프아리에 포함된 모든 문자열 리터럴은 컴파일 시에 클래스 파일에 저장된다. 이때 같은 내용의 문자열 리터럴은 한번만 저장된다. 문자열 리터럴도 String인스턴스이고, 한번 생성하면 내용을 변경할 수 없으니 하나의 인스턴스를 공유하면 되기 때문

<br>

<br>

### 빈 문자열(empty string)

길이가 0인 배열이 존재할 수 있다. char형 배열도 길이가 0인 배열을 생성할 수 있고, 이 배열을 내부적으로 가지고 있는 문자열이 바로 빈 문자열이다.

'String s = "";' 과 같은 문장이 있을 떄, 참조변수 s가 참고하고 있는 String인스턴스는 내부에 'new char[0]'과 같이 길이가 0인 char형 배열을 저장하고 있는 것이다.

<br>

```java
char[] chArr = new char[0];  // 길이가 0인 char배열
int[] iArr = {};             // 길이가 0인 int배열
```

<br>

길이가 0이기 떄문에 아무런 문자도 저장할 수 없는 배열이라 무의미하게 느껴지겠지만 어쨌든 이러한 표현이 가능하다.

> C언어에서는 문자열의 끝에 null 문자가 항상 붙지만, 자바에서는 null 문자를 사용하지 않는다. 대신 문자열의 길이정보를 따로 저장한다.
> 

```java
String a = null;
char c = '\u0000';
```

```java
String a = "";  // 빈 문자열로 초기화
char c = ' ';   // 공백으로 초기화
```

> '\u0000'은 유니코드의 첫 번째 문자로써 아무런 문자도 지정되지 않은 빈 문자이다.
> 

<br>

<br>

### String클래스의 생성자와 메서드

<br>

**String 클래스의 메서드**

| 메서드 / 설명                                                | 예제                                                         | 결과                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| String(String s) / 주어진 문자열(s)을 갖는 String인스턴스를 생성. | String s = new String("Hello");                              | s = "Hello"                                                  |
| String(char[] value) / 주어진 문자열(value)을 갖는 String인스턴스를 생성. | char[] c = {'H', 'e', '1', '1', 'o'}; <br />String s = new STring(c); | s = "Helo"                                                   |
| String(StringBuffer buf) / StringBuffer인스턴스가 갖고 있는 문자열과 같은 내용의 String인스턴스를 생성. | StringBuffer sb = new StringBuffer("Hello"); <br />String s = new String(sb); | s  = "Hello"                                                 |
| char charAt(int index)  / 지정된 위치(index)에 있는 문자를 알려준다. (index는 0부터 시작) | String s = "Hello" <br />String n = "0123456" <br />char c = s.charAt(1); <br />char c2 = n.charAt(1); | c = 'e' c2 = '1'                                             |
| int compareTo(String str) / 문자열(str)과 사전순서로 비교한다. 같으면 0을, 사전순으로 이전이면 음수를, 이후면 양수를 반환. | int i = "aaa".comapreTo("aaa"); <br />int i2 = "aaa".compareTo("bbb"); <br />int i3 = "bbb".compareTo("aaa"); | i = 0 <br />i2 = -1 i3 = 1                                   |
| String concat(String str) / 문자열(str)을 뒤에 덧붙인다.     | String s = "Hello"; <br />String s2 = s.concat(" World");    | s2 = "Hello World"                                           |
| boolean contains(CharSequence s) / 지정된 문자열(s)이 포함되었는지 검사. | String s = "abcdefg"; <br />boolean b = s.contains("bc");    | b = true                                                     |
| boolean endsWith(String suffex) / 지정된 문자열(suffix)로 끝나는지 검사. | String file = "Hello.txt"; <br />boolean b = file.endsWith("txt"); | b = true                                                     |
| boolean equals(Object obj) / 매개변수로 받은 문자열(obj)과 String인스턴스의 문자열을 비교한다. <br />obj가 String이 아니거나 문자열이 다르면 false를 반환. | String s = "Hello"; <br />boolean b = s.equals("Hello"); <br />boolean b2 = s.equals("hello"); | b = true b2 = false                                          |
| boolean equalsIgnoreCase(String str) / 문자열과 String인스턴스의 문자열을 대소문자 구분없이 비교한다. | String s = "Hello"; <br />boolean b = s.equalsIgnoreCase("HELLO"); <br />boolean b2 = s.equalsIgnoreCase("heLLo"); | b = true b2 = true                                           |
| int indexOf(int ch) / 주어진 문자(ch)가 문자열에 존재하는지 확인하여 위치(index)를 알려준다. <br />못 찾으면 -1을 반환한다. (index는 0부터 시작) | String s = "Hello"; <br />int idx1 = s.indexOf('o'); <br />int idx2 = s.indexOf('k'); | idx1 = 4 idx2 = -1                                           |
| int indexOf(int ch, int pos) / 주어진 문자(ch)가 문자열에 존재하는지 지정된 위치(pos)부터 확인하여 위치(Index)를 알려준다. <br />못 찾으면 -1을 반환한다. (index는 0부터 시작) | String s = "Hello"; <br />int idx1 = s.indexOf('e', 0); <br />int idex2 = s.indexOf('e', 2); | idx = 1 idx2 = -1                                            |
| int indexOf(String str) / 주어진 문자열이 존재하는지 확인하여 그 위치(index)를 알려준다. <br />없으면 -1을 반환한다.(index는 0부터 시작) | String s = "ABCDEFG"; <br />int idx = s.indexOf("CD");       | idx = 2                                                      |
| String intern() / 문자열을 상수풀(constant pool)에 등록한다. <br />이미 상수풀에 같은 내용의 문자열이 있을 경우 그 문자열의 주소값을 반환. | String s = new String("abc"); <br />String s2 = new Sgtring("abc"); <br />boolean b = (s == s2); <br />boolean b2 = s.equals(s2); <br />boolean b3 = (s.intern() == s2.intern()); | b = false b2 = true b3 = true                                |
| int lastIndexOf(int ch) / 지정된 문자 또는 문자코드를 문자열의 오른쪽 끝에서 부터 찾아서 위치(index)를 알려준다. <br />못 찾으면 -1을 반환. | String s = "java.lang.Object"; <br />int idx1 = s.lastIndexOf('.'); <br />int idx2 = s.indexOf('.'); | idx1 = 9 idx2 = 4                                            |
| int lastIndexOf(String str) / 지정된 문자열을 인스턴스의 문자열 끝에서 부터 찾아서 위치(index)를 알려준다. <br />못 찾으면 -1을 반환. | String s = "java.lang.java"; <br />int idx1 = s.lastIndexOf("java"); <br />int idx2 = s.indexOf("java"); | idx1 = 10 idx2 = 0                                           |
| int lengh() / 문자열의 길이를 알려준다.                      | String s = "Hello"; <br />int length = s.length();           | length = 5                                                   |
| String replace(char old, char nw) / 문자열 중의 문자(old)를 새로운 문자(nw)로 바꾼 문자열을 반환. | String s = "Hello"; <br />String s1 = s.replcae('H', 'C');   | s1 = "Cello"                                                 |
| String replace(CharSequence old, CharSequence nw) / 문자열 중의 문자(old)를 새로운 문자(nw)로 바꾼 문자열을 반환. | String s = "Hellollo"; <br />String s1 = s.replace("ll", "LL"); | s1 = "HeLLoLLo"                                              |
| String replaceAll(String regex, String replacement) / 문자열 주에서 지정된 문자열(regex)과 일치하는 것을 <br />새로운 문자열(replacement)로 모두 변경. | String ab = "AABBAABB"; <br />String r = ab.replaceAll("BB", "bb"); | r = "AAbbAAbb"                                               |
| String replaceFirst(String regex, String replacements) / 문자열 중에서 지정된 문자열(regex)과 일치 하는 것 중, <br />첫 번째 것만 새로운 문자열(replacement)로 변경. | String ab = "AABBAABB"; <br />String r = ab.replaceFirst("BB", "bb"); | r = "AAbbAABB"                                               |
| String[] split(String regex) / 문자열을 지정된 분리자(regex)로 나누어 문자열 배열에 담아 반환한다. | String animals = "dog, cat, bear"; String[] arr = animals.split(","); | arr[0] = "dog" arr[1] = "cat" arr[2] = "bear"                |
| String[] split(String regex, int limit) / 문자열을 지정된 분리자(regex)로 담아 반환한다. 단, 문자열 전체를 지정된 수(limit)로 자른다. | String animals = "dog, cat, bear"; String[] arr = animals.split(",", 2); | arr[0] = "dog" arr[1] = "cat, bear"                          |
| boolean startsWith(String prefix) / 주어진 문자열(prefix)로 시작하는지 검사. | String s = "java.lang.Object"; <br />boolean b = s.startsWith("java"); boolean b2 = s.startsWith("lang"); | b = true <br />b2 = false                                    |
| String substring(int begin) String substring(int begin, int end) / 주어진 시작위치(begin)부터 끝 위치(end) 범위에 포함된 문자열을 얻는다. 이 떄, 시작위치의 문자는 범위에 포함되지만, 끝 위치의 문자는 포함되지 안흔다. (begin ≤ x < end) | String s = "java.lang.Object" <br />String c = s.substring(10); <br />String p = s.substring(5, 9); | c = "Object" p = "lang"                                      |
| String toLowerCase() / String인스턴스에 저장되어있는 모든 문자열을 소문자로 변환하여 반환한다. | String s = "Hello"; <br />String s1 = s.toLowerCase();       | s1 = "hello"                                                 |
| String toString() / String인스턴스에 저장되어 있는 문자열을 반환한다. | String s = "Hello"; <br />String s1 = s.toString();          | s1 = "Hello"                                                 |
| String toUpperCase() / String인스턴스에 저장되어있는 모든 문자열을 대문자로 변환하여 반환한다. | String s = "Hello"; <br />String s1 = s.toUpperCase();       | s1 = "HELLO"                                                 |
| String trim() / 문자열의 왼쪽 끝과 오른쪽 끝에 있는 공백을 없앤 결과를 반환한다. <br />이 때 문자열 중간에 있는 공백은 제거되지 않는다. | String s = "   Hello World   "; <br />String s1 = s.trim();  | s1 = "Hello World"                                           |
| static String valueOf(boolean b) <br />static String valueOf(char c) <br />static String valueOf(int i) <br />static String valueOf(long l) <br />static String valueOf(float f) <br />static String valueOf(double d) <br />static String valueOf(Object o) /  지정된 값을 문자열로 변환하여 반환한다. <br />참조변수의 경우, toString()을 호출한 결과를 반환. | String b = String.valueOf(true); <br />String c = String.valueOf('a'); <br />String i = String.valueOf(100); <br />String l = String.valueOf(100L); <br />String f = String.valueOf(10f); <br />String d = String.valueOf(10.0); java.util.Date dd = new java.util.Date(); String date = String.valueOf(dd); | b = "true" <br />c = "a" <br />i = "100" l = "100" <br />f = "10.0" <br />d = "10.0" <br />date = "Wed Jan 27 21:26:29 KST 2016" |





<br>

<br>



<aside>
⚠️ CharSequence는 JDK1.4부터 추가된 인터페이스로 String, StrinBuffer 등의 클래스가 구현되었다.
contains(CharSequence s).replace(CharSequence old, CharSequence nw)는 JDK1.5부터 추가되었다.
java.util.Date dd = new java.util.Date();에서 생성된 Date인스턴스는 현재 시간을 갖는다.
</aside>



<br>

<br>

### join()과 StringJoiner

- joint() : 여러 문자열 사이에 구분자를 넣어서 결합한다. 구분자로 문자열을 자르는 split()과 반대의 작업을 한다고 생각하면 이해하기 쉽다.

```java
String animals = "dog, cat, bear";
String[] arr = animals.split(",");   // 문자여을 ','를 구분자로 나눠서 배열에 저장
String str = String.join("-", arr);  // 배열의 문자열을 '-'로 구분해서 결합
System.out.println(str);             // dog-cat-bear
```

- java.util.StringJointer클래스를 사용해서 문자열을 결합할 수도 있다.

```java
StringJoiner sj = new StringJointer(",", "[", "]");
String[] strArr = {"aaa", "bbb", "ccc"};

for (String s : strArr)
		sj.add(s.toUpperCase());

System.out.println(sj.toString());  // [AAA, BBB, CCC]
```

> join과 StringJoiner는 JDk1.8부터 추가되었다.
> 

<br>

<br>

### 문자 인코딩 변환

getBytes(String charsetName)을 사용하면, 문자열의 문자 인코딩을 다른 인코딩으로 변경할 수 있다.

```java
byte[] utf8_str = "가".getBytes("UTF-8");    // 문자열을 UTF-8로 변환
String str = new String(utf8_str, "UTF-8"); // byte배열을 문자열로 변환
```

<br>

서로 다른 문자 인코딩을 사용하는 컴퓨터 간에 데이터를 주고받을 때는 적절한 문자 인코딩이 필요하다. 그렇지 않으면 알아볼 수 없는 내용의 문서를 보게 될 것이다.

<br>

<br>

### String.format()

format()은 형식화된 문자열을 만들어내는 간단한 방법이다. printf()하고 사용법이 완전히 똑같다.

```java
String str = String.format("%d 더하기 %d는 %d입니다.", 3, 5, 3 + 5);
System.out.println(str);  // 3 더하기 5는 8입니다.
```

<br>

<br>

### 기본형 값을 String으로 변환

숫자로 이루어진 문자열을 숫자로, 또는 그 반대로 변환하는 경우가 자주 있다.

valueOf()를 사용하는 방법이 있는데, 성능은 valueOf가 더 좋지만, 빈 문자열을 더하는 방법이 간단하고 편하기 때문에 성능향상이 필요한 경우에만 사용

```java
int i = 100;
String str1 = i + "";            // 100을 "100"으로 변환하는 방법1
String str2 = String.valueOf(i); // 100을 "100"으로 변환하는 방법2
```

<br>

<br>

### String을 기본형 값으로 변환

반대로 String을 기본형으로 변환하는 방법도 간단하다. valueOf()를 쓰거나 앞서 배운 parseInt()를 사용하면 된다.

```java
int i = Integer.parseInt("100"); // "100"을 100으로 변환하는 방법1
int i2 = Integer.valueOf("100"); // "100"을 100으로 변환하는 방법2
```

<br>

valueOf()의 반환 타입은 int가 아니라 Integer인데, 오토박싱(auto-boxing)에 의해 Integer가 int로 자동 변환된다.

```java
Integer i2 = Integer.valueOf("100");  // 원래는 반환 타입이 Integer
```

<br>

예전에는 `parseInt()` 와 같은 메서드를 많이 사용했다. 메서드의 이름을 통일하기 위해 valueOf()가 나중에 추가되었다. valueOf(String s)는 메서드 내부에서 parseInt(String s)를 호출할 뿐, 두 메서드는 반환 타입만 다르지 같은 메서드이다.

```java
public static Integer valueOf(Stirng s) throws RuntimeFormartException {
		return Integer.valueOf(parseInt(s, 10));  // 여기서 10은 10진수를 의미
}
```

<br>

**기본형과 문자열 간의 변환 방법**

| 기본형 -> 문자열                 | 문자열 -> 기본형                       |
| -------------------------------- | -------------------------------------- |
| String String.valueOf(boolean b) | boolean Booelan.parseBoolean(String s) |
| String String.valueOf(cahr c)    | byte Byte.parseByte(String s)          |
| String String.valueOf(int i)     | short Short.parseShort(String s)       |
| String String.valueOf(long l)    | int Integer.parseInt(String s)         |
| String String.valueOf(float f)   | long Long.parseLong(String s)          |
| String String.valueOf(double d)  | float Float.parseFloat(String s)       |
| String String.valueOf(double d)  | double Double.parseDouble(String s)    |



<br>

<br>

### 3) StringBuffer클래스와 StringBuilder클래스

String클래슨느 인스턴스를 생성할 때 지정된 문자열을 변경할 수 없지만 StringBuffer클래스는 변경이 가능. 내부적으로 문자열 편집을 위한 버퍼(buffer)를 가지고 있으며, StringBuffer인스턴스를 생성할 때 그 키그를 지정할 수 있다.

편집할 문자열의 길이를 고려하여 버퍼의 길이를 충분히 잡아주는 것이 좋다. 편집 중인 문자열이 버퍼의 길이를 넘어서게 되면 버퍼의 길이를 늘려주는 작업이 추가로 수행되어야하기 때문에 작업효율이 떨어진다.

StringBuffer클래스는 String클래스와 같이 문자열을 저장하기 위한 char형 배열의 참조변수를 인스턴스변수로 선언해 놓고 있다. StringBuffer인스턴스가 생성될 때, char형 배열이 생성되며 이 때 생성된 char형 배열을 인스턴스변수 value가 참조하게 된다.

```java
public final class StringBuffer implements java.io.Serializable {
		private char[] value;
			...
}
```

<br>

<br>

### StringBuffer의 생성자

StringBuffer클래스의 인스턴스를 생성할 떄, 적절한 길이의 char형 배열이 생성되고, 이 배열은 문자열을 저장하고 편집하기 위한 공간(buffer)으로 사용된다.

StringBuffer인스턴스를 생성할 때, 버퍼의 크기를 지정해주지 않으면 16개의 문자를 저장할 수 있는 크기의 버퍼를 생성한다.

```java
public StringBuffer(int length) {
		value = new char[length];
		shared = false;
}

public StringBuffer() {
		this(16); // 버퍼의 크기를 지정하지 않으면 버퍼의 크기는 16이 된다.
}

public StringBuffer(String str) {
		this(str.length() + 16);  // 지정한 문자열의 길이보다 16이 더 크게 버퍼를 생성한다.
		append(str);
}
```

<br>

아래는 버퍼의 크기를 변경하는 내용의 코드이다. StringBuffer인스턴스로 문자열을 다루는 작업을 할 때, 버퍼의 크기가 작업하려는 문자열의 길이보다 작을 때는 내부적으로 버퍼의 크기를 증가시키는 작업이 수행된다. 배열의 길이는 변경될 수 없으므로 새로운 길이의 배려을 생성한 후에 이전 배열의 값을 복사해야한다.

```java
...
// 새로운 길이(newCapacity)의 배열을 생성한다. newCapacity는 정수값이다.
char newValue[] = new char[newCapacity];

// 배열 value의 내용을 배열 newValue로 복사한다.
System.arraycopy(value, 0, newValue, 0, count);  // count는 문자열의 길이
value = newValue;  // 새로 생성된 배열의 주소를 참조변수 value에 저장
```

<br>

<br>

### StringBuffer의 변경

String과 달리 StringBuffer는 내용을 변경할 수 있다.

```java
StringBuffer sb = new StringBuffer("abc");
```

<br>

그리고 sb에 문자열 "123"을 추가하면,

```java
ab.append("123"); // sb의 내용 뒤에 "123"을 추가한다.
```

<br>

append()는 반환타입이 StirngBuffer인데 자신의 주소를 반환한다. 그래서 이 문장이 수행되면, sb에 새로운 문자열이 추가되고 sb자신의 주소를 반환하여 sb2에는 sb의 주소 Ox100이 저장된다.

```java
StringBuffer sb2 = ab.append("ZZ"); // sb의 내용뒤에 "ZZ"를 추가한다.
System.out.println(sb);  // abc123ZZ
System.out.println(sb2); // abc123ZZ
```

<br>

하나의 StringBuffer인스턴스에 대해 연속적으로 append()를 호출하는 것이 가능하다.

```java
StringBuffer sb = new StringBuffer("abc");
sb.append("123");
sb.append("ZZ");
```

```java
StringBuffer sb = new StringBuffer("abc");
sb.append("123").append("ZZ");
```

<br>

<br>

### StringBuffer의 비교

String클래스에서는 equals메서드를 오버라이딩해서 문자열의 내용을 비교하도록 구현되어 있지만, StringBuffer클래스는 equals메서드를 오버라이딩하지 않아서 StringBuffer클래스의 equals메서드를 사용해도 등가비교연산자(==)로 비교한 것과 같은 결과를 얻는다.

```java
StringBuffer sb = new StringBuffer("abc");
StringBuffer sb2 = new StringBuffer("abc");

System.out.println(sb == sb2);      // false
System.out.println(sb.equals(sb2)); // false
```

But, toString()은 오버라이딩되어 있어서 StringBuffer인스턴스에 toString()을 호출하면, 담고있는 문자열을 String으로 반환한다. 그래서 StringBuffer인스턴스에 담긴 문자열을 비교하기 위해서는 StringBuffer인스턴스에 toString()을 호출해서 String인스턴스를 얻은 다음, equals메서드를 사용해서 비교해야 한다.

```java
String s = sb.toString();
String s2 = sb2.toString();

System.out.println(s.equals(s2)); // true
```

<br>

<br>

**StringBuffer클래스의 메서드**

| 메서드 / 설명                                                | 예제 / 결과                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| StringBuffer()                                               | StringBuffer ab = new StringBuffer();                        |
| 16문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스를 생성한다. | sb = ""                                                      |
| StringBuffer(int length)                                     | StringBuffer sb = new StringBuffer(10);                      |
| 지정된 개수의 문자를 담을 수 있는 버퍼를 가진 StringBuffer인스턴스를 생성한다. | sb = ""                                                      |
| StringBuffer(String str)                                     | StringBuffer ab = new StringBuffer("Hi");                    |
| 지정된 문자열 값(str)을 갖는 StringBuffer 인스턴스를 생성한다. | ab = "Hi"                                                    |
| StringBuffer append(boolean b) <br />StringBuffer append(char c) <br />StringBuffer append(char[] str) <br />StringBuffer append(double d) <br />StringBuffer append(float f) <br />StringBuffer append(int i) <br />StringBuffer append(long l) <br />StringBuffer append(Object obj) <br />StringBuffer append(String str) | StringBuffer sb = new StringBuffer("abc"); <br />StringBuffer sb2 = sb.append(true); <br />sb.append('d').append(10.0f); <br />StringBuffer sb3 = sb.append("ABC").append(123); |
| 매개변수로 입력된 값을 문자열로 변환하여 StringBuffer인스턴스가 저장하고 있는 문자열의 뒤에 덧붙인다. | sb = "abctrued10.0ABC123" <br />sb2 = "abctrued10.0ABC123" <br />sb3 = "abctrued10.0ABC123" |
| int capctity()                                               | StringBuffer sb = new StringBuffer(100) <br />sb.append("abcd") <br />int bufferSize = sb.capacity() <br />int stringSize = sb.length() |
| StringBuffer인스턴스의 버퍼크기를 알려준다. <br />length()는 버퍼에 담긴 문자열의 길이를 알려준다. | bufferSize = 100 <br />stringSize = 4(sb에 담긴 문자열이 "abcd"이므로) |
| char cahrAt(int index)                                       | StringBuffer sb = new StringBuffer("abc") <br />char c = sb.charAt(2) |
| 지정된 위치(index)에 있는 문자를 반환한다.                   | c = 'c'                                                      |
| StringBufer delete(int start, int end)                       | StringBuffer sb = new StringBuffer("abc") <br />StringBuffer sb2 = sb.delete(3,6) |
| 시작위치(start)부터 끝 위치(end) 사이에 있는 문자를 제거한다. 단, 끝 위치의 문자는 제외. | sb = "0126" sb2 = "0126"                                     |
| StringBuffer deleteCharAt(int index)                         | StringBuffer sb = new StringBuffer("0123456") sb.deleteCharAt(3) |
| 지정된 위치(index)의 문자를 제거한다.                        | sb = "0123456"                                               |
| StringBuffer insert(int pos, boolean b) <br />StringBuffer insert(int post, char c) <br />StringBuffer insert(int pos, char[] str) <br />StringBuffer insert(int pos, double d) <br />StringBuffer insert(int pos, float f) <br />StringBuffer insert(int pos, int i) <br />StringBuffer insert(int pos, long l) <br />StringBuffer insert(int pos, Object obj) <br />StringBuffer insert(int pos, String str) | StringBuffer sb = new StringBuffer("0123456") <br />sb.insert(4, '.') |
| 두 번째 매개변수로 받은 값을 문자열로 변환하여 지정된 위치(pos)에 추가한다. pos는 0부터 시작 | sb = "0123.456"                                              |
| int length()                                                 | StringBuffer sb = new StringBuffer("0123456") <br />int lengh = sb.length() |
| StringBuffer인스턴스에 저장되어 있는 문자열의 길이를 반환    | length = 7                                                   |
| StringBuffer replace(int start, int end, String str)         | StringBuffer sb = new StringBuffer("123456") sb.replace(3, 6, "AB") |
| 지정된 범위(start~end)의 문자들을 주어진 문자열로 바꾼다. end위치의 문자는 범위에 포함 되지 않는다. (start ≤ x ≤ end) | sb = "012AB6"  "345"를 "AB"로 바꿨다.                        |
| StringBuffer reverse()                                       | StringBuffer sb = new StringBuffer("0123456") sb.reverse()   |
| StringBuffer인스턴스에 저장되어 있는 문자열의 순서를 거꾸로 나열한다. | sb = "654321"                                                |
| void setCharAt(int index, char ch)                           | StringBuffer sb = new StringBuffer("0123456") sb.setCharAt(5, 'o') |
| 지정된 위치의 문자를 주어진 문자(ch)로 바꾼다.               | sb = "01234o6"                                               |
| void setLength(int newLength)                                | StringBuffer sb = new StringBuffer("0123456") <br />sb.setLength(5) StringBuffer sb2 = new StringBuffer("0123456) sb2.setLength(10) <br />String str = sb2.toString().trim() |
| 지정된 길이로 문자열의 길이를 변경한다. 길이를 늘리는 경우에 나머지 빈 공간을 널문자 '\u0000'로 채운다. | sb = "01234" <br />sb2 = "0123456  " <br />str = "0123456"   |
| String toString()                                            | StringBuffer sb = new StringBuffer("0123456") String str = sb.toString() |
| StringBuffer인스턴스의 문자열을 String으로 변환              | str = "0123456"                                              |
| String substring(int start) <br />String substring(int start, int end) | StringBuffer sb = new StringBuffer("0123456") <br />String str = sb.substring(3) <br />String str2 = sb.substring(3, 5) |
| 지정윈 범위 내의 문자열을 String으로 뽑아서 반환한다. <br />시작위치(start)만 지정하면 시작위치부터 문자열 끝까지 뽑아서 반환한다. | str = "3456" <br />str2 = "34"                               |



<br>

<br>

### StringBuilder란?

⇒ 멀티쓰레드에 안전(thread safe)하도록 동기화 되어있다. 동기화가 StringBuffer의 성능을 떨어뜨린다. 멀티쓰레드로 작성된 프로그램이 아닌 겨우, StringBuffer의 동기화는 불필요하게 성능만 떨어뜨리게 된다. 그래서 StringBuffer에서 쓰레드의 동기화만 뺀 StringBuilder가 새로 추가되었다.

StringBuffer와 같은 기능으로 작성되어 있어서 생성자만 바꾸면 된다.

```java
StringBuffer sb;
sb = new StringBuffer();
sb.append("abc");
```

```java
StringBuilder sb;
sb = new StringBuilder();
sb.append("abc");
```

성능향상이 반드시 필요한 경우를 제외하고는 StringBuffer → StingBuilder로 바꿀 필요는 없다.

<br>

<br>

### 4) Math클래스

Math클래스는 기본저긴 수학계산에 유용한 메서드로 구성되어 있다. Math클래스의 생성자는 접근 제어자가 private이기 때문에 다른 클래스에서 Math인스턴스를 생성할 수 없도록 되어있다. ⇒ 클래스 내에 인스턴스변수가 하나도 없어서 인스턴스를 생성할 필요가 없기 때문. Math클래스의 메서드는 모두 static이다.

```java
public static final double E = 2.7182818284590452354;   // 자연로그의 밑
public static final double PI = 3.14159265358979323846; // 원주율
```

<br>

<br>

### 올림, 버림, 반올림

소수점 n번째 자리에서 반올림한 값을 얻기 위해서는 round()를 사용해야 하는데, 이 메서드는 항상 소수점 첫째자리에서 반올림을 해서 정수값(long)을 결과로 돌려준다.

1. 원래 값에 100을 곱한다.
   - 90.7552 * 100 → 9075.52
2. 위의 결과에 Math.round()를 사용한다.

- Math.round(9075.52) → 9076

3. 위의 결과를 다시 100.0으로 나눈다.

- 9076 / 100.0 → 90.76

- 9076 / 100   → 90

<br>

<br>

### 예외를 발생시키는 메서드

```java
int addExact(int x, int y)       // x + y
int subtractExact(int x, int y)  // x - y
int multiplyExact(int x, int y)  // x * y
int incrementExact(int a)        // a++
int decrementExact(int a)        // a--
int negateExact(int a)           // -a
int toIntEact(long value)        // (int)value - int로의 형변환
```

<br>

메서드 이름에 'Exact'가 포함된 메서드들이 JDK1.8부터 새로 추가되었다. 이들은 정수형간의 연산에서 발생할 수 있는 오버플로우(overflow)를 감지하기 위한 것

<br>

<br>

### StrictMath클래스

Math클래스는 최대한의 성능을 얻기 위해 JVM이 설치된 OS의 메서드를 호출해서 사용한다. 즉, OS에 의존적인 계산을 하고 있는 것이다. 부동소수점 계산, 반올림의 처리방법 설정이 OS마다 다를 수 있기 때문에 자바로 작성된 프로그램임에도 불구하고 컴퓨터마다 결과가 다를 수 있다.

이러한 차이를 없애기 위해 선응은 포기하는 대신, 어떤 OS를 사용하더라도 같은 결과를 얻도록 Math클래스를 새로 작성한 것이 StrictMath클래스이다.

<br>

<br>

### 5) 래퍼(Wrapper) 클래스

자바에서는 8개의 기본형을 객체로 다루지 않는데 이것이 바로 자바가 완전한 객체지향 언어가 아니라는 얘기를 듣는이유다. 그 대신 보다 높은 성능을 얻을 수 있었다.

때로는 기본형(primitive type)변수도 어쩔 수 없기 객체로 다뤄야 하는 경우가 있다.

- 매개변수로 객체를 요구할 때
- 기본형 값이 아닌 객체로 저장해야할 떄
- 객체간의 비교가 필요할 때

⇒ 기본형 값들을 객체로 변환하여 작업을 수행해야 한다. 이 때 사용되는 것이 래퍼(wrapper)클래스이다.

<br>

아래의 코드는 int형의 래퍼 클래스인 Integer클래스의 실제코으디ㅏ.

```java
public final class Integer extends Number implements Comparable {
		...

		private int value;

		...
}
```

<br>

**래퍼 클래스의 생성자**

| 기본형  | 래퍼클래스 | 생성자                                                       | 활용예                                                       |
| ------- | ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| boolean | Boolean    | Boolean (boolean value)<br />Booelan (String s)              | Boolean b = new Boolean(true) <br />Boolean b2 = new Boolean("true") |
| char    | Character  | Character (char value)                                       | Character c = new Character('a')                             |
| byte    | Byte       | Byte (byte value)<br />Byte (String s)                       | Byte b = new Byte(10) <br />Byte b2 = new Byte("10")         |
| short   | Short      | Short (short value)<br />Short (String s)                    | Short s = new Short(10) <br />Short s2 = new Short("10")     |
| int     | Integer    | Integer (int value)<br />Integer (String s)                  | Integer i = new Integer(100) <br />Integer i2 = new Integer("100") |
| long    | Long       | Long (long value)<br />Long (String s)                       | Long i = new Long(100) <br />Long i2 = new Long("100")       |
| float   | Float      | Float (double value)<br />Float (float value)<br />FLoat (String s) | Float f = new Float(1.0) <br />Float f2 = new Float(1.0f) <br />Float f3 = new Float("1.0f") |
| double  | Double     | Double (double value)<br />Double (String s)                 | Double d = new Double(1.0) <br />Double d2 = new Double("1.0") |

<br>

<br>

### Number클래스

이 클래스는 추상클래스로 내부적으로 숫자를 멤버변수로 갖는 래퍼 클래스들의 조상이다. 기본형 중에서 숫자와 관련된 래퍼 클래스들은 모두 Number클래스의 자손이라는 것을 알 수 있다.

![picture](https://github.com/JaeYeon33/TIL/blob/main/JAVA/image/Untitled%2012.png?raw=true)

<br>

그 외에도 Number클래스의 자손으로 BigInteger와 BigDecimal 등이 있는데, BigInteger는 long으로도 다룰 수 없는 큰 범위의 정수를, BigDecimal은 double로도 다룰 수 없는 큰 범위의 부동 소수점수를 처리하기 위한 것으로 연산자의 역할을 대신하는 다양한 메서드를 제공한다.

<br>

객체가 가지고 있는 값을 숫자와 관련된 기본형으로 변환하여 반환하는 메서드들을 정의하고 있다.

```java
public abstract class Number implements java.io.Serializable {
		public abstract int intValue();
		public abstract long longValue();
		public abstract float floatValue();
		public abstract double doubleValue();

		public byte byteValue() {
				return (byte)intValue();
		}

		public short shortValue() {
				return (short)intValue();
		}
}
```

<br>

<br>

### 문자열을 숫자로 변환하기

문자열을 숫자로 변환할 때는 아래의 방법 중 하나를 선택해서 사용하면 된다.

```java
int i = new Integer("100").intValue();  // floatValue(), longValue(), ...
int i2 = new Integer.parseInt("100");   // 주로 이방법을 많이 사용
Integer i3 = Integer.valueOf("100");
```

<br>

**문자열을 기본형 또는 래퍼 클래스로 변환하는 방법**

| 문자열 --> 기본형                     | 문자열 --> 래퍼 클래스             |
| ------------------------------------- | ---------------------------------- |
| byte b = Byte.parseInt("100")         | Byte b = Byte.valueOf("100")       |
| short s = Short.parseShort("100")     | Short s = Short.valueOf("100")     |
| int i = Integer.parseInt("100")       | Integer i = Integer.valueOf("100") |
| long l = Long.paserLong("100")        | Long l = Long.valueOf("100")       |
| float f = Float.parseFloat("3.14")    | Float f = Float.valueOf("3.14")    |
| double d = Double.parseDouble("3.14") | Double d = Double.valueOf("3.14")  |

<br>

<br>

JDK1.5부터 도입된 오토박싱(autoboxing) 기능 때문에 기본형일 때와 래퍼 클래스일 때의 차이가 없어졋다. 그래서 그냥 구별없이 `valueOf()` 를 쓰는 것도 괜찮은 방법이다.

But, 성능은 `valueOf()` 가 조금 더 느리다.

<br>

```java
int i = Integer.paseInt("100"); < -- > int i = Integer.valueOf("100");
long l = Long.parseLong("100"); < -- > long l = Long.valueOf("100");
```

<br>

문자열이 10진수가 아닌 다른 진법(radix)의 숫자일 때도 변환이 가능하도록 다음과 같은 메서드가 제공된다.

```java
static int paseInt(String s, int radix)     // 문자열 s를 radix진법으로 인식
static Integer valueOf(String s, int radix)
```

<br>

문자열 "100"이 2진법의 숫자라면 10진수로 4이고, 8진법의 숫자라면 10진수로 64이고,

16진법의 숫자라면 10진수로 256이 된다. 참고로 2진수 100은 100(2)와 같이 표기한다.

```java
   int i4 = Integer.parseInt("100", 2);  // 100(2) -> 4
   int i5 = Integer.parseInt("100", 8);  // 100(8) -> 64
   int i6 = Integer.parseInt("100", 16); // 100(16) -> 256
	 int i7 = Integer.parseInt("FF", 16);  // FF(16) -> 255
// int i8 = Integer.parseInt("FF");      // NumberFormatException발생
```

<br>

16진법에서는 'A~F'의 문자도 허용하지만, 진법을 생각하면 10진수로 간주기 떄문에 NumberFormatException이 발생한다.



<br>

<br>

<br>

## 2. 유용한 클래스

---

### 1) java.util.Objects 클래스

Object클래스의 보조 클래스로 Math클래스처럼 모든 메서드가 'static'이다. 객체의 비교나 널 체크(null check)에 유용하다.

<br>

```java
static boolean isNull(Object obj)
static boolean nonNull(Object obj)
```

<br>

그리고 requireNonNull()은 해당 객체가 널이 아니여야 하는 경우에 사용한다. 만일 객체가 널이면, NullPointerException을 발생시킨다. 두 번째 매개변수로 지정하는 문자열은 예외의 메시지가 된다.

```java
static <T> T requireNonNull(T obj)
static <T> T requireNonNull(T obj, String message)
static <T> T requireNonNull(T obj, Supplier<String> messageSupplier)
```

<br>

전에는 매개변수의 유효성 검사를 이렇게 했어야 했는데, 이제는 requrireNonNull()의 호출만으로 간단히 끝낼 수 있다.

```java
void setName(String name) {
		if (name == null)
				throw new NullPointerException("name must not be null.");
		this.name = name;
}
```

```java
void setName(String name) {
		this.name = Objects.requireNonNull(name, "name must not be null.");
}
```

<br>

Objects에는 compare()가 추가되었는데, 비교대상이 같으면 0, 크면 양수, 적으면 음수를 반환한다.

```java
static int compare(Object a, Object b, Comparator c)
```

<br>

이 메서드는 a와 b 두 객체를 비교하는데 사용할 비교 기준이 필요하다. 그 역할을 하는 것이 Comparator이다.

```java
static boolean equals(Object a, Object b)
static boolean deppEquals(Object a, Object b)
```

<br>

Object클래스에 정의된 equals()가 왜 Objects클래스에도 있는지 궁금할 텐데, 이 메서드의 장점은 null검사를 하지 않아도 된다는 데 있다.

```java
if (a != null && a.equals(b)) {  // a가 null인지 반드시 확인해야 한다.
		...
}
```

```java
if (Objects.equals(a, b)) {  // 매개변수의 값이 null인지 확인할 필요가 없다.
		...
}
```

<br>

equals()의 내부에서 a와 b의 널 검사를 하기 떄문에 따로 널 검사를 위한 조건식을 따로 넣지 않아도 되는 것이다. 이메서드의 실제 코드는 다음과 같다.

```java
public static boolean equals(Object a, Object b) {
			return (a == b) || (a != null && a.equals(b));
}
```

```java
String[][] str2D = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};
String[][] str2D2 = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};

System.out.println(Objects.equals(str2D, str2D2));     // false
System.out.println(Objects.deepEquals(str2D, str2D2)); // true
```

<br>

위와 같이 두 2차원 문자 배열을 비교할 떄, 단순히 equals()를 써서는 비교할 수 없다.

deepEquals()를 쓰면 간단히 끝난다.

```java
static String toString(Object o)
static String toString(Object o, String nullDefault)
```

<br>

<br>

### 2) java.util.Random 클래스

Random클래스를 사용하면 난수를 얻을 수 있다.

```java
double randNum = Math.random();
double randNum = new Random().nextDouble(); // 위의 문장과 동일
```

<br>

1~6사이의 정수를 난수로 얻고자 할 때는 다음과 같다.

```java
int num - (int)(Math.random() * 6) + 1);
int num = new Random().nextInt(6)  + 1; // nextInt(6)은 0~6사이의 정수를 반환
```

<br>

**Math.random() vs Random의 차이점**

- 종자값(seed)을 설정할 수 있다.
- 종자값이 같은 Random인스턴스들은 항상 같은 난수를 같은 순서대로 반환한다.
  - 종가값 : 난수를 만드는 공식에 사용되는 값으로 같은 공식에 같은 값을 넣으면 같은 결과를 얻는 것처럼 같은 종자값을 넣으면 같은 난수를 얻게 된다.

<br>

<br>

### Random클래스의 생성자와 메서드

생성자 Random()은 종자값은 System.currentTimeMillis()로 하기 때문에 실행할 때마다 얻는 난수가 달라진다.

> System.currentTimeMillis() ⇒ 현재시간을 천분의 1초단위로 변환해서 반환한다.

```java
public Random() {
		this(System.currentTimeMillis());  // Rnadom(long seed)를 호출한다.
}
```

<br>

**Ramdon의 생성자와 메서드**

| 메서드                       | 설명                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| Random()                     | 현재시간(System.currentTimeMillis())을 종자값(seed)으로 이용하는 Random인스턴스를 생성한다. |
| Random (long seed)           | 매개변수seed를 종자값으로 하는 Random인스턴스를 생성한다.    |
| boolean nextBoolean()        | boolean타입의 난수를 반환한다.                               |
| void nextBytes(byte[] bytes) | bytes배열에 byte타입의 난수를 채워서 반환한다.               |
| double nextDouble()          | double타입의 난수를 반환한다. (0.0 ≤ x < 1.0)                |
| float nextFloat()            | float타입의 난수를 반환한다. (0.0 ≤ x ≤ 1.0)                 |
| double nextGaussian()        | 평균은 0.0이고 표준편차는 1.0인 가우사인(Gaussian)분포에 따른 double형의 난수를 반환한다. |
| int nextInt()                | int타입의 난수를 반환한다. (int의 범위)                      |
| int nextInt (int n)          | 0 ~ n의 범위에 있는 int값을 반환한다. (n은 범위에 포함되지 않음) |
| long nextLong ()             | long타입의 난수를 반환한다. (long의 범위)                    |
| void setSeed(long seed)      | 종값을 주어진 값(seed)으로 변경한다.                         |



<br>

<br>

### 3) 정규식(Regular Expression) - java.util.regex

정규식이란 텍스트 데이터 중에서 원하는 조건(패턴, pattern)과 일치하는 문자열을 찾아내기 위해 사용하는 것으로 미리 정의된 기호와 문자를 이용해서 작성한 문자열을 말한다. 원래 Unix에서 사용하던 것이고 Perl의 강력한 기능이었는데 요즘은 Java를 비롯해 다양한 언어에서 지원하고 있다.

<br>

<br>

### 4) java.util.Scanner클래스

Scanner는 화면,파일, 문자열과 같은 입력소스로부터 문자데이터를 읽어오는데 도움을 줄 목적으로 JDK1.5부터 추가되었다. Scanner에는 다음과 같은 생성자를 지원하기 때문에 다양한 입력소스로부터 데이터를 읽을 수 있다.

```java
Scanner(String source)
Scanner(File source)
Scanner(InputStream source)
Scanner(Readable source)
Scanner(ReadableByteChannel source)
Scanner(Path source)  // JDK1.7부터 추가
```

<br>

또한 정규식표현을 이용한 라인단위의 검색을 지원하며 구분자(delimiter)에도 정규식 표현을 사용할 수 있어서 복잡한 구분자도 처리가 가능하다.

```java
Scanner useDelimiter(Pattern pattern)
Scanner useDelimiter(String pattern)
```

<br>

JDK1.6부터는 화면 입출력만 전문적으로 담당하는 `java.io.Console` 이 새로 추가되었다.

```java
// JDK1.5이전
BufferedReader br = new BufferedReader (new InputStreamReader(System.in));
String input = br.readLine();

// JDK1.5이후(java.util.Scanner)
Scanner s = new Scanner(System.in)
String input = s.nextLine();

// JDK1.6이후(java.io.console) - 이클립스에서는 동작 X
Console console = System.console();
String input = console.reaLine();
```

```java
boolean -> nextBoolean()
byte    -> nextByte()
short   -> next Short()
int     -> nextInt()
long    -> ntextLong()
double  -> nextDouble()
float   -> nextFloat()
String  -> nextLine()
```

<br>

<aside>
⚠️ 실제 입려된 데이터의 형식에 맞는 메서드를 사용하지 않으면 `inputMismatchException`이 발생
</aside>

<br>

<br>

### 5) java.util.StringTokenizer클래스

StringTokenizer는 긴 문자열을 지정된 구분자(delimiter)를 기준으로 토큰(token)이라는 여러 개의 문자열로 잘라내는 데 사용된다. 예를 들어 "100,200,300,400"이라는 문자열이 있을 때'.'를 구분자로 잘라내면 "100", "200", "300", "400"이라는 4개의 문자열(토큰)을 얻을 수 있다.

<br>

StringTokenizer를 이용하는 방법 이외에도 split(String regex), Scanner의 useDelimiter(String pattern)를 사용할 수도 있지만,

```java
String[] result = "100,200,300,400".split(",");
Scanner sc2 = new Scanner("100,200,300,400").useDelimiter(",");
```

<br>

이 두 가지 방법은 정규식표현을 사용해야하므로 정규식 표현에 익숙하지 않은 경우 StringTokenizer를 사용하는 것이 간단하면서도 명확한 결과를 얻을 수 있다.

But, StringTokenizer는 구분자로 단 하나의 문자 밖에 사용 못함. 복잡한 형태의 구분자로 문자여을 나눠야할 때는 정규식을 사용하는 메서드를 사용해야 할 것

<br>

<br>

**StringTokenizer의 생성자와 메서드**

| 생성자 / 메서드                                              | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| StringTokenizer(String str, String delim)                    | 문자열(str)을 지정된 구분자(delim)로 나누는 Stringtokenizer를 생성한다. <br />(구분자는 토큰으로 간주되지 않음) |
| StringTokenizer(String str, String delim, boolean returnDelims) | 문자열(str)을 지정된 구분자(delim)로 나누는 Stringtokenizer를 생성한다. <br />returnDelims의 값을 true로 하면 구분자도 토큰으로 간주된다. |
| int countTokens()                                            | 전체 토큰의 수를 반환한다.                                   |
| boolean hasMoreTokens()                                      | 토큰이 남아있는지 알려준다.                                  |
| String nextToken()                                           | 다음 토큰을 반환한다.                                        |



<br>

<br>



### 6) java.math.BigInteger클래스

정수형으로 표현할 수 있는 값의 한계가 있다. 가장 큰 정수형 타입인 long으로 표현할 수 있는 값은 10진수로 19자리 정도이다. 이 값도 상당히 큰 값이지만, 과학적 계산에서는 더 큰 값을 다뤄야할 때가 있다. 그럴 때 사용하면 좋은 것이 BigInteger이다.

BigInteger는 내부적으로 int배열을 사용해서 값을 다룬다. 그래서 long타입보다 훨씬 큰 값을 다룰 수 있는 것이다. 대신 성능은 long타입보다 떨어질 수밖에 없다.

```java
final int signum;  // 부호. 1 (양수), 0, -1(음수) 셋 중의 하나
final int[] mag;   // 값(magnitude)
```

<br>

BigInteger는 String처럼 불변(immutable)이다. 그리고 모든 정수형이 그렇듯이 BigInteger 역시 값을 '2의 보수'의 형태로 표현된다.

<br>

<br>

### BigInteger의 생성

방법은 여러 가지가 있는데, 문자열로 숫자를 표현하는 것이 일반적이다. 정수형 리터럴로는 표현할 수 있는 값의 한계가 있기 때문

```java
BigInteger val;
val = new BigInteger("12345678901234567890");  // 문자열로 생성
val = new IntInteger("FFFF", 16);              // n진수(radix)의 문자열로 생성
val = BigInteger.valueOf(1234567890L);         // 숫자로 생성
```

<br>

<br>

### 다른 타입으로의 변환

문자열. 또는 byte배열로 변환하는 메서드는 다음과 같다.

```java
String toString()           // 문자열로 변환
String toString(int radix)  // 지정된 진법(radix)의 문자열로 변환
byte[] toByteArray()        // byte배열로 변환
```

<br>

BigInteger도 Number로부터 상속받은 기본형으로 변환하는 메서드들을 가지고 있다.

```java
int    intvalue()
long   longValue()
float  floatValue()
double doubleValue()
```

<br>

정수형으로 변환하는 메서드 중에서 이름 끝에 'Exact'가 붙은 것들을 변환한 결과가 변환한 타입의 범위에 속하지 않으면 ArithmeticException을 발생시킨다.

```java
byte byteValueExact()
int  intvalueExact()
long longValueExact()
```

<br>

<br>

### BigInteger의 연산

```java
BigInteger add(BigInteger val)        // 덧셈(this + val)
BigINteger subtract(BigInteger val)   // 뺄셈(this - val)
BigInteger muiltiply(BigInteger val)  // 곱셈(this * val)
BigInteger divide(BigInteger val)     // 나눗셈(this / val)
BigInteger remainder(BigInteger val)  // 나머지(this % val)
```

<br>

<br>

### 비트 연산 메서드

워낙 큰 숫자를 다루기 윟나 클래스이므로, 성능을 향상시키기 위해 비트단위로 연산을 수행하는 메서드들을 많이 가지고 있다.

```java
int bitCount()              // 2진수로 표현했을 떄, 1의 개수(음수는 0의 개수)를 반환
int bitLength()             // 2진수로 표현했을 때, 값을 표현하는데 필요한 bit수
boolean testBit(int n)      // 우측에서 n+1번째 비트가 1이면 true, 0이면 false
BigInteger setBit(int n)    // 우측에서 n+1번째 비트를 1로 변경
BigInteger clearBit(int n)  // 우측에서 n+1번째 비트를 0으로 변경
BigInteger flipBit(int n)   // 우측에서 n+1번째 비트를 전환(1->0, 0->1)
```

> n의 값은 배열의 index처럼 0부터 시작하므로, 우측에서 첫 번째 비트는 n이 0이다.

<br>

정수가 짝수인지 확인할 때, 정수를 2로 나머지 연산한 결과가 0인지 확인하는 조건

```java
BigInteger bi = new BigInteger("4");

if (bi.remainder(new BigInteger("2")).equals(BigInteger.ZERO)) {
		...
}
```

<br>

대신 짝수는 제일 오른쪽 비트가 0일 것이므로, testBit(0)으로 마지막 비트를 확인하는 것이 더 효율적이다.

```java
BigInteger bi = new BigInteger("4");

if (!bi.testBit(0)) {  // if (bi.testBit(0) == false)
	...
}
```

<br>

<br>

### 7) java.math.BigDecimal클래스

double타입으로 표현할 수 있는 값은 상당히 범위가 넓지만, 정밀도가 최대 13자리 밖에 되지 않고 실수형의 특성상 오차를 피할 수 없다. BigDecimal은 실수형과 달리 정수를 이용해서 실수를 표현한다.

scale은 0부터 Integer.MAX_VALUE사이의 범위에 있는 값이다. 그리고 BigDecimal은 정수를 저장하는데 BigInteger를 사용한다.

> BigInteger처럼 BigDecimal도 불변(immutable)이다.

```java
private final BigInteger intVal;  // 정수(unscaled value)
private final int scale;          // 지수(scale)
private transient int precision;  // 정밀도(precision) - 정수의 자리수
```

```java
BigDecimal val = new BigDecimal("123.45");  // 12345x10^-2
System.out.println(val.unscaledValue());    // 12345
System.out.println(val.scale());            // 2
System.out.println(val.precision());        // 5
```

<br>

<br>

### BigDecimal의 생성

문자열로 숫자를 표현하는 것이 일반적이다. 기본형 리터럴로는 표현할 수 있는 값의 한계가 있기 때문.

```java
BigDecimal val;
val = new BigDecimal("123.4567890"); // 문자열로 생성
val = new BigDecimal(123.456);       // double타입의 리터럴로 생성
val = new BigDecimal(123456);        // int, long타입의 리터럴로 생성가능
val = BigDecimal.valueOf(123.456);   // 생성자 대신 valueOf(double)사용
val = BigDecimal.valueOf(123456);    // 생성자 대신 valueOf(int)사용
```

<br>

주의할 점은, double타입의 값을 매개변수로 갖는 생성자를 사용하면 오차가 발생할 수 있다는 것이다.

```java
System.out.println(new bigDecimal(0.1));    // 0.100000000000000005555111...
System.out.println(new BigDEcimal("0.1"));  // 0.1
```

<br>

<br>

### 다른 타입으로의 변환

문자열로 변환하는 메서드는 다음과 같다.

```java
String toPlainString()  // 어떤 경우에도 다른 기호없이 숫자로만 표현
String toString()       // 필요하면 지수형태로 표현할 수도 있음
```

<br>

대부분의 경우 이 두 메서드의 반환결과가 같지만, BigDecimal을 생성할 때 다른 결과를 얻는 경우가 있다.

```java
BigDecimal val = new BigDecimal(1.0e-22);
System.out.println(val.toPlainString());  // 0.00000000000010..
System.out.println(val.toString());       // 1.000000000048...5E-22
```

<br>

BigDecimal도 Number로 부터 상속받은 기본형으로 변환하는 메서드들을 가지고 있다.

```java
int    intValue()
long   longValue()
float  floatValue()
double doubleValue()
```

<br>

정수형으로 변환하는 메서드 중에서 이름 끝에 'Exact'가 붙은 것들은 변환한 결과가 변환한 타입의 범위에 속하지 않으면 ArithmeticException을 발생시킨다.

```java
byte       byteValueExact()
short      shortValueExact()
int        intValueExact()
long       longValueExact()
BigInteger toBigIntegerExact()
```

<br>

<br>

### BigDecimal의 연산

실수형에 사용할 수 있는 모든 연산자와 수학적인 계싼을 쉽게 해주는 메서드들이 정의되어 있다.

```java
BigDecimal add(BigDecimal val)       // 덧셈(this + val)
BigDecimal subtract(BigDecimal val)  // 뺄셈(this - val)
BigDeicmal multiply(BigDecimal val)  // 곱셈(this * val)
BigDecimal divide(BigDecimal val)    // 나눗셈(this / val)
BigDecimal remainder(BigDecimal val) // 나머지(this % val)
```

<br>

BigInteger와 마찬가지로 BigDecimal은 불변이므로, 반환타입이 BigDecimal인 경우 새로운 인스턴스가 반환된다.

한 가지 알아둬야 할 것은 연산결과의 정수, 지수, 정밀도가 달라진다는 것.

```java
                                             // value, scale, precision
BitDecimal bd1 = new BigDecimal("123.456");  // 123456,    3,  6
BigDecimal bd2 = new BigDecimal("1.0");      // 10,        1,  2
BigDecimal bd3 = bd1.multiply(bd2);          // 1234560,   4,  7
```

<br>

<br>

### 반올림 모드 - divide()와 setScale()

다른 연산과 달리 나눗셈을 처리하기 위한 메서드는 다음과 같이 다양한 버전이 존재한다. 나눗셈의 결과를 어떻게 반올림(roundingMode)처리할 것인가와, 몇 번째 자리(scale)에서 반올림할 것인지를 지정할 수 있다. BigDecimal이 아무리 오차없이 실수를 저장한다해도 나눗셈에서 발생하는 오차는 어쩔 수 없다.

```java
BigDecimal divide(BigDecimal divisor)
BigDecimal divide(BigDecimal divisor, int roundingMode)
BigDecimal divide(BigDecimal divisor, RoundingMode roundingMode)
BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)
BigDecimal divide(BigDecimal divisor, RoundingMode roundingMode)
BigDecimal divide(BigDecimal divisor, MathContext mc)
```

<br>

roundingMode는 반올림 처리방법에 대한 것으로 BigDeicmal에 정의된 'ROUND_'로 시작하는 상수들 중에 하나를 선택해서 사용하면 된다. RoundingMode는 이 상수들을 열거형으로 정의한 것으로 나중에 추가되었다. 가능하면 열거형 RoundingMode를 사용하자.

<br>

<br>

**열겨헝 RoundingMode에 정의된 함수**

| 상수        | 설명                                                         |
| ----------- | ------------------------------------------------------------ |
| CEILING     | 올림                                                         |
| FLOOR       | 내림                                                         |
| UP          | 양수일 때는 올림, 음수일 때는 내림                           |
| DOWN        | 양수일 때는 내림, 음수일 때는 올림(UP과 반대)                |
| HALF_UP     | 반올림(5이상 올림, 5미만 버림)                               |
| HALF_EVEN   | 반올림(반올림 자리의 값이 짝수면 HALF_DOWN, 홀수면 HALF_UP)  |
| HALF_DOWN   | 반올림(6이상 올림, 6미만 버림)                               |
| UNNECESSARY | 나눗셈의 결과가 딱 떨어지는 수가 아니면, ArithmeticException 발생 |



<br>

<br>

```java
BigDecimal bigd = new BigDecimal("1.0");
BigDecimal bigd2 = new BigDecimal("3.0");

System.out.println(bigd.divide(bigd2));                            // ArithmeticException 발생
System.out.println(bigd.divide(bigd2, 3, RoundingMOde.HALF_UP));  // 0.333
```



<br>

<br>

### java.math.MathContext

이 클래스는 반올림 모드와 정밀도(precision)을 하나로 묶어 놓은 것일 뿐 별다른 것은 없다.

<br>

**한 가지 주의할 점**

- divide() : scale이 소수점 이하의 자리수를 의미
- MathContext : precision이 정수와 소수점 이하를 모두 포함한 자리수를 의미

```java
BigDecimal bd1 = new BigDecimal("123.456");
BigDecimal bd2 = new BigDecimal("1.0");

System.out.println(bd1.divide(bd2, 2, HALP_UP));                   // 123.46
System.out.println(bd1.divide(bd2, new MathContext(2, HALF_UP)));  // 1.2E + 2
```