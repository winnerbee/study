1、查看系统环境变量

```java
for (String a : System.getenv().keySet()) {
	System.out.println(a + "===" + System.getenv(a));
}
```



