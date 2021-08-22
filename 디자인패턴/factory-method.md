## 팩토리 메소드 패턴 사용시 장점?

실제 구현한 내용은 `자식 클래스`에서 구현이 되므로

세부 구현 코드를 몰라도 부모클래스에서 자유롭게 사용이 가능하여

객체 간의 결합도가 낮아지는 효과를 볼 수 있습니다.

결합도가 낮아진다는 의미는 한 클래스에 변화가 생겼을 때

다른 클래스에 영향이 끼치는 정도가 낮아진다는 의미입니다.

| example

아래 구조에서 첫번째 보이는 `Robot`을 바탕으로 `RobotFactory` 를 만드는것을 `Factory Method`를 활용해 구현합니다.

```
|-- Robot(abstract class)
|    `-- SuperRobot
|    `-- PowerRobot
|
|-- RobotFactory(abstract class)
|    `-- SuperRobotFactory
|    `-- ModifiedSuperRobotFactory
|--
```

아래 3개 기본 클래스를 생성해둡니다.

```java
package pattern.factory;

public abstract class Robot {
	public abstract String getName();
}
```

```java
package pattern.factory;

public class SuperRobot extends Robot {
	@Override
	public String getName() {
		return "SuperRobot";
	}
}
```

```java
package pattern.factory;

public class PowerRobot extends Robot {
	@Override
	public String getName() {
		return "PowerRobot";
	}
}
```

그리고 위 3개 클래스를 이용한 `RobotFactory` 를 구현합니다.

```java
package pattern.factory;

public abstract class RobotFactory {
	abstract Robot createRobot(String name);
}
```

```java
package pattern.factory;

public class SuperRobotFactory extends RobotFactory {
	@Override
	Robot createRobot(String name) {
		switch( name ){
			case "super": return new SuperRobot();
			case "power": return new PowerRobot();
		}
		return null;
	}
}
```

```java
package pattern.factory;

public class ModifiedSuperRobotFactory extends RobotFactory {
	@Override
	Robot createRobot(String name) {
		try {
			Class<?> cls = Class.forName(name);
			Object obj = cls.newInstance();
			return (Robot)obj;
		} catch (Exception e) {
			return null;
		}
	}
}
```

| Test Code

```java
package pattern.factory;

public class FactoryMain {
	public static void main(String[] args) {

		RobotFactory rf = new SuperRobotFactory();
		Robot r = rf.createRobot("super");
		Robot r2 = rf.createRobot("power");

		System.out.println(r.getName());
		System.out.println(r2.getName());

		RobotFactory mrf = new ModifiedSuperRobotFactory();
		Robot r3 =  mrf.createRobot("pattern.factory.SuperRobot");
		Robot r4 =  mrf.createRobot("pattern.factory.PowerRobot");

		System.out.println(r3.getName());
		System.out.println(r4.getName());
	}
}
```

| result

아래 결과를 통해 팩토리 메소드의 패턴을 이용하여 메인 프로그램에서는 어떤 객체가 생성되더라도 신경쓰지 않고 반환된 객체만 사용하면 됩니다. 새로운 로봇이 추가되고 새로운 팩토리가 추가되어도 `메인 프로그램에서 변경할 것은 최소화 됩니다.`

```java
SuperRobot
PowerRobot
SuperRobot
PowerRobot
```

| 참고

- [팩토리 메소드 패턴](https://jdm.kr/blog/180)
