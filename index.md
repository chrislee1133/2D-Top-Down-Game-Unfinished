 <html> 
		<head> 
<title></title> 
<link href="./2D top-down example.css" rel="stylesheet" type="text/css">
</head> 
<body> 
<div class="mainMenu">
	<center>
	<canvas id="myCanvas" width="480" height="320"><p>Your browser does not support HTML5!</p></canvas> 
	</center>
<script type="text/javascript">
	var canvas = document.getElementById("myCanvas");
	var context = canvas.getContext("2d");
	
	var width = canvas.getAttribute('width');
	var height = canvas.getAttribute('height');
	
	var mouseX;
	var mouseY;
	
	var bgImage = new Image();
	var logoImage = new Image();
	var playImage = new Image();
	var instructImage = new Image();
	var settingsImage = new Image();
	var creditsImage = new Image();
	var shipImage = new Image();

	var backgroundY = 0;
	var speed = 1;
	
	var buttonX = [192,110,149,160];
	var buttonY = [100,140,180,220];
	var buttonWidth = [96,260,182,160];
	var buttonHeight = [40,40,40,40];
	
	var shipX = [0,0];
	var shipY = [0,0];
	var shipWidth = 35;
	var shipHeight = 40;
	
	var shipVisible = false;
	var shipSize = shipWidth;
	var shipRotate = 0;
	
	var frames = 30;
    var timerId = 0;
	var fadeId = 0;
	var time = 0.0;

	shipImage.src = "sword.png";
	
	
	bgImage.onload = function(){
		context.drawImage(bgImage, 480, 320);
	};
	bgImage.src = "menubackgroundresized.png";
	
	
	<!-- logoImage.onload = function(){ -->
		<!-- context.drawImage(logoImage, 50, -10); -->
	<!-- } -->
	<!-- logoImage.src = "logo.png"; -->
	
	
	playImage.onload = function(){
		context.drawImage(playImage, buttonX[0], buttonY[0]);
	}
	playImage.src = "play.png";
	
	
	instructImage.onload = function(){
		context.drawImage(instructImage, buttonX[1], buttonY[1]);
	}
	instructImage.src = "instructions.png";
	
	
	settingsImage.onload = function(){
		context.drawImage(settingsImage, buttonX[2], buttonY[2]);
	}
	settingsImage.src = "settings.png";
	
	
	creditsImage.onload = function(){
		context.drawImage(creditsImage, buttonX[3], buttonY[3]);
	}
	creditsImage.src = "credits.png";
	
	
	
	timerId = setInterval("update()", 1000/frames);
	
	canvas.addEventListener("mousemove", checkPos);
	canvas.addEventListener("mouseup", checkClick);
	
	function update() {
		clear();
		move();
		draw();
	}
	function clear() {
		context.clearRect(0, 0, width, height);
	}
	function move(){
		<!-- backgroundY -= speed; -->
		<!-- if(backgroundY == -1 * height){ -->
			<!-- backgroundY = 0; -->
		<!-- } -->
		if(shipSize == shipWidth){
			shipRotate = -1;
		}
		if(shipSize == 0){
			shipRotate = 1;
		}
		shipSize += shipRotate;
	}
	function draw(){
		context.drawImage(bgImage, 0, backgroundY);
		context.drawImage(logoImage, 50,-10);
		context.drawImage(playImage, buttonX[0], buttonY[0]);
		context.drawImage(instructImage, buttonX[1], buttonY[1]);
		context.drawImage(settingsImage, buttonX[2], buttonY[2]);
		context.drawImage(creditsImage, buttonX[3], buttonY[3]);
		if(shipVisible == true){
			context.drawImage(shipImage, shipX[0] - (shipSize/2), shipY[0], shipSize, shipHeight);
			context.drawImage(shipImage, shipX[1] - (shipSize/2), shipY[1], shipSize, shipHeight);
		}
	}
	function checkPos(mouseEvent){
		if(mouseEvent.pageX || mouseEvent.pageY == 0){
			mouseX = mouseEvent.pageX - this.offsetLeft;
			mouseY = mouseEvent.pageY - this.offsetTop;
		}else if(mouseEvent.offsetX || mouseEvent.offsetY == 0){
			mouseX = mouseEvent.offsetX;
			mouseY = mouseEvent.offsetY;
		}
		for(i = 0; i < buttonX.length; i++){
			if(mouseX > buttonX[i] && mouseX < buttonX[i] + buttonWidth[i]){
				if(mouseY > buttonY[i] && mouseY < buttonY[i] + buttonHeight[i]){
					shipVisible = true;
					shipX[0] = buttonX[i] - (shipWidth/2) - 2;
					shipY[0] = buttonY[i] + 2;
					shipX[1] = buttonX[i] + buttonWidth[i] + (shipWidth/2); 
					shipY[1] = buttonY[i] + 2;
				}
			}else{
				shipVisible = false;
			}
		}
	}
	function checkClick(mouseEvent){
		for(i = 0; i < buttonX.length; i++){
			if(mouseX > buttonX[i] && mouseX < buttonX[i] + buttonWidth[i]){
				if(mouseY > buttonY[i] && mouseY < buttonY[i] + buttonHeight[i]){
					fadeId = setInterval("fadeOut()", 1000/frames);
					clearInterval(timerId);
					canvas.removeEventListener("mousemove", checkPos);
					canvas.removeEventListener("mouseup", checkClick);
				}
			}
		}
	}
	function changeScore() {
	document.getElementById("myCanvas").style.zIndex = "-6";
	}
	
	
	function fadeOut(){
		context.fillStyle = "rgba(0,0,0, 0.2)";
		context.fillRect (0, 0, width, height);
		time += 0.1;
		if(time >= 2){
			clearInterval(fadeId);
			time = 0;
			timerId = setInterval("update()", 1000/frames);
			canvas.addEventListener("mousemove", checkPos);
			canvas.addEventListener("mouseup", checkClick);
			changeScore();
		}
	}
	
	</script> 
