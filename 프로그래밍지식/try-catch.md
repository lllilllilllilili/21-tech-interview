### 제한된 시스템 자원에 접근해야 하는 함수가 있다. 이 코드에서는 openResource라는 API함수를 호출하여 그 자원에 대한 핸들을 가져와야 하고 일이 끝나면 closeResource에 핸들을 넘겨줘야 한다. 어떤 경우라도 closeResource함수를 호출해서 자원을 잃어버리지 않으려면 어떻게 해야할까?

```java
SomeResource resource = null;
try {
    resource = getResource();
    use(resource);
} catch(...) {
    ...
} finally {
    if (resource != null) {
        try { resource.close(); } catch(...) { /* 아무것도 안 함 */ }
    }
}

(출처: 최범균님 블로그)
```

| try-with-resources
Try-with-resources는 아래의 코드와 같이
try에 자원 객체를 전달하면, try 코드 블록이 끝나면 자동으로 자원을 종료해주는 기능입니다.

즉, 따로 finally 블록이나 모든 catch 블록에 종료 처리를 하지 않아도 됩니다.

```java
try (SomeResource resource = getResource()) {
    use(resource);
} catch(...) {
    ...
}
```

그리고 아래와 같이 try() 안에 복수의 자원 객체를 전달할 수 있다.

```java
public static String getHtml(String url) throws IOException {

	URL targetUrl = new URL(url);

	try (BufferedReader reader = new BufferedReader(new InputStreamReader(targetUrl.openStream()))){
		StringBuffer html = new StringBuffer();
		String tmp;

		while ((tmp = reader.readLine()) != null) {
			html.append(tmp);
		}
		return html.toString();
	}
}
```

| 참고

- [Try-With-Resources를 이용한 자원해제 처리](https://ryan-han.com/post/java/try_with_resources/)
