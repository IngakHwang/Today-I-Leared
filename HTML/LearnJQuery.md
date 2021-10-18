# Learn JQuery

> 화면의 동적 기능을 좀 더 쉽고 편리하게 개발 할 수 있게 해주는 JS 라이브러리

## JS

```js
$(document).ready(function(){											//document에 DOM 로드
	
	$('#idcheck_btn').click(function(){									//id = idcheck_btn 클릭시
		var id = $('#user_id').val();									//id = user_id의 값을 var id에 저장 후
		alert(id);														//var id 알림
	});
	
	$('#pwd_btn').click(function(){								
		var pw1 = $('#pwd').val();										
		var pw2 = $('#pwd2').val();	
		
		if(pw1!=pw2){
			alert("비밀번호 불일치");
		}else{
			alert("비밀번호 일치");
		}
	});
	
	$('#email').change(function(){
		// 선택한 이메일 값 읽어오기
		// 읽어 온 값 이메일 두번째 자리에 세팅하기
		
		var email_val = $('#email').val();								//select태그에서 선택한 값 var email_val에 담기
		// =
		// var email_val = this.value; (위에 선택된 아이디)
		
		$('#email2').val(email_val);									//id = email2의 값 var email_val값으로 세팅
		
	});
	
	$('#direct').click(function(){										//id = direct 항목 클릭 시
		if(this.checked){												//check 되어 있으면
			// $('#email').attr("disabled", false);	
			$('#email').removeAttr("disabled");							//id = email 항목 disabled(사용불가)
		}else{															//check 안되어 있으면
			$('#email').attr("disabled", true);							//id = email 항목 사용가능
		}
		
	});
	
	$('#box_btn').click(function(){										//id = box_btn 항목 클릭시
		var langs = $('.lang');											//class = lang인 항목들 var langs에 담기
		var count = 0;
		var size = $('input[class="lang"]:checked').length;				//input태그 내 class가 lang인 항목이 checked 되어 있는 갯수
																		//를 var size에 담기
		for(var i=0; i<langs.length;i++){
			if(langs[i].checked){
				//alert(langs[i].value);
				count++;					
			}
		}
		//alert("선택갯수 : "+count+"개");
		alert(size);
	});
	
	$('#name_btn').click(function(){
		//var c_name = $('#c_name').text();
		//alert(c_name);
		//$('#c_name').text('사용자이름');										//text => HTML content 변경
		$('#choice_box').html('<input type="checkbox" id ="c_box">직접선택');	// html => HTML tag 입력
	});
	
	var timer;
	$('#start').click(function(){
		
		
		var clock = function(){													//현재시간 id = loc인 항목에 표시
			var now = new Date().toLocaleTimeString();
			//$('#loc').text(now);
			$('#loc').html("<h3>"+now+"</h3>");
		}
		
		timer = setInterval(clock,1000);										//1초마다 clock 함수 실행
	});

	$('#stop').click(function(){												//종료
		clearInterval(timer);
	});
	
});
```





## HTML 

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Data Collection</title>

<link rel="stylesheet" href="../css/mystyle.css">
<script type="text/javascript" src="../js/jquery-3.6.0.min.js"> </script>			<!--jquery 라이브러리-->
<script type="text/javascript" src="../js/test.js"></script>


</head>

<form action="./data_process.jsp" method="post">
	<table>
		<tr>
			<th colspan=3>데이터수집</th>
		</tr>
		
		<tr>
			<td class="title" id="c_name">이름</td>
			<td class="data">
				<input type='text' name="user_name" value="kim">
				<span id="choice_box">
				</span>
			</td>
			<td><input type="button" value="컨텐츠읽기" id="name_btn"></td>
		</tr>
		
		<tr>
			<td class="title">아이디</td>
			<td class="data"><input type='text' name="user_id" value="ghkdrnjsm" id="user_id"></td>
			<td><input type="button" value="중복검사" id="idcheck_btn"></td>
		</tr>
		
		<tr>
			<td class="title" rowspan="2">패스워드</td>
			<td class="data"><input type='password' name="password" value="1234" id="pwd"></td>
			<td>
				<input type="button" value="start" id="start">
				<input type="button" value="stop" id="stop">
			</td>
		</tr>
		
		<tr>
			<td class="data"><input type='password' name="password2" value="1234" id="pwd2"></td>
			<td>
				<input type="button" value="패스워드 확인" id="pwd_btn">
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
				<input type='text' size="10" name="email2" id="email2">
				<input type='checkbox' id="direct"> 직접선택
			</td>
			<td>
				<select name="email" id="email" disabled="disabled">
					<option selected="selected" >--이메일 선택--</option>
					<option value="nate.com">nate.com</option>
					<option value="naver.com">naver.com</option>
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
			<td><input type="button" value="다중선택" id="box_btn"></td>
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
			<td><span id="loc"></span></td>
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

