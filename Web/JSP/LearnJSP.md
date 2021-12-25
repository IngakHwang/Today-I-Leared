# Java Server Pages

## 현재시간처리

```jsp

<%@page import="java.text.SimpleDateFormat"%>
<%@page import="java.util.Date"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>동적 처리</title>
</head>
<body>

<%	//자바코드 사용 가능
	
	Date now = new Date();
 	
 	SimpleDateFormat format = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
 	
 	String formateNow = format.format(now);
 	
 	System.out.println(formateNow);
 	out.println("<h1>"+formateNow+"</h1>");

%>

<a href = "../day17/data_collection.html">Page이동</a>

</body>
</html>
```



## Random 격언출력

```jsp
<%@page import="java.util.ArrayList"%>
<%@page import="java.util.Random"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>속담 및 격언</title>
</head>
<body>


<%
	//1. 밥 먹고 지냅시다. 쇠는 달구어 졌을 때 때려야 한다.
	//2. 쇠는 달구어 졌을 때 때려야 한다.
	//3. 노력은 배신하지 않는다.
	
	//페이지에 방문 했을 때 랜덤으로 출력되게 하는 것
	Random rm = new Random();

	// Collection 사용
	ArrayList<String> list = new ArrayList<String>();
	list.add("1. 밥 먹고 지냅시다. 쇠는 달구어 졌을 때 때려야 한다.");
	list.add("2. 쇠는 달구어 졌을 때 때려야 한다.");
	list.add("3. 노력은 배신하지 않는다.");
	
	int index = rm.nextInt(3);
	System.out.println(list.get(index));	//Server상에서 print

%>
<h1>오늘의 격언</h1>
	<h2>
        <%
        = list.get(index) 					 //Browser상에서 print
		%>
    </h2>				

</body>
</html>
```

