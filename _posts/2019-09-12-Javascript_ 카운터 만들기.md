---
layout: post
title:  "Javascript_ 카운터 만들기"
author: fennec-fox
tags: Javascript
category:  blog
---

<br>


## index.html

```html
{% raw %}  
<!DOCTYPE html>
<html>

<head>
	<title>Parcel Sandbox</title>
	<meta charset="UTF-8" />
</head>

<body>
	<h2 id="number"> 0 </h2>
	<div>
		<button id="increase"> +1 </button>
		<button id="decrease"> -1 </button>
	</div>
	<script src="src/index.js">
	</script>
</body>

</html>
{% endraw %}  
```

<br>

## index.js

```javascript

const number = document.getElementById("number"); // DOM을 불러드림.
const increase = document.getElementById("increase");
const decrease = document.getElementById("decrease");

increase.onclick = () => { // DOM 이벤트 발생시키기
  const current = parseInt(number.innerText, 10);
  number.innerText = current + 1;
};

decrease.onclick = () => {
  const current = parseInt(number.innerText, 10);
  number.innerText = current - 1;
};

```

<br>

<br>