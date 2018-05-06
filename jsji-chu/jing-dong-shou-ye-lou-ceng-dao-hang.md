```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
<head>
	<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
	<title>Document</title>
	<style type="text/css">
		*{
			margin: 0;
			padding: 0;
		}
		.header{
			width: 1000px;
			height: 800px;
			background-color: #ccc;
			margin: 0 auto;
			margin-bottom: 20px;
		}

		.no1{
			width: 1000px;
			height: 400px;
			background-color: orange;
			margin: 0 auto;
			margin-bottom: 20px;
		}

		.no2{
			width: 1000px;
			height: 565px;
			background-color: purple;
			margin: 0 auto;
			margin-bottom: 20px;
		}

		.no3{
			width: 1000px;
			height: 365px;
			background-color: skyblue;
			margin: 0 auto;
			margin-bottom: 20px;
		}

		.no4{
			width: 1000px;
			height: 444px;
			background-color: yellowgreen;
			margin: 0 auto;
			margin-bottom: 20px;
		}

		.no5{
			width: 1000px;
			height: 632px;
			background-color: #e30;
			margin: 0 auto;
			margin-bottom: 20px;
		}

		.footer{
			width: 1000px;
			height: 800px;
			background-color: #ccc;
			margin: 0 auto;
			margin-bottom: 20px;
		}
		.floorNav{
			position: fixed;
			width: 50px;
			height: 250px;
			background-color: #F8EFF8;
			bottom:100px;
			left: 20px;
		}
		.floorNav ul{
			list-style: none;
		}
		.floorNav ul li{
			width: 48px;
			height: 48px;
			border: 1px dotted #ccc;
			line-height: 48px;
			text-align: center;
			cursor: pointer;
		}
		.floorNav ul li.s1.cur{
			background: orange;
		}
		.floorNav ul li.s2.cur{
			background: purple;
		}
		.floorNav ul li.s3.cur{
			background: skyblue;
		}
		.floorNav ul li.s4.cur{
			background: yellowgreen;
		}
		.floorNav ul li.s5.cur{
			background: #e30;
		}
	</style>
</head>
<body>
	<div class="floorNav">
		<ul>
			<li class="s1">1F电器</li>
			<li class="s2">2F男装</li>
			<li class="s3">3F女装</li>
			<li class="s4">4F玩具</li>
			<li class="s5">5F零食</li>
		</ul>
	</div>

 	<div class="header"></div>

 	<div class="floor no1">
 		<h1>F1电器商城</h1>
 	</div>

 	<div class="floor no2">
 		<h1>F2</h1>
 	</div>

 	<div class="floor no3">
 		<h1>F3</h1>
 	</div>

 	<div class="floor no4">
 		<h1>F4</h1>
 	</div>

 	<div class="floor no5">
 		<h1>F5</h1>
 	</div>

 	<div class="footer"></div>

 	<script type="text/javascript" src="./jquery-1.12.3.min.js"></script>
 	<script type="text/javascript">
 		var $floornavlis = $(".floorNav ul li");


 		//读取每个floor的offset().top值，放入数组
 		var floorOffsetTopArray = [];

 		$(".floor").each(function(){
 			floorOffsetTopArray.push($(this).offset().top);
 		});
 		//补充一项footer的offset().top值：
 		floorOffsetTopArray.push($(".footer").offset().top);

 		console.log(floorOffsetTopArray);  //[820, 1240, 1825, 2210, 2674 , 3326]

 		//当卷动页面的时候
 		$(window).scroll(function(){
 			var A = $(window).scrollTop();
 			//现在我们就要问问自己，此时这个A在第几个楼层。
 			//说白了，就是看看这个A介于数组floorOffsetTopArray那两项之间
 			for(var i = 0 ; i <= 5 ; i++){
 				if(A >= floorOffsetTopArray[i] && A < floorOffsetTopArray[i + 1]){
 					break;
 				}
 			}

 			//此时这个i有几种情况：
 			//i的值是6，表示用户此时在浏览header或者footer。
 			//i的值是0、1、2、3、4。分别表示此时他在浏览第1F、2F、3F、4F、5F。
 			console.log(i);
 			if(i != 6){
 				$floornavlis.eq(i).addClass("cur").siblings().removeClass("cur");
			}else{
				$floornavlis.removeClass("cur");
			}
 		});

 		//点击楼层导航，页面要冲锋到数组的对应位置上：
 		$floornavlis.click(function(){
 			var index = $(this).index();
 			$("html , body").animate({"scrollTop" : floorOffsetTopArray[index]},300);
 		});
 	</script>
</body>
</html>
```