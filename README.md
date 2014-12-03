packingblocks
=============

var interactiveContainer, pageContainer, page0, head, leftSideBar,rightSideBar, reqWidth=9, reqDepth=25, mainContainer, popupContainer, mainMessage, boxesFitHead, fittedItems=new Array(), maxInputBlocks=25, text = "0", eventBroker, saveContainer, validBlocks=0;

var selectedValHeader;

$(document).ready(function(){
	document.onkeydown = function(){
		if(window.event && window.event.keyCode == 9) 
        { // New action for TAB
        	return false; 
		}
	}
	
	
	var table=["<div id=\"blocker\" onclick=\"instruct()\" ></div><div class=\"question\" onclick=\"instruct()\"><div style=\"position:absolute; left:2%; top:3%; opacity:0.6;\" ><div><img src=\"images/btInfoOn.png\" width=\"25px\" height=\"27px\" /></div></div><span style=\"text-align:left; font-family:myFontfamily; left:45px; line-height:1.4em; width:420px; position:absolute; top:45px;\">Choose the number of blocks that you will build boxes for. Enter a number for the width of the box. Enter a number for the depth of the box. Tap the Build button to watch the packing machine build your box. Try to find the number of blocks that can be packaged in the most sizes of boxes.</span><i style=\"font-size: 21px;  float:right; opacity: .6;\" class=\"closeBtn icon icon-emove\" ><strong>&times;</strong></i></div>"];
	
	
	hideTopDiv = $('<div class="hideBb"/>')
	interactiveContainer = $("<div id=\"interactive-container\" />");
	pageContainer = $("<div id=\"page-container\"/>");
	page0 = $("<div id=\"page-0\" class=\"page blockpacking\">");
	head = $("<div class=\"prompt\">Create box designs that fit exactly <span><select onchange=\"testBox();\" id=\"validTBox\" class=\"validBoxField numTxta\" readonly=\"false\" style=\"padding:2px 5px; 2px 5px; border-radius:5px; margin-left:2px; background-color:#D6DDDF;color:#000; width:100px;\"></select></span>  blocks</div>");
	
	
	
	leftSideBar = $("<div class=\"left-sidebar\"><div class=\"build-a-box\">Build a Box</div><table style=\"position:absolute; left:0px; width:230px;\"><tbody><tr class=\"ls-width\" style=\"height:40px;\"><td align=\"center\"><label>Width</label><select onchange=\"valueChange()\" class=\"ls-width-value numTxta\" id=\"widthB\" placeholder=\"pick\" style=\"padding:2px 5px; 2px 5px; border-radius:5px; margin-left:5px; background-color:#D6DDDF;color:#000; width:70px;\" disabled=\"true\"></select></td></tr><tr class=\"ls-depth\" style=\"height:40px;\"><td align=\"center\"><label>Depth </label><select onchange=\"valueChange()\" class=\"ls-depth-value numTxta\" id=\"heightB\"  disabled=\"true\" style=\"padding:2px 5px; 2px 5px; border-radius:5px; margin-left:2px; background-color:#D6DDDF;color:#000; width:70px;\"></select></td></tr><tr><td align=\"center\" colspan=\"2\"><div class=\"pack-button\"></div></td></tr>");
	
	
	//leftSideBar = $("<div class=\"left-sidebar\"><div class=\"build-a-box\">Build a Box</div><table><tbody><tr class=\"ls-width\"><td><label>Width</label><select onchange=\"valueChange()\" class=\"ls-width-value numTxta\" id=\"widthB\" placeholder=\"pick\" style=\"padding:2px 5px; 2px 5px; border-radius:5px; margin-left:5px; background-color:#D6DDDF;color:#000; width:70px;\" disabled=\"true\" ></select></td></tr><tr class=\"ls-depth\"><td><label>Depth </label><select onchange=\"valueChange()\" class=\"ls-depth-value numTxta\" id=\"heightB\"  readonly=\"true\" style=\"padding:2px 5px; 2px 5px; border-radius:5px; margin-left:2px; background-color:#D6DDDF;color:#000; width:70px;\" disabled=\"true\"></select></td></tr><tr><td colspan=\"2\"><div class=\"pack-button\"></div></td></tr>");
	//
	footerContainer = $("<div id=\"menu-container\"><div id=\"navigation-container\"></div><div id=\"tools-container\"><div id=\"tools\"><div style=\"cursor:default;\" id=\"tool-toggle\" class=\"inactive\"><div class=\"btReset\"><div class=\"icoReset\"></div></div></div><div id=\"tool-list-container\" class=\"slideRight\"></div></div><div id=\"score-button-container\"><div  class=\"inactive \"><div id=\"scoreAnim\" class=\"scoreBtn\"><div  id=\"contCorrect\">0 <img src=\"images/IconCorrect.png\"></div><div id=\"contIncorrect\">0 <img src=\"images/IconIncorrect.png\"></div></div></div><div style=\"cursor:default;\" id=\"score-button\" class=\"inactive btFeedback\"><div class=\"icoFeed\"></div></div></div><div id=\"save-button-container\"><div style=\"cursor:default;\" id=\"save-button\" class=\"inactive\"><i class=\"icons icon-save icon-3x\"></i></div></div><div id=\"instructions-button-container\"><div id=\"instructions-button\" onclick=\"openInst()\" class=\"btInfo\"><div class=\"icoInfo\"></div></div>");
	mainContainer = "<div class=\"main\"><div class=\"box main-box\"><div class=\"box-dims\"></div><canvas id=\"box-canvas\" class=\"box-canvas\" width=\"520\" height=\"375\"> </canvas></div><div class=\"ok-button-container\" style=\"display: block; \"><div class=\"ok-button\"></div></div></div>";
	rightSideBar = $("<div class=\"right-sidebar\"></div>");
	popupContainer = '<div id="popup-container" class="popup"><div id="popupwrapper" align="center"><div id="popupContent">If you change the number of blocks, the designs that you created will be deleted. Are you sure you want to change the number of blocks?<div id="popupMsg"></div><div id="okBtnHolder"><img id="okBtn" src="images/button-ok.png"/><img id="noBtn" src="images/button-no.png"/></div></div></div></div>';
	mainMessage = $("<div class=\"main-message\"></div>");
	boxesFitHead = $("<div class=\"shipped-boxes\">Boxes that fit <span id=\"boxesCount\">"+text+"</span> blocks:</div>");
	
	saveContainer = $('<div id="modal-container" class="save"><div class="modal-lightbox"></div><div class="modal-wrapper"><div class="modal"><div class="modal-message"></div><div class="modal-header"><i class="icon icon-save" style="float:left; opacity: .2;"></i><i class="icon close-button" style="font-size: 18px; padding: 7px 4px; float:right; opacity: .6;">×</i></div><div class="modal-body"><h3>Save your work?</h3><div class="button-container"><div class="button save-state-button"><i class="icon icon-ok icon-4x"></i><br>Save</div></div><div class="button-container"><div class="button cancel-button"><i class="icon icon-remove icon-4x"></i><br>Cancel</div></div></div><div class="modal-footer"></div></div></div></div>');
	
	$('body').append(hideTopDiv);
	$('body').append(table[0]);
	page0.append(footerContainer);
	page0.append(head);
	page0.append(leftSideBar);
	page0.append(rightSideBar);
	page0.append(boxesFitHead);
	page0.append(mainContainer);
	page0.append(mainMessage);
	page0.append(popupContainer);
	pageContainer.append(page0);
	interactiveContainer.append(pageContainer);
	$('body').append(interactiveContainer);
	width_Box();height_Box();Number_Box();
	/* Keypad Intialization */        
	numKeypad = new vKeyPad(".numTxt");
	numKeypad.afterNumClick=testBox;
	numKeypad.onTextChanged = validate;
	page0.click(function(event){
		var target = $(event.target);
		if(target.attr('type')!="text")
		   numKeypad.hidePad();
	});
	
	
	$(".icoInfo").on('click',function () {
			toggleClassButton($(this),'icoInfoMove');
			
	})
	
        $(".icoReset").unbind('click',reset);
	
	function width_Box()

	{
		var a=window.document.getElementById("widthB");
		var str1 = "";
		for(i=""; i<=9; i++)

		{
			str1 +=("<option value=\""+i+"\">"+i+"</option>")
		}
		a.innerHTML = str1;
	}
	function height_Box()

	{
		var a=window.document.getElementById("heightB");
		var str1 = "";
		for(i=""; i<=25; i++)

		{
			str1 +=("<option value=\""+i+"\">"+i+"</option>")
		}
		a.innerHTML = str1;
	}
	function Number_Box()

	{
		var a=window.document.getElementById("validTBox");
		var str2 = "";
		for(k=""; k<=25; k++)

		{
			str2 +=("<option value=\""+k+"\">"+k+"</option>")
		}
		a.innerHTML = str2;
	}
	$('.pack-button').click( function (event){
		if(parseInt($(".ls-width-value")[0].value) > 0 && parseInt($(".ls-depth-value")[0].value)>0 && $(".main-message").is(':visible')==false)
		{
			var blockWidth = $(".ls-width-value")[0].value;
			var blockDepth = $(".ls-depth-value")[0].value;
			var blockHeight = 1;
			var blockColor = [[45,129,177],[0,166,210],[100,170,166]];
			drawBlocks(blockWidth, blockDepth, blockHeight, blockColor);
			$(".ls-width-value").attr('disabled', 'disabled');
			$(".ls-depth-value").attr('disabled', 'disabled');
		}
			selectedValHeader = $('#validTBox').val();
	});
	$(".popup").hide();
	$(".ok-button-container").hide();
	$(".main-message").hide();
	$("#okBtn").click(function (event){
		$(".ls-width-value").val($(".ls-width-value option:first").val());
		$(".ls-depth-value").val($(".ls-depth-value option:first").val());
		$(".ls-width-value").removeAttr('disabled');
		$(".ls-depth-value").removeAttr('disabled');
		$(".right-sidebar").html("");
		$(".main-message").html("");
		$(".main-message").hide();
		$(".ok-button-container").hide();
		fittedItems=new Array();
		$("#boxesCount").html("0");
		try{
		window.ctx.setTransform(1,0,0,1,0,0);
		window.ctx.clearRect(0, 0, window.canwidth, window.canheight);
		}catch(e){}
		text = window.document.getElementById("validTBox").selectedIndex;
		
		if(text > 0)
		{
			$('#widthB').removeAttr('disabled');
			$('#heightB').removeAttr('disabled');
			
			if ($(".right-sidebar").children().length > 0 || $(".ok-button-container").is(':visible')) {
				$(".popup").show();
			}
			else {
				$("#boxesCount").html(text);
			}
			
		}
		else{
			$('#widthB').attr('disabled','disabled');
			$('#heightB').attr('disabled','disabled');
		}	
		$(".popup").hide();
	});
	$("#noBtn").click(function (event){
		$(".popup").hide();
		$('#validTBox').val(selectedValHeader);
	});
	$(".ok-button").click(function (event){
		$(".main-message").html("");
		$(".main-message").hide();
		$(".ls-width-value").removeAttr('disabled');
		$(".ls-depth-value").removeAttr('disabled');
		if(window.clearCanvas==true)
		{
			window.ctx.setTransform(1,0,0,1,0,0);
			//window.ctx.scale(1, 1);
			window.ctx.clearRect(0, 0, window.canwidth, window.canheight);
			
			window.clearCanvas = false;
		}
		
				
		var blockWidth = $(".ls-width-value")[0].value;
		var blockDepth = $(".ls-depth-value")[0].value;
		if(window.correctAns == true)
		{
			if(fittedItems.length > 0)
			{
				var canAdd = true;
				for(var itm=0; itm<fittedItems.length; itm++)
				{
					if(fittedItems[itm][0]==blockWidth && fittedItems[itm][1]==blockDepth)
					{
						canAdd = false;
						break;
					}
				}
				if (canAdd) {
					fittedItems.push([blockWidth, blockDepth]);
						$(".right-sidebar").append("<div class=\"itemFit\">"+blockWidth+" &#215; "+blockDepth+"</div>");
				}
			}
			else{
				
				fittedItems.push([blockWidth, blockDepth]);
				$(".right-sidebar").append("<div class=\"itemFit\">"+blockWidth+" &#215; "+blockDepth+"</div>");
			}
			if(window.canAddBox == true)
			{
			$(".main-message").html("Can you find another box design that fits "+text+" boxes exactly?");
			$(".main-message").show();
			if (window.newImgWidth > $(".right-sidebar").width()) {
					$(".right-sidebar").append("<img src='"+window.newImgURL+"' width=\"90%\"/>");
				}
				else {
					$(".right-sidebar").append("<img src='"+window.newImgURL+"'/>");	
				}	
			}
			
		}
		$(".ls-width-value")[0].value="";
		$(".ls-depth-value")[0].value="";
		$(".ok-button-container").hide();
	});
	$("#save-button-container").hide();
	//save click
	$('#save-button').on('click',function(){
		//saveFunction();
		$("#interactive-container").append(saveContainer);
		$("#modal-container").css({'z-index':10000});
		$(".close-button").bind('click', closeModalContainer);
		$(".cancel-button").bind('click', closeModalContainer);
		$(".save-state-button").bind('click', saveFunction);
		
		$(".cancel-button").removeClass("inactive");
		$(".save-canvas-button").removeClass("inactive");
		$(".save-state-button").removeClass("inactive");
		$(".modal-message").html("Saved!").hide();
	});
	
	// Retrive Saved State Start  
	eventBroker = _({}).extend(require('chaplin/lib/event_broker'));
	eventBroker.publishEvent("#fetch", { type : 'state' }, function(state) {
		if (state) {
			_.each(JSON.parse(state), function(value, key, list) {
				if (value[0]!="" && value[0]!=null) {
					$('.' + key).append(value[0]);
					$("#validTBox").val(value[1]);
					validBlocks = value[1];
					$("#boxesCount").html(value[1]);
					selectedValHeader = value[1];
					if (value[1] > 0) {
						$('#widthB').removeAttr('disabled');
						$('#heightB').removeAttr('disabled');
					}
					$('.' + key).find('.itemFit').each(function (){
					var blockSize = $(this).html();
					blockWidth = blockSize.toString().split(" ")[0];
					blockDepth = blockSize.toString().split(" ")[2];
					fittedItems.push([blockWidth, blockDepth]);
					valueChange();
				});
				}
				
				
				
			});
		}
	});
	// Retrive Saved State End
	
	eventBroker.subscribeEvent('#doSave', function(state) {
	    var state = {};
		state['right-sidebar'] = [$('.right-sidebar').html(),validBlocks];
	    
	    var message = {
		    type : 'state',
		    data : JSON.stringify(state)
	    };
	    eventBroker.publishEvent("#save", message);
	});
	
});
function testBox()
{
	
	
	text = window.document.getElementById("validTBox").selectedIndex;
	if(text > 0)
	{
		$('#widthB').removeAttr('disabled');
		$('#heightB').removeAttr('disabled');
		if ($(".right-sidebar").children().length > 0 || $(".ok-button-container").is(':visible')) {
			$(".popup").show();
		}
		else {
			$("#boxesCount").html(text);
		}
	}
	else{
		$('#widthB').attr('disabled','disabled');
		$('#heightB').attr('disabled','disabled');
		
	}
}

