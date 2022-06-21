## **doGet()? doPost()?**

 doGet(), doPost()는 방식이 다를뿐 하는 역할은 같습니다. GET방식, POST방식 많이 들어봤을텐데 그 역할을 수행합니다. 서블릿이 **요청(request)**을 처리할 수 있도록 허용하기 위해(서비스 메서드를 통해) 서버에서 호출이 됩니다. 이 메서드를 재정의하여 서블릿을 **요청(request)**했을 때, **응답(response)**을 정의합니다.

 응답하는 방식은 여러가지가 있는데 예제에서는 PrintWriter를 사용하여 응답처리를 했습니다. 응답처리를 하기 전 responseHeader의 속성을 수정하여 **응답(response)방식**을 변경할수도 있습니다. 만약 잘못된 형태로 요청을 했거나 재정의한 메서드가 잘 못된 경우 **잘못된 요청(BadRequest)**(개발을 하다보면 자주 볼 수 있는 Error500) 메시지를 반환합니다.



 **구글링 및 제 주관적인 생각을 주저리 주저리 정의한 내용이라 실제기능과 다를 수도 있습니다

 

------

## **doGet()과 doPost()의 차이점**

 차이점은 GET방식과 POST방식의 차이점과 거의 동일하다고 볼 수 있습니다.

대표적인 차이점은 GET방식은 URL 즉 **Header(헤더)**에 변수를 포함시켜 요청을 하고, POST방식은 **Body(바디)**에 변수를 포함시켜 요청을 합니다. 상세한 차이점은 아래 표를 참고 바랍니다.



![img](https://blog.kakaocdn.net/dn/blYbRx/btrvcGZmLtX/lpG404DaBAHrlTbhmfMpP0/img.png)



출처 : https://mangkyu.tistory.com/17

 

------

## **doGet(), doPost() 예제**

 서블릿의 doGet()과 doPost()를 결과를 출력하는 예제를 만들어보았습니다. doGet(), doPost()를 사용하는 분들께 참고바랍니다.

#### **HelloWorldServlet.java**

```
package test;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class HelloWorldServlet
 */
public class HelloWorldServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

    /**
     * Default constructor. 
     */
    public HelloWorldServlet() {
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		PrintWriter pw = response.getWriter();
		pw.write("Hello World Get!!");
		pw.flush();	
		pw.close();
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		PrintWriter pw = response.getWriter();
		pw.write("Hello World Post!!");
		pw.flush();	
		pw.close();
	}

}
```

 간단하게 HelloWorldServlet을 만들어서 doGet()을 호출하면 **"Hello World Get!!"**을 response하도록, doPost()를 호출하면 **"Hello World Post!!"**를 reponse하도록 만들었습니다.

 

#### **web.xml**

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
<servlet>
   	<description>Hello World 서블릿 호출</description>
    <display-name>HelloWorld</display-name>
    <servlet-name>HelloWorld</servlet-name>
    <servlet-class>test.HelloWorldServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>HelloWorld</servlet-name>
    <url-pattern>/HelloWorld.do</url-pattern>
</servlet-mapping>
</web-app>
```

 서블릿과 URL을 매핑해줍니다. "HelloWorld.do"를 URL을 호출하면 위의 서블릿 기능을 수행합니다.

 

#### **index.jsp**

```
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="http://code.jquery.com/jquery-latest.min.js"></script>
<script type='text/javascript'>
$(document).ready(function(){
	// HelloWorld서블릿을 Get방식으로 호출
	$("#btnHelloWorldGet").bind("click", function(){
		$.ajax({            
            type : "GET",
            url : "HelloWorld.do",
            success : function(data){
                $("#inputHelloWorld").val(data);
            },
        	error : function(){
            	alert('통신실패!!');
       		},
        });
	});
	
	// HelloWorld서블릿을 Post방식으로 호출
	$("#btnHelloWorldPost").bind("click", function(){
		$.ajax({            
            type : "POST",
            url : "HelloWorld.do",
            success : function(data){
                $("#inputHelloWorld").val(data);
            },
        	error : function(){
            	alert('통신실패!!');
       		},
        });
	});	
});
</script>
</head>
<body>
<button id="btnHelloWorldGet">HelloWorld Get 호출</button>   
<button id="btnHelloWorldPost">HelloWorld Post 호출</button>   
<input type="text" id="inputHelloWorld">
</body>
</html>
```

ajax를 사용하여 HelloWorldServlet을 호출하는 간단한 테스트페이지를 만들어봤습니다.

버튼을 클릭하면 inputHelloWorld InputBox에 response받은 데이터를 출력합니다.

 

------

#### **화면**

**doGet 호출**



![img](https://blog.kakaocdn.net/dn/Ftbz0/btrvdvwx0Wn/ALQTwLj7CFbkZdj5aL4731/img.png)



 

**doPost 호출**



![img](https://blog.kakaocdn.net/dn/bEmYYt/btru9QaDjb8/TxOiFs64q98kV43wiTdTk0/img.png)

