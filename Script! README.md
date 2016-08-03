# Agar.io-Extension-3.1v
// ==UserScript==
// @name        ʞᏗᎮᎮᏗ鿆鿅 Marco 
// @description Kappa_extension     ~credits to Condoriano, Tom buris, and Jack burch [Agar Coding Gods]
// @version     3.1v
// @author      Kappa Agar
// @icon        http://i.imgur.com/zaqvBEI.png
// @include     http://agar.io/*
// @include     https://agar.io/*
// @grant       none
// @namespace   https://Tampermonkey.com
// @run-at      document-end
// ==/UserScript==
window.onload = function() {
var ctx = document.getElementById("canvas").getContext("3d");
    $("h2").replaceWith('<h2 style="color: white; font-family: Ubuntu; font-size: 200%;">ʞᏗᎮᎮᏗ</h2>');
};

document.getElementById("nick").styler = true;
setTimeout(function() {
	'use strict';
	var music = document.getElementById("nick").music;
	console.log("music is: "+music);

	var panels = document.getElementsByClassName("agario-panel");

	for (var n = 0;n<panels.length;n++) {
		panels[n].style.backgroundColor = "black";
		panels[n].style.color = "white";
		panels[n].style.outline = "1px solid white";
		panels[n].style.borderRadius = "0px";
	}

	remove();

	$("#settings").show();
	$("#instructions").show();

	document.getElementsByClassName("btn btn-info btn-settings")[0].addEventListener('click', function() {$("#instructions").show();});

	var inputs = document.getElementsByTagName("input");

	for (var n = 0; n < inputs.length; n++) {
		if (inputs[n].type == "text" || (inputs[n].type != "radio" && inputs[n].type != "checkbox")) {
			inputs[n].style.backgroundColor = "black";
			inputs[n].style.color = "white";
			if (inputs[n].id===null || inputs[n].id === "") {
				inputs[n].id = "uniqueID"+n;
			}
			document.styleSheets[document.styleSheets.length-1].addRule('#'+inputs[n].id+'::selection','background: green');
		}
	}

	//document.styleSheets[document.styleSheets.length-1].insertRule('#nick::focus {outline:0px none transparent;}', 0);

	document.getElementById("gamemode").style.backgroundColor = "black";
	document.getElementById("gamemode").style.color = "white";
	//document.styleSheets[document.styleSheets.length-1].addRule('option::hover','background: green');

	document.getElementById("region").style.backgroundColor = "black";
	document.getElementById("region").style.color = "white";


	document.getElementById("quality").style.backgroundColor = "black";
	document.getElementById("quality").style.color = "white";

	document.getElementById("statsGraph").style.bottom = "";
	document.getElementById("statsGraph").style.top = "40px";

	document.getElementById("stats").getElementsByTagName("hr")[0].style.top = "270px";

	document.getElementById("socialStats").style.bottom = "";
	document.getElementById("socialStats").style.top = "290px";

	document.getElementById("statsContinue").style.bottom = "20px";
	document.getElementById("statsContinue").style.top = "344px";

	document.getElementById("stats").getElementsByTagName("hr")[1].style.top = "40px";

    document.getElementById("#chithercomUI2").style.visibility = "hidden";

	document.getElementById("stats").style.height = "398px";
	document.getElementById("stats").style.padding = "0 0 0";

	document.getElementsByClassName("agario-exp-bar progress")[0].style.backgroundColor = "black";

    document.getElementsByClassName("diep-cross").style.visibility = "hidden";



	var subs = document.getElementById("stats").getElementsByTagName("span");
	for (var n = 0; n < subs.length; n++) {
		if (subs[n].id == "statsSubtext" || subs[n].id == "statsText") {
			subs[n].style.color = "white";
			subs[n].style.opacity = 0.5;
		}
	}

	function remove() {
		var clone = document.getElementById("adsBottom").cloneNode(true);
		clone.style.left = "-9999px";
		document.getElementsByTagName("body")[0].appendChild(clone);
		document.getElementById("overlays").removeChild(document.getElementById("adsBottom"));

		var clone = document.getElementById("s300x250").cloneNode(true);
		clone.style.left = "-9999px";
		document.getElementsByTagName("body")[0].appendChild(clone);
		document.getElementById("stats").removeChild(document.getElementById("s300x250"));

		if (!music) {
			var clone = document.getElementById("adbg").cloneNode(true);
			clone.style.left = "-9999px";
			document.getElementsByTagName("body")[0].appendChild(clone);
			document.getElementById("mainPanel").removeChild(document.getElementById("mainPanel").getElementsByTagName("center")[1]);
		}

		document.getElementsByClassName("agario-panel agario-side-panel agario-shop-panel")[0].removeChild(document.getElementById("blocker"));
		document.getElementsByClassName("agario-panel agario-side-panel agario-panel-freecoins")[0].removeChild(document.getElementById("coins-blocker"));

		document.getElementsByClassName("side-container right-container")[0].removeChild(document.getElementById("agario-web-incentive"));
		document.getElementsByClassName("side-container right-container")[0].removeChild(document.getElementsByClassName("agario-promo")[0]);

		if (!music) {
			document.getElementById("mainPanel").removeChild(document.getElementsByTagName("hr")[1]);
		}
                                 //CSS inject finish


		//document.getElementsByClassName("row")[0].removeChild(document.getElementsByClassName("btn btn-info btn-settings")[0]);
	}
}, 100);