function validate(target){
	var textClass = $(target).attr('class');
	if(textClass.indexOf("ls-width-value")!=-1)
	{
		if(parseInt(target.value) > reqWidth)
		{
			$(".popup").show();
			target.value = target.value.substr(0, target.value.length-1);
			$("#popupMsg").html("Sorry, the packaging machine will not allow for widths larger than "+reqWidth+" blocks.");
			return false;
		}
	}
	if(textClass.indexOf("ls-depth-value")!=-1)
	{
		if(parseInt(target.value) > reqDepth)
		{
			$(".popup").show();
			target.value = target.value.substr(0, target.value.length-1);
			$("#popupMsg").html("Sorry, the packaging machine will not allow for depths larger than "+reqDepth+" blocks.");
			return false;
		}
	}
	if(textClass.indexOf("validBoxField")!=-1)
	{
		if(parseInt(target.value) > maxInputBlocks)
		{
			target.value = target.value.substr(0, target.value.length-1);
		}
		validBlocks = parseInt(target.value);
		$(".shipped-boxes").html("Boxes that fit "+text+" blocks:");
	}
	return true;
}

function ValidateBox(){
	
	var blockWidth = $(".ls-width-value")[0].value;
	var blockDepth = $(".ls-depth-value")[0].value;
	if(blockWidth * blockDepth > validBlocks)
	{
		var aud = new Audio();
		aud.src = "audio/ding.mp3";
		aud.play();
		this.correctAns = false;
		aud.addEventListener('timeupdate', function(ev) {
			if (aud.currentTime == aud.duration) {
				$(".ok-button-container").show();
				$(".main-message").html("Sorry, this box is not full. It will not work. Try again!");
				$(".ok-button").css({'background-image':'url(images/button-try-main.png)'});
			}
			
		});
	}
	else if(blockWidth * blockDepth < validBlocks)
	{
		var aud = new Audio();
		aud.src = "audio/ding.mp3";
		aud.play();
		this.correctAns = false;
		aud.addEventListener('timeupdate', function(ev) {
			if (aud.currentTime == aud.duration) {
		$(".ok-button-container").show();
		$(".main-message").html("Sorry, this box is too small. It will not work, Try again!");
		$(".ok-button").css({'background-image':'url(images/button-try-main.png)'});
			}
		});
	}
	else {
		$(".ok-button").css({'background-image':'url(images/button-ok-main.png)'});
		this.correctAns = true;
		
		if(fittedItems.length > 0)
		{
			var isFirstTime = true;
			for(var itm=0; itm<fittedItems.length; itm++)
			{
				if(fittedItems[itm][0]==blockWidth && fittedItems[itm][1]==blockDepth)
				{
					isFirstTime = false;
					break;
				}
			}
			if (isFirstTime) {
				//fittedItems.push(blockWidth);
				this.canAddBox = true;
				$(".main-message").html("Great &#150; this box works. It will be added to your list for the customer.");
				
			}
			else {
				this.canAddBox = false;
				$(".main-message").html("You have already found this design. Try another.");
				$(".ok-button").css({'background-image':'url(images/button-try-main.png)'});
			}
		}
		else{
			//fittedItems.push(blockWidth);
			this.canAddBox = true;
			$(".main-message").html("Great &#150; this box works. It will be added to your list for the customer.");
		}
		
		$(".ok-button-container").show();
	}
	$(".main-message").show();
	
	this.clearCanvas = true;
//setTimeout(function (){this.clearCanvas = true;$(".popup").show();}, 2000);

}