</div>
<div class="frame">
   <div class="corner_topleft"></div>
   <div class="corner_topright"></div>
   <div class="corner_bottomleft"></div>
   <div class="corner_bottomright"></div>
   
   
   <div class="camera">
      <div class="map pixel-art">
         <div class="character" facing="down" walking="true">
            <div class="shadow pixel-art"></div>
            <div class="character_spritesheet pixel-art"></div>
         </div>
      </div>

		<!-- div classes and svg converters for the d-pad buttons at the bottom of the screen -->
      <div class="dpad">
         <div class="DemoDirectionUI flex-center">
               <button class="dpad-button dpad-left">
                   <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 -0.5 13 13" shape-rendering="crispEdges">
                       <path class="Arrow_outline-top"  stroke="#5f5f5f" d="M1 0h11M0 1h1M12 1h1M0 2h1M12 2h1M0 3h1M12 3h1M0 4h1M12 4h1M0 5h1M12 5h1M0 6h1M12 6h1M0 7h1M12 7h1M0 8h1M12 8h1" />
                       <path class="Arrow_surface" stroke="#f5f5f5" d="M1 1h11M1 2h11M1 3h5M7 3h5M1 4h4M7 4h5M1 5h3M7 5h5M1 6h4M7 6h5M1 7h5M7 7h5M1 8h11" />
                       <path class="Arrow_arrow-inset"  stroke="#434343" d="M6 3h1M5 4h1M4 5h1" />
                       <path class="Arrow_arrow-body" stroke="#5f5f5f" d="M6 4h1M5 5h2M5 6h2M6 7h1" />
                       <path class="Arrow_outline-bottom" stroke="#434343" d="M0 9h1M12 9h1M0 10h1M12 10h1M0 11h1M12 11h1M1 12h11" />
                       <path class="Arrow_edge" stroke="#ffffff" d="M1 9h11" />
                       <path class="Arrow_front" stroke="#cccccc" d="M1 10h11M1 11h11" />
                   </svg>
               </button>
               <button class="dpad-button dpad-up">
                   <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 -0.5 13 13" shape-rendering="crispEdges">
                       <path class="Arrow_outline-top"  stroke="#5f5f5f" d="M1 0h11M0 1h1M12 1h1M0 2h1M12 2h1M0 3h1M12 3h1M0 4h1M12 4h1M0 5h1M12 5h1M0 6h1M12 6h1M0 7h1M12 7h1M0 8h1M12 8h1" />
                       <path class="Arrow_surface" stroke="#f5f5f5" d="M1 1h11M1 2h11M1 3h11M1 4h5M7 4h5M1 5h4M8 5h4M1 6h3M9 6h3M1 7h11M1 8h11" />
                       <path class="Arrow_arrow-inset"  stroke="#434343" d="M6 4h1M5 5h1M7 5h1" />
                       <path class="Arrow_arrow-body" stroke="#5f5f5f" d="M6 5h1M4 6h5" />
                       <path class="Arrow_outline-bottom" stroke="#434343" d="M0 9h1M12 9h1M0 10h1M12 10h1M0 11h1M12 11h1M1 12h11" />
                       <path class="Arrow_edge" stroke="#ffffff" d="M1 9h11" />
                       <path class="Arrow_front" stroke="#cccccc" d="M1 10h11M1 11h11" />
                   </svg>
               </button>
               <button class="dpad-button dpad-down">
                   <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 -0.5 13 13" shape-rendering="crispEdges">
                       <path class="Arrow_outline-top" stroke="#5f5f5f" d="M1 0h11M0 1h1M12 1h1M0 2h1M12 2h1M0 3h1M12 3h1M0 4h1M12 4h1M0 5h1M12 5h1M0 6h1M12 6h1M0 7h1M12 7h1M0 8h1M12 8h1" />
                       <path class="Arrow_surface" stroke="#f5f5f5" d="M1 1h11M1 2h11M1 3h11M1 4h3M9 4h3M1 5h4M8 5h4M1 6h5M7 6h5M1 7h11M1 8h11" />
                       <path class="Arrow_arrow-inset" stroke="#434343" d="M4 4h5" />
                       <path class="Arrow_arrow-body" stroke="#5f5f5f" d="M5 5h3M6 6h1" />
                       <path class="Arrow_outline-bottom" stroke="#434343" d="M0 9h1M12 9h1M0 10h1M12 10h1M0 11h1M12 11h1M1 12h11" />
                       <path class="Arrow_edge" stroke="#ffffff" d="M1 9h11" />
                       <path class="Arrow_front" stroke="#cccccc" d="M1 10h11M1 11h11" />
                   </svg>
               </button>
               <button class="dpad-button dpad-right">
                   <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 -0.5 13 13" shape-rendering="crispEdges">
                       <path class="Arrow_outline-top"  stroke="#5f5f5f" d="M1 0h11M0 1h1M12 1h1M0 2h1M12 2h1M0 3h1M12 3h1M0 4h1M12 4h1M0 5h1M12 5h1M0 6h1M12 6h1M0 7h1M12 7h1M0 8h1M12 8h1" />
                       <path class="Arrow_surface" stroke="#f5f5f5" d="M1 1h11M1 2h11M1 3h5M7 3h5M1 4h5M8 4h4M1 5h5M9 5h3M1 6h5M8 6h4M1 7h5M7 7h5M1 8h11" />
                       <path class="Arrow_arrow-inset"  stroke="#434343" d="M6 3h1M7 4h1M8 5h1" />
                       <path class="Arrow_arrow-body" stroke="#5f5f5f" d="M6 4h1M6 5h2M6 6h2M6 7h1" />
                       <path class="Arrow_outline-bottom" stroke="#434343" d="M0 9h1M12 9h1M0 10h1M12 10h1M0 11h1M12 11h1M1 12h11" />
                       <path class="Arrow_edge" stroke="#ffffff" d="M1 9h11" />
                       <path class="Arrow_front" stroke="#cccccc" d="M1 10h11M1 11h11" />
                   </svg>
               </button>
           </div>
      </div>
      
      <!-- a pixel png converter for the "Top Down Camera" text near the to of the screen -->
      <svg class="headline" xmlns="http://www.w3.org/2000/svg" viewBox="0 -0.5 75 14" shape-rendering="crispEdges">
