<!DOCTYPE html>
<html>
<head>
	<meta charset="utf8">
	<title>carousel</title>
	<style type="text/css">
		*{
			margin: 0;
			padding: 0;
		}
		#carousel{
			width: 300px;
			height: 500px;
			overflow: hidden;
			position: relative;
		}
		#list img{
			width: 300px;
			height: 500px;
			float: left;
		}
		#list{
			position: absolute;
			width: 2100px;
		}
		#buttons span{
			background-color: #535353;
			border-radius: 15px;
			width: 15px;
			height: 15px;
			display: block;
			border: 1px solid #ccc;
			float: left;
		}
		#buttons span:nth-child(n+2){
			margin-left: 3px;
		}
		#buttons{
			position: absolute;
			width: 97px;
			left: 0;
			right: 0;
			margin: auto;
			bottom: 5px;
		}
		#buttons span:hover{
			background-color: darkorange;
			cursor: pointer;
		}
		#buttons .on{
			background-color: orangered;
		}
		.arrow{
			height: 35px;
			width: 35px;
			/*opacity: 0.3;*/
			background-color: rgba(0, 0, 0, 0.3);
			display: none;
			color: white;
			text-decoration: none;
			font-size:30px;
			text-align: center;
			position: absolute;
			font-weight: bold;
		}
		#prev{
			left: 5px;
			top: 0;
			bottom: 0;
			margin: auto;
		}
		#next{
			right: 5px;
			top: 0;
			bottom: 0;
			margin: auto;
		}
		#carousel:hover .arrow{
			display: block;
		}
		.arrow:hover{
			background-color: rgba(0, 0, 0, 0.7);
		}
	</style>
</head>
<body>
	<div id="carousel">
		<div id="list" style="left: -300px">
			<img src="image/eg.jpg" alt="">
			<img src="image/户外.jpg" alt="">
			<img src="image/清纯.jpg" alt="">
			<img src="image/美女.jpg" alt="">
			<img src="image/艾薇儿.jpg" alt="">
			<img src="image/eg.jpg" alt="">
			<img src="image/户外.jpg" alt="">
		</div>
		<div id="buttons">
			<span index="1" class="on"></span>
			<span index="2"></span>
			<span index="3"></span>
			<span index="4"></span>
			<span index="5"></span>
		</div>
		<a href="javascript:void(0)" class="arrow" id="prev">&lt</a>
		<a href="javascript:void(0)" class="arrow" id="next">&gt</a>	
	</div>
</body>
<script type="text/javascript">
	function getElementById(id){
		return document.getElementById(id);
	}
	window.onload = function(){

		//获取所有的元素
		var carousel = getElementById("carousel");
		var list = getElementById("list");
		var buttons = getElementById("buttons").getElementsByTagName("span");
		var prev = getElementById("prev");
		var next = getElementById("next");
		var index = 0;
		var animated = 0;
		var timer;

		//显示小圆点
		function showButton(){
			for(var i = 0 ; i < buttons.length ; i ++){
				buttons[i].className = "";
			}
			if(index==-1) index = 4;
			if(index==5) index = 0;
			buttons[index].className = "on";
		}

		//运动函数
		function animate(offset){
			var time = 300;
			var interval = 10;
			var speed = offset / (time/interval);
			var newLeft = parseInt(list.style.left) + offset;


			var go = function(){
				// debugger;
				if(parseInt(list.style.left) < newLeft && speed > 0 || parseInt(list.style.left) > newLeft && speed < 0){
					animated = 1;
					list.style.left = parseInt(list.style.left) + speed + 'px';
					setTimeout(go, interval);
				}else{
					animated = 0;
					list.style.left = newLeft +'px';
					list.style.left = check(parseInt(list.style.left)) + 'px';
				}
			}
			go();

			
		}

		//检测函数
		function check(left){
			if(left > -300) return -1500;
			if(left < -1500) return -300;
		}

		//点击箭头切换页面
		prev.onclick = function(){
			if(!animated){
				animate(300);
				index-=1;
				showButton();
			}else{
				return;
			}
		}
		next.onclick = function(){
			if(!animated){
				animate(-300);
				index+=1;
				showButton();
			}else{
				return;
			}
		}


		//button跟随滚动
		for(var i = 0 ; i < buttons.length ; i ++){
			if(!animated){
				buttons[i].onclick = function(i){
					return function(){
						var offset = (i - index) * -300;
						animate(offset);
						index = i;
						showButton();
					}
				}(i);
			}else{
				return;
			}
		}

		// var timer = 

		function play(){
			timer = setInterval(next.onclick, 1000);
		}

		function stop(){
			clearInterval(timer);
		}

		carousel.onmouseover = stop;
		carousel.onmouseout = play;

		play();
		




	}
</script>
</html>
