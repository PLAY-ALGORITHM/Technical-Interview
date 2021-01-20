
## 렌더링
- HTML, JavaScript, CSS와 같이 개발자가 작성한 문서를 웹 브라우저에 출력하는 과정


## 렌더링 과정

### 1. Loading
서버에서 HTML 파일을 클라이언트(웹 브라우저)로 보내면 브라우저에서는 로딩을 통해 파일을 읽고, 팝업창을 열지 말지, 다운로드를 받을 지를 결정한다.

### 2. Parsing
HTML파일은 DOM(Document Object Model)로, CSS파일은 CSSOM(CSS Object Model)로 변환된다.
   
   **2-1. HTML -> DOM(Parsing)** 
	   ![enter image description here](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/full-process.png?hl=ko)
	  

 <br>
	   1) Conversion : 브라우저가 HTML의 원시 Bytes를 네트워크에서 읽어와서, 해당 파일에 대해 지정된 인코딩 (ex.UTF-8)에 따라 개별 Characters로 변환<br>
	   2) Tokenizing : 브라우저가 xml파일 형식과 같은 문자열을 토큰으로 변환한다. <br>
	   3) Lexing : 토큰들이 해당 속성 및 규칙을 정의하는 객체로 변환된다.<br>
	   4) DOM construction : HTML마크업이 여러태그 간의 관계를 정의하기 때문에 생성된 객체들은 트리 데이터 구조에 연결된다. 

	   
 <br>
:mag: java script인 script태그를 만나면 스크립트가 해석, 실행되는 동안 파싱은 중단된다. 스타일 시트는 DOM트리를 변경하지 않기 때문에 파싱을 기다리거나 중단하지 않는다고 한다.

	   
<br><br>

**2-2. CSS -> CSSOM**
![enter image description here](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/cssom-tree.png?hl=ko)

브라우저는 페이지의 DOM을 생성하는 동안 외부 CSS 파일을 참조하는 link tag를 접한다. 브라우저는 이 리소스에 대한 요청을 즉시 발송하고 요청의 결과로 아래와 같은 각 태그에 대한 스타일 컨텐츠가 반환된다.

 body { font-size:  16px  }<br>
 p { font-weight: bold }<br>
 span { color: red }<br>
 p span { display: none }<br>
 img {  float: right }<br>


### 3. Render Tree생성


![enter image description here](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/render-tree-construction.png)

CCSOM 과 DOM트리가 결합하여 렌더링 트리를 형성한다. 
렌더링 트리는 표시되는 각 요소의 레이아웃을 계산하는데 사용되며 픽셀을 화면에 렌더링하는 페인트 프로세스에 대한 입력으로 처리된다. 

브라우저가 DOM과 CCSOM을 렌더링 트리에 결합한다. 이 트리는 페이지에 표시되는 모든 DOM 컨텐츠와 각 노드에 대한 CCSOM스타일 정보를 캡쳐한다. 실제 화면에 표현되는 노드들로만 구성된다.
script tag, meta tag, span tag중 display:none과 같은 태그들은 렌더링 트리에서 생략된다.

<br>  
:mag:  visibility:invisible은 렌더링이 되어있지만 보이지 않는 기능, display:none은 요소가 보이지 않으며 레이아웃에 포함되지 않으므로 렌더링 트리에서 제거됨. 따라서 display:none을 사용하는 것이 효율적일 수 있다고 한다.


### Layout
레이아웃 단계에서는 각 노드들의 정확한 위치와 크기를 계산하여 화면의 어느 위치에 노드가 출력될지 계산한다.

viewport(화면 디스플레이상의 표시 영역)는 화면의 크기, 브라우저 창의 크기에 따라 달라진다.

### Paint
레이아웃 계산 완료 후에 요소들을 화면에 그리게 된다. 요소들의 위치와 스타일 계산이 완료된 render tree를 이용해 텍스트, 이미지, 색, 효과등이 처리되어 그려진다. 