function drawBlocks(x, y, z, color){
	var t, n, r, i, o, s, a, u, l, c, p;
	this.modelX = parseInt(x);
	this.modelY = parseInt(y);
	this.modelZ = parseInt(z);
	this.bc = color;
	this.canvas_el = $("#box-canvas")[0];
	this.canwidth = this.canvas_el.width;
	this.canheight = this.canvas_el.height;
	this.ctx = this.canvas_el.getContext("2d");
	totWidth = parseInt(x);
	totDepth = parseInt(y);
	validBlocks = window.document.getElementById("validTBox").selectedIndex;
	totBlocksAllowed=validBlocks;
	totFilledBlocks=-1;
	this.blockSize = {width:30, height:30};
	this.scale = (this.canheight)/((totDepth) * this.blockSize.height);
	if(this.scale>1)
	{
		this.scale = 1;
	}
	this.ctx.scale(this.scale, this.scale);
	this.ctx.translate((this.canwidth/this.scale)-(totDepth*this.blockSize.width)-10, 0);
	(function widthLoop (bw) {          
	   setTimeout(function () {
		   --bw;
		   if(bw>=0)
		   {
			this.ctx.translate(-15,11);
			(function depthLoop (bd) {          
			   setTimeout(function () {
				   var block = totDepth - bd;
				   totFilledBlocks++;
					  if(totFilledBlocks < totBlocksAllowed)
					{
						drawNewBox(block*this.blockSize.width, block*5.5, this.blockSize.width,this.blockSize.height,[[45,129,177],[0,166,210],[100,170,166]]);
					}
					else {
						drawNewBox(block*this.blockSize.width, block*5.5, this.blockSize.width,this.blockSize.height,[[255,255,255],[255,255,255],[255,255,255]]);
					}            
				  if (--bd) depthLoop(bd);
			   }, 20)
			})(totDepth); 
			this.ctx.save();
			this.ctx.restore();
			widthLoop(bw);
		   }
			else {
				this.ctx.closePath();
				this.ctx.beginPath();
				this.ctx.moveTo(-5, this.blockSize.height+20);
				this.ctx.lineTo((totDepth)*30-5, (this.blockSize.height+20)+(totDepth*6));
				this.ctx.strokeStyle = "rgb(255,0,0)";
				this.ctx.lineWidth = 2;
				this.ctx.stroke();
				this.ctx.moveTo((totDepth*30)+(totWidth*15), 30-((totWidth-2)*11)+(totDepth*6));
				this.ctx.lineTo((totDepth)*30, (this.blockSize.height+20)+(totDepth*6));
				this.ctx.strokeStyle = "rgb(255,0,0)";
				this.ctx.lineWidth = 2;
				this.ctx.stroke();
				
				this.ctx.closePath();
				this.ctx.beginPath();
				this.ctx.font = "24px Arial";
				this.ctx.fillStyle = "rgb(255,0,0)";
				this.ctx.textAlign = "right";
				this.ctx.textBaseline = "middle";
				this.ctx.fillText(totDepth, ((totDepth)*30-5)/2, (this.blockSize.height+20+(this.blockSize.height+20)+(totDepth*6))/2+20);
				this.ctx.closePath();
				this.ctx.beginPath();
				this.ctx.closePath();
				this.ctx.beginPath();
				this.ctx.font = "24px Arial";
				this.ctx.fillStyle = "rgb(255,0,0)";
				this.ctx.textAlign = "right";
				this.ctx.textBaseline = "middle";
				this.ctx.fillText(totWidth, (((totDepth*30)+(totWidth*15))+((totDepth)*30))/2+10, ((30-((totWidth-2)*11)+(totDepth*6))+((this.blockSize.height+20)+(totDepth*6)))/2+20);
				this.ctx.closePath();	
				AnimationEnded();	
			}
		}, ((totDepth + totWidth+1) * 20))
	})(totWidth); 
}

