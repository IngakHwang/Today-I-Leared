# HTML, css ,JS 활용

## mystyle.css

```css
	table{
		border : solid 1px black;
		border-collapse : collapse;
		margin : 50px auto;
	}

	td{
		border : solid 1px black;
		border-collapse : collapse;
	}
	
	#action_btn{
		text-align:center;
		height : 40px;
	}
	.title{
		text-align:right;
		padding-right: 3px;
	}
	
	th{
		height : 100px;
		font-size : 20px;
	}
	.data{
		width : 350px;
		height : 30px;
		padding-left : 10px;
	}
```



## validate.js

```js

 	function hi(){
		alert("Welcome~");	
	}
	
	function getId(){
		var ele = document.getElementById("user_id");
		
		//값이 비어있으면 경고를 출력하시오.
		var id = ele.value;
		if(ele.value==""){
			alert("아이디을 입력하세요");
		} else{
			alert(ele.value);	
		}
	}
	
	function compare_password(){
		var ele1 = document.getElementById("password");
		var ele2 = document.getElementById("password2");
		
		var pw1 = ele1.value;
		var pw2 = ele2.value;
		
		if(pw1!=pw2){
			alert("비밀번호 불일치");
		}else{
			alert("비밀번호 일치")
		}
		
	}
	
	function check_radio(){
		
		var tools = document.getElementsByClassName("tools");
		//alert(tools.length);
		for(var i=0;tools.length;i++){
			if(tools[i].checked){
				alert(tools[i].value);	
			}
		}
		
	}
	
	function check_checkbox(){
		
		var checkboxs = document.getElementsByClassName("lang");
		//alert(checkbox.length);
		for(var i=0;checkbox.length;i++){
			if(checkbox[i].checked){
				alert(checkbox[i].value);
			}
		}
		
		
	}
```

## data_collection.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Data Collection</title>

<link rel="stylesheet" href="../css/mystyle.css">
<script type="text/javascript" src="../js/validate.js"></script>

</head>

<form action="./data_process.jsp" method="post">
	<table>
		<tr>
			<th colspan=3>데이터수집</th>
		</tr>
		
		<tr>
			<td class="title">이름</td>
			<td class="data"><input type='text' name="user_name" value="kim"></td>
			<td></td>
		</tr>
		
		<tr>
			<td class="title">아이디</td>
			<td class="data"><input type='text' name="user_id" value="ghkdrnjsm" id="user_id"></td>
			<td><input type="button" value="중복검사" onclick="getId();"></td>
		</tr>
		
		<tr>
			<td class="title" rowspan="2">패스워드</td>
			<td class="data"><input type='password' name="password" value="1234" id="password"></td>
			<td></td>
		</tr>
		
		<tr>
			<td class="data"><input type='password' name="password2" value="1234" id="password2"></td>
			<td>
				<input type="button" value="패스워드 확인" onclick="compare_password();">
			</td>
		</tr>
		
		<tr>
			<td class="title">전화번호</td>
			<td class="data">
			<!-- <input type='text' size="3" name="phone_no1"> - -->
			<select name="phone_no1">
				<option>--선택--</option>
				<option selected="selected">010</option>
				<option>051</option>
				<option>02</option>
			</select>
			 
			<input type='text' size="3" name="phone_no2"> - 
			<input type='text' size="3" name="phone_no3">
			</td>
			<td></td>
		</tr>
		
		<tr>
			<td class="title">이메일</td>
			<td class="data">
				<input type='text' size="10" name="email1">@
				<input type='text' size="10" name="email2">
			</td>
			<td>
				<select name="email">
					<option>--이메일 선택--</option>
					<option value="nate.com">nate.com</option>
					<option selected="selected" value="naver.com">naver.com</option>
					<option value="google.com">google.com</option>
				</select>
			</td>
		</tr>
		
		<tr>
			<td class="title">사용언어</td>
			<td class="data">
				<input type='checkbox' name="langs" checked="checked" value="java" class="lang">자바
				<input type='checkbox' name="langs" value="python" class="lang">파이썬
				<input type='checkbox' name="langs" value="javascript" class="lang">자바스크립트
				<input type='checkbox' name="langs" value="C#" class="lang">C#
			</td>
			<td><button type="button" onclick="check_checkbox();">다중선택확인</button></td>
		</tr>
		
		<tr>
			<td class="title">사용툴</td>
			<td class="data">
				<input type='radio' name="tools" value="eclipse" class="tools">이클립스
				<input type='radio' name="tools" checked="checked" class="tools" value="notepad">메모장
				<input type='radio' name="tools" value="VSC" class="tools">VSC
			</td>
			<td><button type="button" onclick="check_radio();">단일선택확인</button></td>
		</tr>
		
		<tr>
			<td class="title">비고</td>
			<td class="data">
				<textarea rows="5" cols="30" name="etc">Welcome</textarea>
			</td>
			<td></td>
		</tr>
		
		<tr>
			<td colspan=3 id="action_btn">
				<input type="submit" value="전송">
				<input type="reset" value="다시작성">
			</td>
	
		</tr>
		
	</table>
</form>

<br>
<a href="./form_copy.html">Page</a>

</body>
</html>
```

