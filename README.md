php
===
CREATE TABLE comments(
  Id int PRIMARY KEY,
  Name varchar (50) NOT NULL,
  Text varchar (500) NOT NULL)
  
  
  <html src="js_comments.js">
<div id="comments">
	<head>
		<title>Comments List</title>
		 <link rel="stylesheet" type="text/css" href="style/style.css" />
		 <style type="text/css">
	             h1 {font-size: 30px; font-family: Comic Sans MS}
	             h2 {font-size: 20px; font-family: Comic Sans MS}
	             p {font-size: 20px; font-family: Comic Sans MS}
				 body {
						margin-top: 10px;
						margin-right: 650px;
						margin-bottom: 10px;
						margin-left: 70px;
					   }
	     </style>
	</head>
	<body style="background-color:#ffffff;">
	<b><p style="font-size:20px; font-family:Comic Sans MS">Comments List</p></b>
			 <p style="padding:30px;border:2px solid black;">
			 <textarea rows='18' cols='50' id="text"  name="text"  onfocus="clearText(this)" onblur="clearText(this)" style="font-size:15px; font-family:Comic Sans MS"></textarea>
			 <br/></p>
			 <b><p style="font-size:20px; font-family:Comic Sans MS">Add new comment</p></b>
			 <p style="padding:25px;border:2px solid black;">
			 <input id="name" type="text" name="Name" value="Name" maxlength="60" style="font-size:15px; font-family:Comic Sans MS" onfocus="clearText(this)" onblur="clearText(this)"/><br/>
			 <textarea rows='8' cols='50' id="text"  name="text"  onfocus="clearText(this)" onblur="clearText(this)" style="font-size:15px; font-family:Comic Sans MS">Comment text</textarea>
			 <br/><br/>
			 <tr>
			 <td><input type="submit" value="Add new comment" style="font-size:15px; font-family:Comic Sans MS"></td>	
			 </p> 	 
	</body>
	<?php
include("show_comments.php");
show_comments(Вмонтируйте сюда ид вашей статьи);
?>
</html>



<?php
                $hostname="localhost"; // Имя хоста
                $login="sa"; // Логин для подкл. к серверу баз даных
                $pwd="julia-пк\sqlexpress"; // Пароль для подкл. к серверу баз даных
                $db_name="education";  // Название базы даных
                //подключение к базе
                $con = @mysql_connect($login, $pwd) or die("Error! connect-database");
                mysql_select_db($db_name, $con) or die ("Error! select-database");                 
?>



<?php
// Этот блок кода нужен для корректной работы Ajax скрипта
sleep(1); 
header("Content-type: text/plain; charset=windows-1251");
header("Cache-Control: no-store, no-cache, must-revalidate");
header("Cache-Control: post-check=0, pre-check=0", false);
// Преобразуем полученые данные в нужную кодировку
while(list ($key, $val) = each ($_POST)){$_POST[$key] = iconv("UTF-8","CP1251", $_POST[$key]);}
// Устанавливаем параметры валидации                                       
$nl = strlen($_POST['name']);
$tl = strlen($_POST['text']);
$id = $_GET['id'];
$name = $_POST['name'];
$text = $_POST['text'];
if($nl<0 or $nl>60 or $tl<0 or $tl>500 or $_POST['nr']!='nerobot')
{$validate = false;}
else{$validate = true;}
// Если прошли валидацию
if($validate)
{
// Добавляем комментарий
include("config.php");
mysql_query("insert into comments (id, name, text) values ('{$id}', '{$name}', '{$text}', '0')") or die ("Error! query - add_comment");
echo '<font color="green">Комментарий добавлен и ожидает проверки!</font>';
}
else
{
echo '<font color="red">Заполните правильно поля ввода!</font>';
}
?>



<?php
function show_comments($id)//вывод всех комментариев к статье
{
 include("config.php");
 $res = mysql_query("select * from comments where id like $id and public = 1 order by "id", $con) or die ("Error! query – show comments");
 while($arr = mysql_fetch_array($res, MYSQL_NUM))
 {
 echo "
<div class=main>
   <div class=block_name>
   <span class=name>[2]</span>
   </div>
   <div class=coment>
   <div>$arr[4]</div>
   </div>
   </div>
      ";
}
}
?>


/* ---------------------------- */
/* XMLHTTPRequest Enable */
/* ---------------------------- */
function createObject() {
var request_type;
var browser = navigator.appName;
if(browser == "Microsoft Internet Explorer"){
request_type = new ActiveXObject("Microsoft.XMLHTTP");
}else{
request_type = new XMLHttpRequest();
}
return request_type;
}
 
function ajax(param)
{
      var req = createObject();
      method=(!param.method ? "POST" : param.method.toUpperCase());
      
      if(method=="GET")
      {
          send=null;
          param.url=param.url+"&ajax=true";
      }
      else
      {
          send="";
          for (var i in param.data) send+= i+"="+param.data[i]+"&";
          send=send+"ajax=true";
       
      }
      
      req.open(method, param.url, true);
      if(param.statbox)document.getElementById(param.statbox).innerHTML = '<img src="images/wait.gif"> Пожалуйста подождите...';
      req.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
      req.send(send);
      req.onreadystatechange = function()
      {
          if (req.readyState == 4 && req.status == 200) //если ответ положительный
          {
          if(param.success)param.success(req.responseText);
          }
      }
}
function toggle(id)
{
      var e = document.getElementById(id);
      var dh = gh(id);
      var elems = e.getElementsByTagName('*');
      
      if (e.style.display == "none")
      {
          for(var i=0; i=0;i-=5)
          {
          (function()
               {
              var pos=i;
              setTimeout(function()
              {
                   e.style.height = (pos/100)*dh+"px";
                   if (pos<=0)
                   {
                   e.style.display = "none";
                   e.style.height=lh;
                   }
              },1000-(pos*5));
               }
          )();
          }
          return true;
      }
      return false;
}
 
function vhe(obj, vh){obj.style.visibility=vh;}
 
function gh(id)
{
      var e = document.getElementById(id);
      if(e.style.display == "none")
      {
          e.style.visibility = "hidden";
          e.style.display = "block";
          dh = e.clientHeight||e.offsetHeight+5; // Высота
          e.style.display = "none";
          e.style.visibility = "visible";
      }
      else
      {
          dh = e.clientHeight||e.offsetHeight+5; // Высота
      }
      return dh;
}
function clearText(field)
{
      if(field.defaultValue == field.value)field.value = '';else if(field.value == '')field.value = field.defaultValue;
}