function drawNewBox(blockX, blockY, blockWidth, blockHeight, blockColor) {
	this.ctx.lineWidth = 0.5;
	this.ctx.strokeStyle = "rgb(0,0,0)";
	this.ctx.closePath();
	this.ctx.moveTo(blockX, (blockHeight*1.5+blockY));
	this.ctx.lineTo((blockWidth/1.8)+blockX, (blockHeight/0.9+blockY));
	this.ctx.lineTo(blockWidth*1.5+blockX, (blockHeight*1.3+blockY));
        this.ctx.moveTo((blockWidth/2)+blockX, (blockHeight*1.1+blockY));
        this.ctx.lineTo((blockWidth/2)+blockX, blockY);
	this.ctx.stroke();
        
        if(blockColor!=null)
	{
		this.ctx.closePath();
		this.ctx.beginPath();
	}
	this.ctx.closePath();
	this.ctx.beginPath();
	this.ctx.moveTo((blockWidth/2)+blockX, blockY);
	this.ctx.lineTo(blockWidth*1.5+blockX, (blockHeight/6+blockY));this.ctx.stroke();
	this.ctx.lineTo(blockWidth+blockX, (blockHeight/1.8+blockY));
	this.ctx.lineTo(blockX, (blockHeight/2.6+blockY));
	this.ctx.lineTo((blockWidth/2)+blockX, blockY);
	this.ctx.stroke();
	if(blockColor!=null)
	{
		this.ctx.fillStyle = "rgba(" + blockColor[0][0] + "," + blockColor[0][1] + "," + blockColor[0][2] + ", 1)";
		this.ctx.fill();
		
	}
	this.ctx.closePath();
	this.ctx.beginPath();	
	this.ctx.moveTo(blockWidth+blockX, (blockHeight/1.8+blockY));
	this.ctx.lineTo(blockX, (blockHeight/2.6+blockY));
	this.ctx.lineTo(blockX, (blockHeight*1.5+blockY));
	this.ctx.lineTo(blockWidth+blockX, (blockHeight*1.7+blockY));
	this.ctx.lineTo(blockWidth+blockX, (blockHeight/1.8+blockY));
	this.ctx.stroke();

	if(blockColor!=null)
	{
		this.ctx.fillStyle = "rgba(" + blockColor[1][0] + "," + blockColor[1][1] + "," + blockColor[1][2] + ", 1)";
		this.ctx.fill();
		
	}
	this.ctx.closePath();
	this.ctx.beginPath();
	this.ctx.moveTo(blockWidth+blockX, (blockHeight/1.8+blockY));
	this.ctx.lineTo(blockWidth*1.5+blockX, (blockHeight/6+blockY));
	this.ctx.lineTo(blockWidth*1.5+blockX, (blockHeight*1.3+blockY));
	this.ctx.lineTo(blockWidth+blockX, (blockHeight*1.7+blockY));
	this.ctx.lineTo(blockWidth+blockX, (blockHeight/1.8+blockY));
	this.ctx.stroke();
        
	if(blockColor!=null)
	{
		this.ctx.fillStyle = "rgba(" + blockColor[2][0] + "," + blockColor[2][1] + "," + blockColor[2][2] + ", 1)";
		this.ctx.fill();
		
	}
	this.ctx.closePath();
}
function AnimationEnded(){
	var TrimmedCanvas = TrimCanvas(this.canvas_el);
	this.newImgURL = TrimmedCanvas.toDataURL();
	this.newImgWidth = TrimmedCanvas.width;
	this.newImgHeight = TrimmedCanvas.height;
	ValidateBox();
}
function valueChange(){
	$(".icoReset").css({'opacity':'1'});
	$(".icoReset").bind('click',reset);
	$(".main-message").html("");
	$(".main-message").hide();
}