<metadata>Made with Pixels to Svg https://codepen.io/shshaw/pen/XbxvNj</metadata>
<path stroke="rgba(128,191,255,0.01568627450980392)" d="M0 0h1M74 0h1M0 13h1M74 13h1" />
<path stroke="#5f5f5f" d="M1 0h73M0 1h1M74 1h1M0 2h1M74 2h1M0 3h1M74 3h1M0 4h1M74 4h1M0 5h1M74 5h1M0 6h1M74 6h1M0 7h1M74 7h1M0 8h1M74 8h1M0 9h1M74 9h1M0 10h1M74 10h1M0 11h1M74 11h1" />
<path stroke="#f5f5f5" d="M1 1h73M1 2h73M1 3h3M7 3h12M21 3h22M45 3h29M1 4h4M6 4h13M20 4h1M22 4h20M43 4h2M46 4h28M1 5h4M6 5h2M10 5h2M15 5h4M20 5h2M23 5h2M27 5h2M30 5h1M32 5h1M34 5h1M38 5h4M43 5h5M50 5h2M56 5h3M61 5h3M66 5h1M69 5h5M1 6h4M6 6h1M8 6h2M11 6h1M13 6h2M16 6h3M20 6h2M23 6h1M25 6h2M28 6h1M30 6h1M32 6h1M34 6h1M36 6h2M39 6h3M43 6h7M51 6h1M53 6h1M55 6h1M57 6h1M59 6h2M62 6h1M64 6h5M70 6h4M1 7h4M6 7h1M8 7h2M11 7h1M13 7h2M16 7h3M20 7h2M23 7h1M25 7h2M28 7h1M30 7h1M32 7h1M34 7h1M36 7h2M39 7h3M43 7h5M51 7h1M53 7h1M55 7h1M57 7h1M62 7h1M64 7h3M70 7h4M1 8h4M6 8h1M8 8h2M11 8h1M13 8h2M16 8h3M20 8h1M22 8h2M25 8h2M28 8h1M30 8h1M32 8h1M34 8h1M36 8h2M39 8h3M43 8h2M46 8h1M48 8h2M51 8h1M53 8h1M55 8h1M57 8h1M59 8h4M64 8h2M67 8h2M70 8h4M1 9h4M6 9h2M10 9h2M15 9h4M21 9h4M27 9h3M34 9h1M36 9h2M39 9h4M45 9h3M51 9h1M53 9h1M55 9h1M57 9h2M62 9h1M64 9h3M70 9h4M1 10h11M13 10h61M1 11h11M13 11h61" />
<path stroke="#323234" d="M4 3h3M19 3h2M43 3h2M5 4h1M19 4h1M21 4h1M42 4h1M45 4h1M5 5h1M8 5h2M12 5h3M19 5h1M22 5h1M25 5h2M29 5h1M31 5h1M33 5h1M35 5h3M42 5h1M48 5h2M52 5h4M59 5h2M64 5h2M67 5h2M5 6h1M7 6h1M10 6h1M12 6h1M15 6h1M19 6h1M22 6h1M24 6h1M27 6h1M29 6h1M31 6h1M33 6h1M35 6h1M38 6h1M42 6h1M50 6h1M52 6h1M54 6h1M56 6h1M58 6h1M61 6h1M63 6h1M69 6h1M5 7h1M7 7h1M10 7h1M12 7h1M15 7h1M19 7h1M22 7h1M24 7h1M27 7h1M29 7h1M31 7h1M33 7h1M35 7h1M38 7h1M42 7h1M48 7h3M52 7h1M54 7h1M56 7h1M58 7h4M63 7h1M67 7h3M5 8h1M7 8h1M10 8h1M12 8h1M15 8h1M19 8h1M21 8h1M24 8h1M27 8h1M29 8h1M31 8h1M33 8h1M35 8h1M38 8h1M42 8h1M45 8h1M47 8h1M50 8h1M52 8h1M54 8h1M56 8h1M58 8h1M63 8h1M66 8h1M69 8h1M5 9h1M8 9h2M12 9h3M19 9h2M25 9h2M30 9h4M35 9h1M38 9h1M43 9h2M48 9h3M52 9h1M54 9h1M56 9h1M59 9h3M63 9h1M67 9h3M12 10h1M12 11h1" />
<path stroke="#434343" d="M0 12h1M74 12h1M1 13h73" />
<path stroke="#cccccc" d="M1 12h73" />
</svg>
      
      
      
   </div>
