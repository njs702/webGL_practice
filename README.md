# WebGL Tutorial - make a simple dice

이 튜토리얼에서는, WebGL을 사용하여 간단한 주사위를 돌리는 그래픽을 단계별로 구현하였다. 수업 시간에 다루었던 make CUBE, View & projection, depth test, texture mapping을 순차적으로 진행하며 간단한 주사위가 돌아가는 프로그램을 만드는 것이 이 튜토리얼의 목적이다.

index.html과 script.js 파일의 skeleton code로 이환용 교수님의 webgl-1.0의 T11.js와 index.html을 사용하였다.

+ 첫 번째 단계 : Make CUBE
  + Primitive - Triangle <br/>
  ![파이프라인](https://git.ajou.ac.kr/Junseok/webgl-tutorial/uploads/0a0e95c3d6ec1dd66defe62654bcd365/%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8.PNG) <br/>
  CUBE를 제작하기 위해서는 우선 전체 파이프라인 과정 중 Primitive processing 과정을 거쳐야 한다. 점, 선, 삼각형 중 하나만 선택하여 그릴 수 있기 때문에 우리는
  큐브를 제작하기 위해 한 면을 삼각형 두개로 구성할 것이다.<br/><br/>
 
 Make vertexData of CUBE

      var vertexData = [
		// Backface (RED/WHITE) -> z = 0.5
        -0.5, -0.5, -0.5,  1.0, 0.0, 0.0, 1.0,
         0.5,  0.5, -0.5,  1.0, 0.0, 0.0, 1.0,
         0.5, -0.5, -0.5,  1.0, 0.0, 0.0, 1.0,
        -0.5, -0.5, -0.5,  1.0, 0.0, 0.0, 1.0,
        -0.5,  0.5, -0.5,  1.0, 0.0, 0.0, 1.0,
         0.5,  0.5, -0.5,  1.0, 1.0, 1.0, 1.0, 
		// Front (BLUE/WHITE) -> z = 0.5
        -0.5, -0.5,  0.5,  0.0, 0.0, 1.0, 1.0,
         0.5,  0.5,  0.5,  0.0, 0.0, 1.0, 1.0,
         0.5, -0.5,  0.5,  0.0, 0.0, 1.0, 1.0,
        -0.5, -0.5,  0.5,  0.0, 0.0, 1.0, 1.0,
        -0.5,  0.5,  0.5,  0.0, 0.0, 1.0, 1.0,
         0.5,  0.5,  0.5,  1.0, 1.0, 1.0, 1.0, 
		// LEFT (GREEN/WHITE) -> z = 0.5
        -0.5, -0.5, -0.5,  0.0, 1.0, 0.0, 1.0,
        -0.5,  0.5,  0.5,  0.0, 1.0, 0.0, 1.0,
        -0.5,  0.5, -0.5,  0.0, 1.0, 0.0, 1.0,
        -0.5, -0.5, -0.5,  0.0, 1.0, 0.0, 1.0,
        -0.5, -0.5,  0.5,  0.0, 1.0, 0.0, 1.0,
        -0.5,  0.5,  0.5,  0.0, 1.0, 1.0, 1.0, 
		// RIGHT (YELLOW/WHITE) -> z = 0.5
         0.5, -0.5, -0.5,  1.0, 1.0, 0.0, 1.0,
         0.5,  0.5,  0.5,  1.0, 1.0, 0.0, 1.0,
         0.5,  0.5, -0.5,  1.0, 1.0, 0.0, 1.0,
         0.5, -0.5, -0.5,  1.0, 1.0, 0.0, 1.0,
         0.5, -0.5,  0.5,  1.0, 1.0, 0.0, 1.0,
         0.5,  0.5,  0.5,  1.0, 1.0, 1.0, 1.0, 
		// BOTTON (MAGENTA/WHITE) -> z = 0.5
        -0.5, -0.5, -0.5,  1.0, 0.0, 1.0, 1.0,
         0.5, -0.5,  0.5,  1.0, 0.0, 1.0, 1.0,
         0.5, -0.5, -0.5,  1.0, 0.0, 1.0, 1.0,
        -0.5, -0.5, -0.5,  1.0, 0.0, 1.0, 1.0,
        -0.5, -0.5,  0.5,  1.0, 0.0, 1.0, 1.0,
         0.5, -0.5,  0.5,  1.0, 1.0, 1.0, 1.0, 
		// TOP (CYAN/WHITE) -> z = 0.5
        -0.5,  0.5, -0.5,  0.0, 1.0, 1.0, 1.0,
         0.5,  0.5,  0.5,  0.0, 1.0, 1.0, 1.0,
         0.5,  0.5, -0.5,  0.0, 1.0, 1.0, 1.0,
        -0.5,  0.5, -0.5,  0.0, 1.0, 1.0, 1.0,
        -0.5,  0.5,  0.5,  0.0, 1.0, 1.0, 1.0,
         0.5,  0.5,  0.5,  1.0, 1.0, 1.0, 1.0 ];

CUBE 제작을 위한 뼈대 코드로 이환용 교수님의 webgl-1.0 : TO6_final.js 를 참고하였다. <br/><br/>
*생성된 큐브의 모습<br/>
![큐브생성](https://git.ajou.ac.kr/Junseok/webgl-tutorial/uploads/72074b7873e62fc0f7830fec912d8e57/%ED%81%90%EB%B8%8C%EC%83%9D%EC%84%B1.PNG)

+ 두 번째 단계 : Mapping texture image
면에 텍스쳐 이미지 입히기 <br/>
```
var vertexData = [
		// Backface (RED/WHITE) -> z = 0.5 , 1
        -0.5, -0.5, -0.5,  1.0, 0.0, 0.0, opacity1,  0.0,  0.0,
         0.5,  0.5, -0.5,  1.0, 0.0, 0.0, opacity1,  1.0,  1.0,
         0.5, -0.5, -0.5,  1.0, 0.0, 0.0, opacity1,  1.0,  0.0,
        -0.5, -0.5, -0.5,  1.0, 0.0, 0.0, opacity2,  0.0,  0.0,
        -0.5,  0.5, -0.5,  1.0, 0.0, 0.0, opacity2,  0.0,  0.0,
         0.5,  0.5, -0.5,  1.0, 0.0, 0.0, opacity2,  0.0,  0.0, 
		// Front (BLUE/WHITE) -> z = 0.5  , 2     
        -0.5, -0.5,  0.5,  0.0, 0.0, 1.0, opacity1,  0.0,  0.0,
         0.5, -0.5,  0.5,  0.0, 0.0, 1.0, opacity1,  1.0,  0.0,
		 0.5,  0.5,  0.5,  0.0, 0.0, 1.0, opacity1,  1.0,  1.0,
        -0.5, -0.5,  0.5,  0.0, 0.0, 1.0, opacity2,  0.0,  0.0,
         0.5,  0.5,  0.5,  0.0, 0.0, 1.0, opacity2,  1.0,  1.0, 
		-0.5,  0.5,  0.5,  0.0, 0.0, 1.0, opacity2,  0.0,  1.0,
		// LEFT (GREEN/WHITE) -> z = 0.5   ,3                    
        -0.5, -0.5, -0.5,  0.0, 1.0, 0.0, opacity2,  0.0,  0.0,
        -0.5,  0.5,  0.5,  0.0, 1.0, 0.0, opacity2,  2.0,  2.0,
        -0.5,  0.5, -0.5,  0.0, 1.0, 0.0, opacity2,  2.0,  0.0,
        -0.5, -0.5, -0.5,  0.0, 1.0, 0.0, opacity1,  0.0,  0.0,
        -0.5, -0.5,  0.5,  0.0, 1.0, 0.0, opacity1,  0.0,  0.0,
        -0.5,  0.5,  0.5,  0.0, 1.0, 0.0, opacity1,  0.0,  0.0, 
		// RIGHT (YELLOW/WHITE) -> z = 0.5   ,4      
         0.5, -0.5, -0.5,  1.0, 1.0, 0.0, opacity1,  0.0,  0.0,
         0.5,  0.5, -0.5,  1.0, 1.0, 0.0, opacity1,  2.0,  0.0,
		 0.5,  0.5,  0.5,  1.0, 1.0, 0.0, opacity1,  2.0,  2.0,
         0.5, -0.5, -0.5,  1.0, 1.0, 0.0, opacity2,  0.0,  0.0,
         0.5,  0.5,  0.5,  1.0, 1.0, 0.0, opacity2,  2.0,  2.0, 
		 0.5, -0.5,  0.5,  1.0, 1.0, 0.0, opacity2,  0.0,  2.0,
		// BOTTON (MAGENTA/WHITE) -> z = 0.5   ,5                
        -0.5, -0.5, -0.5,  1.0, 0.0, 1.0, opacity1,  0.0,  0.0,
         0.5, -0.5, -0.5,  1.0, 0.0, 1.0, opacity1,  2.0,  0.0,
		 0.5, -0.5,  0.5,  1.0, 0.0, 1.0, opacity1,  2.0,  2.0,
        -0.5, -0.5, -0.5,  1.0, 0.0, 1.0, opacity2,  0.0,  0.0,
         0.5, -0.5,  0.5,  1.0, 0.0, 1.0, opacity2,  2.0,  2.0, 
		-0.5, -0.5,  0.5,  1.0, 0.0, 1.0, opacity2,  0.0,  2.0,
		// TOP (CYAN/WHITE) -> z = 0.5          ,6              
        -0.5,  0.5, -0.5,  0.0, 1.0, 1.0, opacity2,  0.0,  0.0,
         0.5,  0.5,  0.5,  0.0, 1.0, 1.0, opacity2,  3.0,  3.0,
         0.5,  0.5, -0.5,  0.0, 1.0, 1.0, opacity2,  3.0,  0.0,
        -0.5,  0.5, -0.5,  0.0, 1.0, 1.0, opacity1,  0.0,  0.0,
        -0.5,  0.5,  0.5,  0.0, 1.0, 1.0, opacity1,  0.0,  3.0,
         0.5,  0.5,  0.5,  0.0, 1.0, 1.0, opacity1,  3.0,  3.0 ];
```

  텍스쳐를 사용하기 전에 텍스쳐의 좌표와 정육면체의 면의 정점을 매핑 시켜줘야 한다.<br/>
  매칭된 vertexdata를 활용하여 image texture를 입힌다.<br>
  ```
  var tex1, tex2;

function initialiseBuffer() {

    gl.vertexBuffer = gl.createBuffer();
    gl.bindBuffer(gl.ARRAY_BUFFER, gl.vertexBuffer);
    gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(vertexData), gl.DYNAMIC_DRAW);
	
	tex2 = gl.createTexture(); 
	var image = new Image();
	image.src = "dot.png";
	image.addEventListener('load', function() {
		// Now that the image has loaded make copy it to the texture.
		gl.bindTexture(gl.TEXTURE_2D, tex2);
		gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGBA, gl.RGBA,gl.UNSIGNED_BYTE, image);
		// It must be inside of EventListener
		gl.generateMipmap(gl.TEXTURE_2D);
		gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.LINEAR);
		gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MAG_FILTER, gl.LINEAR);
		gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_S, gl.REPEAT);
		gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_T, gl.REPEAT);
		});
    return testGLError("initialiseBuffers");
}
  ```
초기 initialiseBuffer<br/><br/>
*사용된 이미지 리소스<br/>
![dot](/uploads/f21f68fe05e8c8fb21ba4a889eeae0ec/dot.png)<br/>

```
var fragmentShaderSource = `
			varying highp vec4 col; 
			varying highp vec2 uv; 
			uniform sampler2D tex1;
			uniform sampler2D tex2;
			void main(void) 
			{ 
				gl_FragColor = 0.3 * col + 0.3 * texture2D(tex1, uv) + 0.4 * texture2D(tex2, uv);
			}`;
```
initialise fragmentshader
```
var vertexShaderSource = `
			attribute highp vec4 myVertex; 
			attribute highp vec4 myColor; 
			attribute highp vec2 myUV; 
			uniform mediump mat4 mMat; 
			uniform mediump mat4 vMat; 
			uniform mediump mat4 pMat; 
			varying  highp vec4 col;
			varying  highp vec2 uv;
			void main(void)  
			{ 
				gl_Position = pMat * vMat * mMat * myVertex; 
				gl_PointSize = 8.0;
				col = myColor; 
				uv = myUV;
			}`;
```
initialise vertexshader<br/><br/>
*텍스쳐 이미지가 매핑된 큐브의 모습<br/>
![주사위텍스쳐입힘](/uploads/967b540c719ef52b5c971745b09062d8/주사위텍스쳐입힘.PNG)<br/>

+ 세 번째 단계 : 주사위 굴리기(Rotate the dice)<br/>
주사위의 형체를 만들었으니, 이제는 주사위를 굴려야 할 차례다.<br/>
주사위의 랜덤 확률을 고려하여, 임의의 난수를 생성해 주사위의 Rotate speed를 설정했다.<br/>
```
const rand_0_9 = Math.random();
speedRot = rand_0_9;
```
먼저 임의의 난수를 생성한다(0~1).<br/>
```
function fn_load_dice(){
    
    speedRot = speedRot * 0.97;
    if(speedRot < 0.000000001){
        flag_animation = 0;
    }
    else{
        flag_animation = 1;
    }
    
}
```
주사위가 돌아가는 속도인 speedRot에 곱연산을 통해 그 속도를 줄여나간다. 속도가 일정 수준 이상으로 줄어들면 큐브가 멈추게 된다.
rendersence() 함수 내부에 fn_load_dice();를 호출해 계속해서 반복해 수행하다가, 속도가 줄어들면 큐브가 멈추게 구현했다.<br/><br/>
*주사위 돌아가는 모습<br/>
![뤀앳x](/uploads/c00f952179a34590a598031d4bb841df/뤀앳x.gif)
  
+ 마지막 단계 : 주사위 굴리기 효과 구현(rolling the dice)<br/>
주사위를 직접 우리가 굴리는 효과를 구현하기 위해, webGL의 view, projection 개념을 적용시켰다.<br/>
![모질라lookat](/uploads/fb9b8626cdc1910cd6982c4b594d3851/모질라lookat.PNG)<br/>
우리가 바라보는 눈, 물체, upvector를 사용한다면 물체의 이동 효과를 느낄 수 있다. 만약 우리의 눈의 위치가 위로 올라간다면, 물체는 아래로 떨어지는 듯한 효과를 얻을 수 있을 것이다.<br/>
![lookat](/uploads/8202c8c30caf6e981b4fcda6aa552075/lookat.PNG)<br/>
```
mat4.lookAt(vMat, [0,y_cam,2], [0.0 ,0.0, 0.0], [0,1,0]);
    if(y_cam < 3.0){
        y_cam = y_cam + 0.3;
    }
```
renderscene() 함수 안에서, lookAt의 y_cam변수를 매 단계마다 0.3씩 증가하도록 했다. 즉 우리 눈이 점점 0.3크기만큼 올라간다는 뜻이다. 그렇게 되면 물체는 반대로 0.3씩 아래로 내려가 보이는 효과를 보일 것이다.<br/>
주사위가 컴퓨터 화면에서 사라지지 않도록 하기 위해 3.0의 마지노선을 두었다.<br/><br/>
*주사위 굴리는 모습<br/>
![뤀앳o](/uploads/fefb1bf43c9d97a5fe5bc45deb3373c0/뤀앳o.gif)<br/><br/>

# 참조
https://www.youtube.com/watch?v=uVDZton_TwU&list=PLKseYrrlvWNqmtCMZyoraXIAG2F0sG2o7&index=4 <br/>
https://glmatrix.net/docs/module-mat4.html <br/>
https://github.com/hwan-ajou/webgl-1.0 <br/>
https://developer.mozilla.org/ko/docs/Web/API/WebGL_API/Tutorial/Using_textures_in_WebGL <br/>