function instruct(){
	try {
		$('.icoInfo').removeClass('icoInfoMove');
	} catch(e) {}
	
	$('.question,#blocker').hide();
	try {
		$('#instructions-button-container').unbind('click',openInst);
	} catch(e){}
		$('#instructions-button-container').bind('click',openInst);
		
}

function openInst(){
	$('.question,#blocker').show();
}

/* NAV */

function TrimCanvas(c) {
  var ctx = c.getContext('2d'),
    copy = document.createElement('canvas').getContext('2d'),
    pixels = ctx.getImageData(0, 0, c.width, c.height),
    l = pixels.data.length,
    i,
    bound = {
      top: null,
      left: null,
      right: null,
      bottom: null
    },
    x, y;
 
  for (i = 0; i < l; i += 4) {
    if (pixels.data[i+3] !== 0) {
      x = (i / 4) % c.width;
      y = ~~((i / 4) / c.width);
  
      if (bound.top === null) {
        bound.top = y;
      }
      
      if (bound.left === null) {
        bound.left = x; 
      } else if (x < bound.left) {
        bound.left = x;
      }
      
      if (bound.right === null) {
        bound.right = x; 
      } else if (bound.right < x) {
        bound.right = x;
      }
      
      if (bound.bottom === null) {
        bound.bottom = y;
      } else if (bound.bottom < y) {
        bound.bottom = y;
      }
    }
  }
  
bound.bottom++;
bound.right++;
    
  var trimHeight = bound.bottom - bound.top,
      trimWidth = bound.right - bound.left,
      trimmed = ctx.getImageData(bound.left, bound.top, trimWidth, trimHeight);
  
  copy.canvas.width = trimWidth;
  copy.canvas.height = trimHeight;
  copy.putImageData(trimmed, 0, 0);
  // open new window with trimmed image:
  return copy.canvas;
}


