---
layout: post
title:  "Javascript_ 모달만들기"
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
	<h1> 안녕하세요 </h1>
	<p> 내용내용내용 </p>
	<button id="open"> 모달 열기 </button>
	<div class="modal-wrapper" style="display:none;">
		<div class="modal">
			<div class="modal-title">
				모달 타이틀
			</div>
			<p> 모달 내용 </p>
			<div class="close-wrapper">
				<button id="close">닫기</button>
			</div>
		</div>
	</div>
	<script src="src/index.js">
	</script>
</body>
</html>
{% endraw %}  

```

<br>

## styles.css

```css

{% raw %}  
body {
  font-family: sans-serif;
}

.modal-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  background: white;
  padding: 24px 16px;
  border-radius: 4px;
  width: 320px;
}

.modal-title {
  font-size: 24px;
  font-weight: bold;
}

.close-wrapper {
  text-align: right;
}
{% endraw %}  

```

<br>

## index.js

```javascript

import "./styles.css";

const open = document.getElementById("open");
const close = document.getElementById("close");
const modal = document.querySelector(".modal-wrapper");

open.onclick = () => {
  modal.style.display = "flex";
};

close.onclick = () => {
  modal.style.display = "none";
};

```

