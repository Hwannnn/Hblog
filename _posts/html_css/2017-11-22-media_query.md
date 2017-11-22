---
layout: post
title: media query 제대로 사용하기
published: True
categories: 
- html_css
tags:
- html
- css
- media query
- responsive web
---



## media query 이해하기

* 미디어 쿼리를 이용하여 반응형 디자인을 할 때에 보통 min-width, max-width 속성을 많이 사용한다.
* 이번 HTML/CSS 1일차 교육을 듣기 전에는 두 속성 중에 아무거나 선택해서 해상도에 대한 css 정의만 하면 되는 줄 알았다.
* 그런데, 각 속성마다 사용하는 상황이 다르고 미디어 쿼리를 작성하는 순서를 지켜야 한다는 것도 알게 되었다. 
* 결론적으로는 두 속성 중에  어떤 속성을 사용하는지는 _**디자인 가이드**_에 따라 다르다. 



### max-width 이용

* 디자인 가이드가 PC -> Pad -> mobile과 같은 순서로 나왔다면 max-width 속성을 사용하고 미디어 쿼리를 작성할 때는 max-width가 큰 순으로 작성한다.
* 처음 PC 화면을 기준으로 시작해서 Pad화면과  mobile 화면 순서로 맞춰가며 미디어 쿼리를 작성한다.
* 즉, 큰 것부터해서 작은 것을 디자인해나가는 것이다.
* 다음은 테스트 실습 코드이다. 궁금하면 저장해서 브라우저에서 어떤 결과가 나오는지 알 수 있다.
<a href="/test_html/media_query_max.html">media_query_test</a>

```html
<!doctype html>
<html>
<head>
	<meta charset="utf-8">
	<title>Media query test</title>
	<style>
		body {
			background-color: black;	
		}

		.header {
			margin: 10px auto;
			width: 1024px;
			background-color: red;
		}

		.container {
			margin: 10px auto;
			width: 1024px;
			background-color: orange;
			overflow: hidden;
		}
			.first-aside {
				background-color: green;
				width: 30%;
				float: left;
			}

			.content {
				background-color: blue;
				width: 40%;	
				float: left;
			}

			.second-aside {
				background-color: navy;
				width: 30%;	
				float: left;
			}

				.section {
					background-color: gray;
				}

		.footer {
			margin: 10px auto;
			width: 1024px;
			background-color: yellow;
		}

		@media(max-width: 780px) {
			.first-aside {
				width: 100%;
				clear: both;
			}

			.content {
				width: 60%;
			}

			.second-aside {
				width: 40%;
			}
		}

		@media(max-width: 380px) {
			.container {
				overflow: auto;
			}

			.content,
			.first-aside,
			.second-aside {
				width: 100%;
			}
      }
	</style>
</head>

<body>
	<div class="header">
		<h1>Header</h1>
	</div>

	<div class="container">
		<div class="first-aside">
			<h2>Hellow World</h2>
		</div>
      
		<div class="content">
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
		</div>

		<div class="second-aside">
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
		</div>
	</div>

	<div class="footer">
		<h2>Footer</h2>
	</div>
</body>
</html>
```



### min-width 이용

- 반대로, 디자인 가이드가 mobile -> Pad -> PC와 같은 순서로 나왔다면 min-width 속성을 사용하고 미디어 쿼리를 작성할 때는 min-width가 작은 순으로 작성한다.
- 처음 mobile 화면을 기준으로 시작하여 Pad화면과 PC 화면에 맞춰가며 미디어 쿼리를 작성한다.
- 즉, 작은 것부터 큰 것을 디자인해나가는 것이다.
- 다음은 테스트 실습 코드이다. 궁금하면 저장해서 브라우저에서 어떤 결과가 나오는지 알 수 있다.
<a href="/test_html/media_query_min.html">media_query_test</a>

```html
<!doctype html>
<html>

<head>
	<meta charset="utf-8">
	<title>Media query test</title>
	<style>
		body {
			background-color: black;	
		}

		.header {
			margin: 10px auto;
			width: 1024px;
			background-color: red;
		}

		.container {
			margin: 10px auto;
			width: 1024px;
			background-color: orange;
		}
			.first-aside {
				background-color: green;
			}

			.content {
				background-color: blue;
			}

			.second-aside {
				background-color: navy;
			}

				.section {
					background-color: gray;
				}

		.footer {
			margin: 10px auto;
			width: 1024px;
			background-color: yellow;
		}


		@media(min-width: 780px) {
			.container {
				overflow: hidden;
			}

			.content {
				width: 60%;
				float: left;
			}

			.second-aside {
				width: 40%;
				float: left;
			}
		}

		@media(min-width: 1024px) {
			.container {
				overflow: hidden;
			}

			.first-aside {
				width: 30%;
				float: left;
			}

			.content {
				width: 40%;	
				float: left;		
			}

			.second-aside {
				width: 30%;
				float: left;
			}
		}
	</style>
</head>

<body>
	<div class="header">
		<h1>Header</h1>
	</div>

	<div class="container">
		<div class="first-aside">
			<h2>Hellow World</h2>
		</div>

		<div class="content">
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
		</div>

		<div class="second-aside">
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
			<div class="section">
				<h3>Section</h3>
			</div>
		</div>
	</div>

	<div class="footer">
		<h2>Footer</h2>
	</div>
</body>
</html>
```



### 실제 자주 사용되는 속성은?

* 두 속성 중에 min-width 속성이 자주 사용한다고 한다.
* 그 이유는 max-width 속성을 사용하는 경우에는 더 주의해야할게 많아지기 떄문이라고 한다.(정확한 이유나 상황은 모르겠다..)
* 생각해보니 작은 것부터 차례대로 디자인해나가는 것이 좋은 방향?인 것 같기도 하다.