(function() {

	var lastEdit = 1465120103810;
	var profileOptions = {};
	var topPlayers = [], allPlayers = [];
	var loopCount = 0;
	var overlaysOpened = true;
	var gameLoaded = false, coinFirstClick = false;
	var keysHold = {}, ejectorLoop = null;
	var canvas = document.getElementById('canvas');
	var canvas2 = document.getElementById('openfl-content').getElementsByTagName('canvas')[0];

	$(document).ready(function() {
		window.onbeforeunload = function() { return 'Quit game?'; };
		$(document).ajaxComplete(function(e, xhr, stg) {
			if(stg.url.indexOf('findServer') != -1 && xhr.status == 200) {
				if(gameLoaded) setTimeout(onRoomLoad, 800);
				else setTimeout(onGameLoad, 1800);
				gameLoaded = true;
			}
		});
		setTimeout(function() {
			if(!gameLoaded) $('#region').val('US-Atlanta').change();
		}, 10000);
		editOverlays();
		hookOverlays();
		hookKeys();
	});

	function editOverlays() {
		[ 'settings', 'scale_setting', 'quality_setting', 'location' ].forEach(function(e) { localStorage.removeItem(e); });
		$('button[data-itr="play"]').css({'width': '230px'});
		$('button[data-itr="play_as_guest"]').css({'width': '112px'});
		$('button[data-itr="login_and_play"]').css({'width': '112px'}).after('<button id="joinNewRoom" title="Join new room" style="width: 40px; display: block; float: right;" class="btn btn-success btn-refresh"><i class="glyphicon glyphicon-refresh"></i></button>');

		$('#nick').before('<div id="profiles" style="text-align: center; margin-bottom: 6px;"><span> </span></div>');
		for(i = 0; i <= 3; i++) { $('#profiles').append('<button data-title="Loading profile..." class="btn" style=" background-color:black; color:black; margin: 0px 1px; padding: 0px 5px; border: none;">' + (i ? i : '0') + '</button>'); }

		$('#settings').before('<hr style="margin-top: 10px; margin-bottom: 10px;">');
		$('#options').css({ 'margin-top': '16px' })
			.append('<div>Action on room join:<select id="optnOnJoin" style="margin: 0px 6px; background-color:black; color"><option value="">Do nothing</option><option value="spawn">Spawn</option><option value="spec">Join spectator</option></select></div>')
			.append('<div><label><input id="optnSpawn" type="checkbox">Auto respawn</label></div>')
			.append('<div><label><input id="optnCoin" type="checkbox">Click coin every</label><input id="optnCoin_Interval" type="number" min="1" value="15" disabled="disabled" style="margin: 0px 6px; width: 50px; background-color:black;" "><span>min</span></div>')
			.append('<div><label><input id="optnMove" type="checkbox">Randomized movement every</label><input id="optnMove_Interval" type="number" min="1" value="5" disabled="disabled" style="margin: 0px 6px; width: 50px; background-color:black;"><span>sec</span></div>');

		var addedOptions = $('#options div');
		for(var i = 0; i < addedOptions.length; i++) {
			addedOptions.eq(i).css({ 'width': '282px', 'margin': '2px 0px' });
			if(i === 0) addedOptions.eq(i).css({ 'border-top': '1px dashed', 'margin-top': '10px', 'padding-top': '10px' });
		}

		$('#instructions')
			.html('<div style="text-align: center;"><strong>Instructions</strong>Move your mouse to control your cell<br>Press <b>Space</b> to split<br>Press <b>W</b> to eject some mass</div><hr style="color:#FF0000; margin: 10px 0px;">')
			.append('<div><strong>Ingame Shortcuts / Macros</strong><span style="width: 300px; text-align: center; "><b>~</b> Toggle selectable leaderboard list</span><span style="width: 300px; text-align: center; margin-bottom: 4px;"><b>1</b> - <b>0</b> Copy leaderboard name #1 to #10</span><span><b>9</b> Shoot mass cont.</span><span><b>Q</b> Shoot mass 4 times</span><span><b>E</b> Split into 16 pieces</span><span><b>A</b> DoubleSplit/PopSplit</span><span><b>V</b> Stop movement</span><span><b>C</b> Skin panel</span><span><b>B</b> Shop panel</span><span><b>,</b> Free coins panel</span></div>')
			.after('<div class="modInfo-toggler" style="font-size: 75%; color:#922B21;float: right; margin-bottom: 6px !important;">Dyna </div>')
            .after('<div class="modInfo-toggler" style="font-size: 75%; color:#A93226; float: right; margin-bottom: 6px !important;">Mite </div>');
		$('#instructions div').css({ 'color': '#555', 'padding': '0px 10px', 'font-size': '12px', 'margin': '12px 0px' });
		$('#instructions strong').css({ 'color': '#333', 'display': 'block', 'margin-bottom': '6px', 'text-align': 'center', 'font-size': '16px' });
		$('#instructions b').css({ 'background-color': '#1E1E1E', 'color': '#ff0000', 'padding': '1px 5px', 'border-radius': '3px', 'min-width': '20px', 'display': 'inline-block', 'margin': '1px 0px', 'text-align': 'center' });
		$('#instructions span').css({ 'display': 'inline-block', 'min-width': '150px' });

		$('footer.tosBox.left').removeClass('left').addClass('gamemodes').css({ 'position': 'absolute', 'bottom': '32px', 'right': '0px', 'font-size': '12px', 'background-color': 'black', 'border-radius': '15px 0px 0px 15px', 'padding-left': '18px' });
		$('footer.tosBox.right').removeClass('right').addClass('tos').css({ 'position': 'absolute', 'bottom': '2px', 'right': '0px', 'font-size': '12px', 'background-color': 'black', 'border-radius': '15px 0px 0px 15px', 'padding-left': '18px' });

		$('footer a').css('color', '#FFF');
		appendGoogleAd();
		setInterval(appendGoogleAd, 90000);

		$('footer').eq(0).before('<div id="toolTip" style="position: absolute; display: none; transform: translate(-50%, -100%); background-color: #222; color: #EEE; text-align: center; padding: 2px 6px; border-radius: 3px;"></div><div id="modInfo"><div id="modInfo-header"><button class="modInfo-toggler">&#x2716;</button><h2>Script Info</h2></span></div><div id="modInfo-content"></div><div id="modInfo-footer"></div></div>');
		$('#modInfo').css({ 'display': 'none', 'width': '820px', 'height': '500px', 'padding': '0px 20px', 'position': 'absolute', 'top': '50%', 'left': '50%', 'transform': 'translate(-50%, -50%)', 'background-color': '#123', 'color': '#AAA', 'border': '1px solid #000000', 'border-radius': '12px', 'box-shadow': '0px 0px 100px #012 inset' });
		$('#modInfo-header').css({ 'font-family': 'Consolas', 'color': '#EEE', 'padding': '20px 0px', 'text-align': 'center', 'border-bottom': '2px solid #28B', 'position': 'relative' });
		$('#modInfo-header button').css({ 'float': 'right', 'color': '#AAA', 'border': 'none', 'background-color': 'rgba(0, 0, 0, 0.3)', 'position': 'absolute', 'top': '10', 'right': '0' });
		$('#modInfo-header h2').css({ 'margin': '0px' });
		$('#modInfo-header span').css({ 'font-size': '80%', 'color': '#999' });
		$('#modInfo-content').css({ 'font-family': 'Tahoma, sans-serif', 'width': '780px', 'height': '338px', 'overflow': 'auto', 'margin': '20px 0px' });
		$('#modInfo-footer').css({ 'font-size': '80%' });
		$('#modInfo-content').html('<div id="modInfo-content-features"><span style="color: #EEE;">Features</span><ul><li>Auto respawn<li>Auto click hourly coin - clicks at set interval<li>Auto-save settings<li>Profiles - you can save 10 different profiles each with their own settings<li>Reload server button<li>Various macros/shortcuts<li>Join room action - choose between do nothing, spawn, or join spectator<li>Copy leaderboard names<li>Randomize movement - move randomly at set interval<li>Confirm dialog on logout and closing/reloading tab<li>Hide ads and promos<li>Fps Conuter<li>Skype: Dyna.1537 </ul></div>');
		$('#overlays').after('<div id="playerListBox" style="display: none; z-index: 201; position: absolute; top: 10px; left: 10px; opacity: 0.9; background-color: red; padding: 8px; border-radius: 6px;"><a href="#" style="position: absolute; top: 0px; right: 0px;">&times;</a><a href="#">Leaderboard</a><a href="#">All Players</a><textarea rows="10" cols="50" readonly="readonly" wrap="off" style="display: block; border: none; padding: 8px 0px 8px 8px;"></textarea></div>');

		$('#playerListBox a').css({ 'text-decoration': 'none', 'color': '#FFF', 'display': 'inline-block', 'padding': '2px 12px', 'border-radius': '4px 4px 0px 0px' });
		$('#playerListBox a').eq(0).css({ 'position': 'absolute', 'top': '-2px', 'right': '6px', 'font-size': '20px', 'padding': '0' });
		$('#playerListBox a').eq(1).css({ 'background-color': '#FFF', 'color': '#000' });

		$('#advertisement').hide().css({ 'visibility': 'hidden' });
		$('#agario-web-incentive a, .agario-promo a').remove();
		$('#region').val('').css({ 'display': 'block' });
		//$('#stats').css('height', '375px');
		//$('#stats hr').remove();
		//$('#socialStats').css('bottom', '65px');
		//$('#statsContinue').css('bottom', '25px');
		//$('#statsGraph').css({ 'bottom': '110px', 'opacity': '1' }).attr('height', '200px');
		//$('#s300x250').css({ 'display': 'none', 'z-index': '-10' });
		setTimeout(function() { $('#___ytsubscribe_0').css({'width': '112px'}); }, 12000);
	}

	function hookOverlays() {
		$('#freeCoins').on('click', function() { getCoin(); });
		$('button[data-itr="play"]').on('click', function() { overlaysOpened = false; });
		$('button[data-itr="play_as_guest"]').on('click', function() { overlaysOpened = false; });
		$('button[data-itr="spectate"]').on('click', function() { overlaysOpened = true; });
		$('button[data-itr="logout"]').removeAttr('onclick').on('click', function() { var lg = confirm('Logout?'); if(lg) logout(); });
		$('#joinNewRoom').on('click', function() {
			var q = $('#gamemode').val();
			if(q == ':party') $('button[data-itr="create_party"]').click();
			else {
				$('#gamemode').val(':party').change();
				$('#gamemode').val(q).change();
			}
		});
		$('#playerListBox a').on('click', function(e) {
			var curLink = $(this).index();
			if(curLink) {
				$('#playerListBox a').css({ 'background-color': 'transparent', 'color': '#FFF' });
				$(this).css({ 'background-color': '#FFF', 'color': '#000' });
				populatePlayerList(curLink);
			}
			else {
				if($(this).parent().is(':hidden')) $('#playerListBox a').eq(1).click();
				$(this).parent().fadeToggle(200);
			}
			return false;
		});
		$('#profiles button').on('mouseover', function(e) { $('#toolTip').html($(this).attr('data-title')).css({ 'display': 'block', 'top': e.clientY - 20, 'left': e.clientX }); });
		$('#profiles button').on('mouseout', function(e) { $('#toolTip').css({ 'display': 'none' }); });
		$('.modInfo-toggler').on('click', function(e) { $('#modInfo').fadeToggle(); e.preventDefault(); });
		$('.modInfoChangelog-toggler').on('click', function(e) { $(this).next().slideToggle(); e.preventDefault(); });
		$.each(['show', 'hide'], function (i, ev) { var el = $.fn[ev]; $.fn[ev] = function () { this.trigger(ev); return el.apply(this, arguments); }; });
		$('#openfl-content').on('hide', function() {
			setTimeout(function() {
				$('#openfl-content').css({'opacity' : '1'});
				$('#openfl-overlay').css({'pointer-events' : ''});
			}, 10);
		});
	}

	function onGameLoad() {
		handleOptions();
		setInterval(logicLoop, 1000);
		if(localStorage.getItem('activeProfile') === null) localStorage.setItem('activeProfile', 0);
		$('#profiles button').eq(localStorage.getItem('activeProfile')).click();
		updateProfileTitle();
	}
	function onRoomLoad() {
		if(profileOptions.optnOnJoin == 'spawn') $('button[data-itr="play"]').click();
		else if(profileOptions.optnOnJoin == 'spec') $('button[data-itr="spectate"]').click();
		topPlayers = [];
		allPlayers = [];
	}

	function updateProfileTitle(p) {
		var n, m;
		if(p === undefined) { n = 0; m = 3; }
		else { n = p; m = p; }
		for(i = n; i <= m; i++) {
			var temp = JSON.parse(localStorage.getItem('rprofile' + i));
			if(temp === null) $('#profiles button').eq(i).attr('data-title', 'Empty profile');
			else {
				var x, y, z = temp.nick;
                $('#region option').each(function(e) { if($(this).val() == temp.region) x = $(this).text().split(' (')[0]; });
				$('#gamemode option').each(function(e) { if($(this).val() == temp.gamemode) y = '<span style="font-weight: bold; color: ' + (e === 0 ? '#3F3' : e == 1 ? '#3AF' : e == 2 ? '#F33' : '#A3F') + ';">' + $(this).text() + '</span>'; });
				$('#profiles button').eq(i).attr('data-title', x + ' - ' + y + '<br>' + z);
			}
		}
	}

	function logicLoop() {
		loopCount++;
		$('button[data-itr="spectate"]').removeAttr('disabled');
		if(profileOptions.optnSpawn && !overlaysOpened) MC.setNick($('#nick').val());
		if(profileOptions.optnCoin && loopCount % (parseInt($('#optnCoin_Interval').val()) * 60) === 0) showPanel(5);
		if(profileOptions.optnMove && loopCount % parseInt($('#optnMove_Interval').val()) === 0) simulateMove(Math.random() * innerWidth, Math.random() * innerHeight, canvas);
		if(!coinFirstClick) {
			if(profileOptions.optnCoin && $('#coins-blocker').css('display') == 'none') {
				coinFirstClick = true;
				setTimeout(function() { showPanel(5); }, 1600);
			}
		}
	}

	function hookKeys() {
		$(document).on('keyup', function(e) {
			var key = e.which || e.keyCode;
			keysHold[key] = false;
			if(key == 38) { // key E
				clearInterval(ejectorLoop);
				ejectorLoop = null;
			}
		});

		$(document).on('keydown', function(e) {
            var key = e.which || e.keyCode;
            var spKeys = e.ctrlKey || e.altKey || e.shiftKey;
            //console.log('keydown ' + key);
            if($('#overlays').is(':hidden') && !spKeys) {
                if(key == 27) overlaysOpened = true; // key ESC
                if(!keysHold[key]) {
                    if(key == 36) { // key NUMPAD 9
                        if(!ejectorLoop) {
                            ejectorLoop = setInterval(function() {
                                window.onkeydown({ keyCode: 87 });
                                window.onkeyup({ keyCode: 87 });
                            }, 10);
                        }
                    }
                    else if(key == 37) { // key Q
                        setIntervalX(function() {
                            window.onkeydown({ keyCode: 87 }); // key W
                            window.onkeyup({ keyCode: 87 });
                        }, 10, 4);
                    }
                    else if(key == 37) { // key E
                        setIntervalX(function() {
                            window.onkeydown({ keyCode: 32 });  // key SPACE
                            window.onkeyup({ keyCode: 32 });
                        }, 120, 4);
                    }
                    else if(key == 35) { // key A
                        setIntervalX(function() {
                            window.onkeydown({ keyCode: 32 });  // key SPACE
                            window.onkeyup({ keyCode: 32 });
                        }, 60, 2);
                    }
                    else if(key == 67) showPanel(2); // key C
                    else if(key == 66) showPanel(1); // key B
                    else if(key == 188) showPanel(5); // key ,
                }
                keysHold[key] = true;
            }
            if(document.activeElement.tagName.toUpperCase() != 'INPUT' && document.activeElement.type != 'text') {
                if(key == 192) { // key ~
                    $('#playerListBox a').eq(0).click();
                    keysHold[key] = true;
                }
                else if(key >= 48 && key <= 57) { // keys 0-9
                    var playerPos = key == 48 ? 9 : key - 49;
                    if(topPlayers[playerPos] !== undefined) {
                        var newName = topPlayers[playerPos];
                        var cnfrm = confirm('Copy "' + newName + '" to current profile?');
                        if(cnfrm) $('#nick').val(newName).change();
                    }
                }
            }
        });
    }


	function populatePlayerList(curTab) {
		var playerList = '';
		if(curTab == 1) for(var i = 0; i < topPlayers.length; i++) playerList += (i + 1) + '. ' + topPlayers[i] + (i == topPlayers.length - 1 ? '' : '\n');
		else if(curTab == 2) for(var i = 0; i < allPlayers.length; i++) playerList += (i + 1) + '. ' + allPlayers[i] + (i == allPlayers.length - 1 ? '' : '\n');
		$('#playerListBox textarea').html(playerList);
	}

	function handleOptions() {
		var profiles = $('#profiles button');
		var checkboxes = $('#options input[type="checkbox"]');
		var textfields = $('#nick, #gamemode, #region, #quality, #optnOnJoin, #optnCoin_Interval, #optnMove_Interval');

		checkboxes.on('change', function() {
			var optnID = $(this).attr('id');
			var optnEnabled = $(this).is(':checked');
			profileOptions[optnID] = optnEnabled;
			localStorage.setItem('rprofile' + localStorage.getItem('activeProfile'), JSON.stringify(profileOptions));
			updateProfileTitle(localStorage.getItem('activeProfile'));
			if(optnID == 'optnCoin') {
				if(optnEnabled) $('#optnCoin_Interval').removeAttr('disabled');
				else $('#optnCoin_Interval').attr('disabled', 'disabled');
			}
			else if(optnID == 'optnMove') {
				if(optnEnabled) $('#optnMove_Interval').removeAttr('disabled');
				else $('#optnMove_Interval').attr('disabled', 'disabled');
			}
		});
		textfields.on('change', function() {
			var optnID = $(this).attr('id');
			var optnValue = $(this).val();
			if(parseInt(optnValue) < parseInt($(this).attr('min'))) $(this).val($(this).attr('min')).change();
			profileOptions[optnID] = optnValue;
			localStorage.setItem('rprofile' + localStorage.getItem('activeProfile'), JSON.stringify(profileOptions));
			updateProfileTitle(localStorage.getItem('activeProfile'));
			if(optnID == 'optnCoin_Interval') loopCount = 0;
		});

		profiles.on('click', function() {
			var curProfile = parseInt($(this).text()) ? parseInt($(this).text()) : 0;
			var profileExist = false;
			profiles.css({ 'background-color': 'black', 'color': '#FF0000' });
			$(this).css({ 'background-color': '#1E1E1E' });
			localStorage.setItem('activeProfile', curProfile);
			if(localStorage.getItem('rprofile' + curProfile)) { profileOptions = JSON.parse(localStorage.getItem('rprofile' + curProfile)); profileExist = true; }
			if(!Object.keys(profileOptions).length) profileExist = false;

			checkboxes.each(function() {
				var optnID = $(this).attr('id');
				var optnEnabled = $(this).is(':checked');
				if(profileExist) {
					if(optnEnabled && !profileOptions[optnID]) $(this).click().change();
					else if(profileOptions[optnID] === true && !optnEnabled) $(this).click().change();
				}
			});
			textfields.each(function() {
				var optnID = $(this).attr('id');
				var optnValue = $(this).val();
				if(!profileExist) {
					if(optnID == 'nick') {
						if(localStorage.getItem('profile' + curProfile) !== null) {
							$(this).val(JSON.parse(localStorage.getItem('profile' + curProfile)).nick);
							localStorage.removeItem('profile' + curProfile);
						}
						else $(this).val('NooB OR GoD');
					}
					profileOptions[optnID] = $(this).val();
				}
				if(profileExist && profileOptions[optnID] != optnValue) $(this).val(profileOptions[optnID]).change();
			});
			
			updateProfileTitle(localStorage.getItem('activeProfile'));
		});
	}

	function appendGoogleAd() {
		window.google_ad_client = "ca-pub-8318511014856551"; window.google_ad_slot = "5881481221"; window.google_ad_width = 300; window.google_ad_height = 250;
		var container = document.getElementById('agario-web-incentive'), script = document.createElement('script'), w = document.write;
		document.write = function (content) { container.innerHTML = content; document.write = w; };
		script.src = 'http://pagead2.googlesyndication.com/pagead/show_ads.js';
		document.body.appendChild(script);
		setTimeout(function() { script.remove(); }, 50000);
	}

	function getCoin() {
		var xPoses = [ -151, 192, 192, 192, 232 ];
		var yPoses = [ 31, -208, -160, -150, -62 ];
		var delays = [ 500, 1700, 1750, 1800, 2000 ];
		for(var i = 0; i < xPoses.length; i++) {
			(function(j) {
				setTimeout(function() { simulateClick(window.innerWidth / 2 + xPoses[j], window.innerHeight / 2 + yPoses[j], canvas2); }, delays[j]);
			})(i);
		}
	}

	function showPanel(x) {
		$('#openfl-content').css({'opacity' : '0.70'});
		$('#openfl-overlay').css({'pointer-events' : 'none'});
		if(x == 1) $('#openShopBtn').click();
		else if(x == 2) $('#skinButton').click();
		else if(x == 3) $('#boostButton').click();
		else if(x == 4) $('#massButton').click();
		else if(x == 5) $('#freeCoins').click();
	}

	function simulateMove(x, y, el) {
		if(!el) el = document.elementFromPoint(x, y);
		var ev = new MouseEvent('mousemove', { 'clientX': x, 'clientY': y }); el.dispatchEvent(ev);
	}

	function simulateClick(x, y, el) {
		// console.log(x + ',' + y);
		if(!el) el = document.elementFromPoint(x, y);
		var ev = new MouseEvent('mousedown', { 'clientX': x, 'clientY': y }); el.dispatchEvent(ev);
		ev = new MouseEvent('mouseup', { 'clientX': x, 'clientY': y }); el.dispatchEvent(ev);
	}

	function setIntervalX(callback, delay, repetitions) {
		var x = 0;
		var intervalID = window.setInterval(function () {
			callback();
			if (++x === repetitions) window.clearInterval(intervalID);
		}, delay);
	}

	function timeSince(date) {
		var seconds = Math.floor((new Date() - date) / 1000);
		var interval = Math.floor(seconds / 31536000);
		if(interval > 1) return interval + ' years'; interval = Math.floor(seconds / 2592000);
		if(interval > 1) return interval + ' months'; interval = Math.floor(seconds / 86400);
		if(interval > 1) return interval + ' days'; interval = Math.floor(seconds / 3600);
		if(interval > 1) return interval + ' hours'; interval = Math.floor(seconds / 60);
		if(interval > 1) return interval + ' minutes';
		return Math.floor(seconds) + ' seconds';
	}

	function canvasModding() {
		var proxiedFillText = CanvasRenderingContext2D.prototype.fillText;
		CanvasRenderingContext2D.prototype.fillText = function() {
			if(arguments[0] == 'Leaderboard') {
				arguments[0] = 'Leaderborad';
				topPlayers = [];
			}
			else if(parseInt(arguments[0]) >= 1 && parseInt(arguments[0]) <= 10) { // 1. xxx to 10. xxx
				var rank = parseInt(arguments[0]);
				if(rank <= 9 && arguments[0][1] == '.' || rank == 10 && arguments[0][2] == '.') {
					var tempName = arguments[0].substr(rank == 10 ? 4 : 3);
					topPlayers[rank - 1] = tempName;
					if(allPlayers.indexOf(tempName) == -1) allPlayers.push(tempName);
				}
			}
			return proxiedFillText.apply(this, arguments);
		};
		var proxiedStrokeText = CanvasRenderingContext2D.prototype.strokeText;
		CanvasRenderingContext2D.prototype.strokeText = function() {
			if(isNaN(arguments[0]) && allPlayers.indexOf(arguments[0]) == -1) allPlayers.push(arguments[0]);
			return proxiedStrokeText.apply(this, arguments);
		};
	}
	canvasModding();
//---------------------- Burst Splitter --------------------//


/*(function() {
    var amount = 7;
    var duration = 10; //ms

    var overwriting = function(evt) {
        if (evt.keyCode === 69) { // KEY_E
            for (var i = 0; i < amount; ++i) {
                setTimeout(function() {
                    window.onkeydown({keyCode: 32}); // KEY_space
                    window.onkeyup({keyCode: 32});
                }, i * duration);
            }
        }
    };

    window.addEventListener('keydown', overwriting);
})();

//-------------------Mass Ejector --------------//


/*(function() {
    var amount = 4;
    var duration = 10; //ms

    var overwriting = function(evt) {
        if (evt.keyCode === 81) { // KEY_Q
            for (var i = 0; i < amount; ++i) {
                setTimeout(function() {
                    window.onkeydown({keyCode: 87}); // KEY_W
                    window.onkeyup({keyCode: 87});
                }, i * duration);
            }
        }
    };

    window.addEventListener('keydown', overwriting);
})();*/

//-------------------Bot Mass Ejector --------------//

     (function() {
    var amount = 4;
    var duration = 10; //ms

    var overwriting = function(evt) {
        if (evt.keyCode === 67) { // KEY_Q
            for (var i = 0; i < amount; ++i) {
                setTimeout(function() {
                    window.onkeydown({keyCode: 67}); // KEY_W
                    window.onkeyup({keyCode: 67});
                }, i * duration);
            }
        }
    };

    window.addEventListener('keydown', overwriting);
})();

//-------------------DoubleSplit------------------------//

/*(function() {
    var amount = 2;
    var duration = 40; //ms

    var overwriting = function(evt) {
        if (evt.keyCode === 65) { // KEY_A
            for (var i = 0; i < amount; ++i) {
                setTimeout(function() {
                    window.onkeydown({keyCode: 32}); // KEY_space
                    window.onkeyup({keyCode: 32});
                }, i * duration);
            }
        }
    };

    window.addEventListener('keydown', overwriting);*/
})();
//-------------------Fps COunter------------------------//
    (function() {
  var UPDATE_DELAY = 700;

  var lastUpdate = 0;
  var frames = 0;

  var displayElement = document.createElement("div");
  displayElement.style.padding = "0px";
  displayElement.style.font = "16px ubuntu";
  displayElement.style.display = "block";
  displayElement.style.position = "fixed";
  displayElement.style.top = "0px";
  displayElement.style.left = "45px";
  displayElement.textContent = "Calculating...";
  document.body.appendChild(displayElement);

  function cssColorToRGB(color) {
    var values;

    if (color.startsWith("rgba")) {
      values = color.substring(5, color.length - 1).split(",");
    } else if (color.startsWith("rgb")) {
      values = color.substring(4, color.length - 1).split(",");
    } else if (color.startsWith("#") && color.length === 4) {
      values = [];
      values[0] = "" + parseInt("0x" + color.substr(1, 1));
      values[1] = "" + parseInt("0x" + color.substr(2, 1));
      values[2] = "" + parseInt("0x" + color.substr(3, 1));
    } else if (color.startsWith("#") && color.length === 7) {
      values = [];
      values[0] = "" + parseInt("0x" + color.substr(1, 2));
      values[1] = "" + parseInt("0x" + color.substr(3, 2));
      values[2] = "" + parseInt("0x" + color.substr(5, 2));
    } else {
      return {r : 255, g : 255, b : 255};
    }

    return {
      r : Number(values[0]),
      g : Number(values[1]),
      b : Number(values[2])
    };
  }

  function getInvertedRGB(values) {
    return "rgb(" + (255 - values.r) + "," + (255 - values.g) + ","
      + (255 - values.b) + ")";
  }

  function getOpaqueRGB(values) {
    return "rgba(" + values.r + "," + values.g + "," + values.b + ",0.7)";
  }

  function updateCounter() {
    var bgColor = getComputedStyle(document.body, null).getPropertyValue(
      "background-color");
    var bgColorValues = cssColorToRGB(bgColor);
    var textColor = getInvertedRGB(bgColorValues);
    var displayBg = getOpaqueRGB(bgColorValues);
    displayElement.style.color = "646C78";
    displayElement.style.opacity=  "0.8";

    var now = Date.now();
    var elapsed = now - lastUpdate;
    if (elapsed < UPDATE_DELAY) {
      ++frames;
    } else {
      var fps = Math.round(frames / (elapsed / 1000));
      displayElement.textContent = fps;
      frames = 0;
      lastUpdate = now;
    }

    requestAnimationFrame(updateCounter);
  }

  lastUpdate = Date.now();
  requestAnimationFrame(updateCounter);


})();

