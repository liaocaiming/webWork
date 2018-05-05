# 一. js回到顶部的方法
```
div id='box1' style="height: 300px; background: red; overflow-y: auto">
	<div id='odiv' style="height: 800px; width: 200px;background: blue">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ducimus explicabo id sequi, eum, mollitia deserunt voluptatibus, placeat ipsam et repellendus aut unde magni sed necessitatibus voluptatum ex dicta maiores asperiores.Lorem ipsum dolor sit amet, consectetur adipisicing elit. Culpa aut, veritatis, repellendus dolorem ratione perspiciatis? Eius ipsa aliquam voluptatum voluptates magnam, nulla iure mollitia dolorum, aut ut earum totam perferendis!</div>
</div>
<button id='btn' style='position: fixed; top: 100px; left: 200px;'>5555</button>
<script type="text/javascript">
var btn = document.getElementById('btn')
var odiv = document.getElementById('odiv')
var box1=document.getElementById('box1')
btn.onclick = function () {
	console.log(odiv, 'div')
	box1.scrollTop = 0 // 将父盒子的scrollTop值设置为0
	console.log(box1.scrollTop)
	// box1.scrollTo(0)
	// odiv.scrollIntoView(true)  // 盒子回到顶部的可视区
}

box1.onscroll = function (e) {
	console.log(e)
}
odiv.addEventListener('scroll', function(e) {
   console.log(e, 'e')
}, false)
```