function toggleClassButton(element,className){
	var currentButton=element;
	if(!currentButton.hasClass(className)){
		currentButton.addClass(className);
	}else{
		currentButton.removeClass(className);	
	}
}



function reset(){
	var current=$(this);
	current.removeClass('icoResetMove');
	var to=setTimeout(function(){
	clearInterval(to);
	current.addClass('icoResetMove');
	setTimeout(function(){
	$(".ls-width-value").val($(".ls-width-value option:first").val());
	$(".ls-depth-value").val($(".ls-depth-value option:first").val());
	$(".ls-width-value").removeAttr('disabled');
	$(".ls-depth-value").removeAttr('disabled');
	$(".right-sidebar").html("");
	$(".main-message").html("");
	$(".main-message").hide();
	$(".ok-button-container").hide();
	fittedItems=new Array();
	window.document.getElementById("validTBox").selectedIndex ="";
	$("#boxesCount").html("0");
	try {
	window.ctx.setTransform(1,0,0,1,0,0);
	window.ctx.clearRect(0, 0, window.canwidth, window.canheight);
	}catch(e){}
	},1000,change());
	},200);
}

function change(){
		$(".icoReset").css({'opacity':'.5'});
		$(".icoReset").unbind('click',reset);
	}
function saveFunction() {
    var state = {};
	state['right-sidebar'] = [$('.right-sidebar').html(),validBlocks];
    
    var message = {
	    type : 'state',
	    data : JSON.stringify(state)
    };
    
    eventBroker.publishEvent("#save", message);
    
    $(".cancel-button").addClass("inactive");
    $(".save-canvas-button").addClass("inactive");
    $(".save-state-button").addClass("inactive");
    $(".modal-message").html("Saved!").show();
    setTimeout(function(){closeModalContainer();},1000)
}
function closeModalContainer()
{
    $("#modal-container").remove();
    $(".close-button").unbind('click', closeModalContainer);
    $(".cancel-button").unbind('click', closeModalContainer);
    $(".save-state-button").unbind('click', saveFunction);
}