</div>

<script type="application/javascript">
	var character = document.querySelector(".character");
var map = document.querySelector(".map");

//start in the middle of the map
var x = 90;
var y = 34;
var held_directions = []; //State of which arrow keys we are holding down
var speed = 1; //How fast the character moves in pixels per frame

const placeCharacter = () => {
   
   var pixelSize = parseInt(
      getComputedStyle(document.documentElement).getPropertyValue('--pixel-size')
   );
   
   const held_direction = held_directions[0];
   if (held_direction) {
      if (held_direction === directions.right) {x += speed;}
      if (held_direction === directions.left) {x -= speed;}
      if (held_direction === directions.down) {y += speed;}
      if (held_direction === directions.up) {y -= speed;}
      character.setAttribute("facing", held_direction);
   }
   character.setAttribute("walking", held_direction ? "true" : "false");
   
   //Limits (gives the illusion of walls)
   var leftLimit = -8;
   var rightLimit = (16 * 11.5)+2;
   var topLimit = -26;
   var bottomLimit = (16 * 8);
   if (x < leftLimit) { x = leftLimit; }
   if (x > rightLimit) { x = rightLimit; }
   if (y < topLimit) { y = topLimit; }
   if (y > bottomLimit) { y = bottomLimit; }
   
   
   var camera_left = pixelSize * 66;
   var camera_top = pixelSize * 42;
   
   map.style.transform = `translate3d( ${-x*pixelSize+camera_left}px, ${-y*pixelSize+camera_top}px, 0 )`;
   character.style.transform = `translate3d( ${x*pixelSize}px, ${y*pixelSize}px, 0 )`;  
}