//-------------------Mouse Clicks------------------------//
/*window.addEventListener('keydown', keydown);
window.addEventListener('keyup', keyup);
var Feed = false;
var Speed = 10; //default = 25
var splits = 1;

//Funtions
function split() {
    $("body").trigger($.Event("keydown", { keyCode: 32}));
    $("body").trigger($.Event("keyup", { keyCode: 32}));
}
function mass() {
    if (Feed) {
        window.onkeydown({keyCode: 87});
        window.onkeyup({keyCode: 87});
        setTimeout(mass, Speed);
    }
}
//---------------------------mouse CLick---------------------------//
 (function() {
    document.getElementById("canvas").addEventListener("mousedown", function(event) {
        if (event.which == 1) {
            split();
        }
        else if (event.which == 2) {
            split();
            setTimeout(split, Speed);
            setTimeout(split, Speed*2);
            setTimeout(split, Speed*3);
        }
        else if (event.which == 3) {
            Feed = true;
            setTimeout(mass, Speed);
        }
    });

    document.getElementById("canvas").addEventListener("mouseup", function(event) {
        if (event.which == 3) {
            Feed = false;
        }
    });
    $('#canvas').bind('contextmenu', function(e) {
        e.preventDefault();
    });
}());*/

//-------------------Macros------------------------//
window.addEventListener('keydown', keydown);
window.addEventListener('keyup', keyup);
var Feed = false;
var Dingus = false;
var imlost = 25;
load();
function load() {

        setTimeout(load, 100);

}
function keydown(event) {
    if (event.keyCode == 81) {
        Feed = true;
        setTimeout(fukherriteindapussie, imlost);
    } // Tricksplit
    if (event.keyCode == 69 || event.keyCode == 52) { //( ͡° ͜ʖ ͡°)
        ilikedick();
        setTimeout(ilikedick, imlost);
        setTimeout(ilikedick, imlost*2);
        setTimeout(ilikedick, imlost*3);
    } // Triplesplit
    if (event.keyCode == 51 || event.keyCode == 65) {
        ilikedick();
        setTimeout(ilikedick, imlost);
        setTimeout(ilikedick, imlost*2);
    } // Doublesplit
    if (event.keyCode == 65 || event.keyCode == 50) {
        ilikedick();
        setTimeout(ilikedick, imlost);
    } // Split
    if (event.keyCode == 49) {
        ilikedick();
    }
} // When Player Lets Go Of Q, It Stops Feeding
function keyup(event) {
    if (event.keyCode == 81) {
        Feed = false;
    }
    if (event.keyCode == 79) {
        Dingus = false;
    }
}
// Feed Macro With Q
function fukherriteindapussie() {
    if (Feed) {
        window.onkeydown({keyCode: 87});
        window.onkeyup({keyCode: 87});
        setTimeout(fukherriteindapussie, imlost);
    }
}
function ilikedick() {
    $("body").trigger($.Event("keydown", { keyCode: 32}));
    $("body").trigger($.Event("keyup", { keyCode: 32}));
}

// ====================== END ===================================//