//Set up the game loop
const step = () => {
   placeCharacter();
   window.requestAnimationFrame(() => {
      step();
   })
}
step(); //kick off the first step!



/* Direction key state */
const directions = {
   up: "up",
   down: "down",
   left: "left",
   right: "right",
}
const keys = {
   38: directions.up,
   37: directions.left,
   39: directions.right,
   40: directions.down,
}
document.addEventListener("keydown", (e) => {
   var dir = keys[e.which];
   if (dir && held_directions.indexOf(dir) === -1) {
      held_directions.unshift(dir)
   }
})

document.addEventListener("keyup", (e) => {
   var dir = keys[e.which];
   var index = held_directions.indexOf(dir);
   if (index > -1) {
      held_directions.splice(index, 1)
   }
});



/* BONUS! Dpad functionality for mouse and touch */
var isPressed = false;
const removePressedAll = () => {
   document.querySelectorAll(".dpad-button").forEach(d => {
      d.classList.remove("pressed")
   })
}
document.body.addEventListener("mousedown", () => {
   console.log('mouse is down')
   isPressed = true;
})
document.body.addEventListener("mouseup", () => {
   console.log('mouse is up')
   isPressed = false;
   held_directions = [];
   removePressedAll();
})
const handleDpadPress = (direction, click) => {   
   if (click) {
      isPressed = true;
   }
   held_directions = (isPressed) ? [direction] : []
   
   if (isPressed) {
      removePressedAll();
      document.querySelector(".dpad-"+direction).classList.add("pressed");
   }
}
//Bind a ton of events for the dpad
document.querySelector(".dpad-left").addEventListener("touchstart", (e) => handleDpadPress(directions.left, true));
document.querySelector(".dpad-up").addEventListener("touchstart", (e) => handleDpadPress(directions.up, true));
document.querySelector(".dpad-right").addEventListener("touchstart", (e) => handleDpadPress(directions.right, true));
document.querySelector(".dpad-down").addEventListener("touchstart", (e) => handleDpadPress(directions.down, true));

document.querySelector(".dpad-left").addEventListener("mousedown", (e) => handleDpadPress(directions.left, true));
document.querySelector(".dpad-up").addEventListener("mousedown", (e) => handleDpadPress(directions.up, true));
document.querySelector(".dpad-right").addEventListener("mousedown", (e) => handleDpadPress(directions.right, true));
document.querySelector(".dpad-down").addEventListener("mousedown", (e) => handleDpadPress(directions.down, true));

document.querySelector(".dpad-left").addEventListener("mouseover", (e) => handleDpadPress(directions.left));
document.querySelector(".dpad-up").addEventListener("mouseover", (e) => handleDpadPress(directions.up));
document.querySelector(".dpad-right").addEventListener("mouseover", (e) => handleDpadPress(directions.right));
document.querySelector(".dpad-down").addEventListener("mouseover", (e) => handleDpadPress(directions.down));

</script> 

</body>
</html>
