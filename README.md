# Halloween-Scavenger-Hunt

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Haunted Pharmacy XR</title>

<script src="https://aframe.io/releases/1.5.0/aframe.min.js"></script>

<style>
body {
    margin: 0;
    overflow: hidden;
    font-family: Arial, sans-serif;
}

#hud {
    position: absolute;
    top: 10px;
    left: 10px;
    z-index: 999;
    background: rgba(0,0,0,0.8);
    color: white;
    padding: 12px;
    border-radius: 10px;
    min-width: 240px;
}

#message {
    margin-top: 10px;
    color: gold;
    font-weight: bold;
}
</style>
</head>

<body>

<div id="hud">
    <div>🎃 Items Found: <span id="score">0</span>/10</div>
    <div>⏱ Time Left: <span id="timer">300</span>s</div>
    <div id="message">
        Find all the haunted pharmacy items!
    </div>
</div>

<a-scene
background="color:#050505"
fog="type:linear; color:#111111; near:10; far:60">

    <!-- LIGHTS -->

    <a-light
        type="ambient"
        intensity="0.5">
    </a-light>

    <a-light
        type="directional"
        color="#88ccff"
        intensity="1"
        position="-5 10 5">
    </a-light>

    <!-- MOON -->

    <a-sphere
        position="0 25 -45"
        radius="3"
        color="#ddddff">
    </a-sphere>

    <!-- GROUND -->

    <a-plane
        rotation="-90 0 0"
        width="80"
        height="80"
        color="#202020">
    </a-plane>

    <!-- BUILDING -->

    <a-box
        position="0 2 -10"
        width="22"
        height="5"
        depth="22"
        color="#444">
    </a-box>

    <a-box
        position="0 2 -10"
        width="20"
        height="4"
        depth="20"
        color="#222"
        material="side:back">
    </a-box>

    <!-- PHARMACY SHELVES -->

    <a-box position="-6 1 -10" width="1" height="2" depth="12" color="#6b4f2a"></a-box>
    <a-box position="6 1 -10" width="1" height="2" depth="12" color="#6b4f2a"></a-box>

    <!-- PUMPKINS -->

    <a-sphere position="-9 .6 -5" radius=".6" color="orange"></a-sphere>
    <a-sphere position="9 .6 -5" radius=".6" color="orange"></a-sphere>

    <!-- GHOST -->

    <a-sphere
        position="0 3 -18"
        radius="1"
        color="white"
        opacity="0.7"
        animation="property: position;
                   from:0 3 -18;
                   to:0 4 -18;
                   dur:2000;
                   dir:alternate;
                   loop:true">
    </a-sphere>

    <!-- PLAYER -->

    <a-entity position="0 1.6 5">

        <a-camera wasd-controls look-controls>

            <a-cursor
                color="white"
                fuse="true"
                fuse-timeout="800">
            </a-cursor>

        </a-camera>

    </a-entity>

    <!-- COLLECTIBLES -->

    <a-sphere class="collectible" position="-4 1 -14" radius=".3" color="#ff00ff"></a-sphere>
    <a-sphere class="collectible" position="4 1 -14" radius=".3" color="#00ffff"></a-sphere>

    <a-box class="collectible" position="-8 1 -7" width=".5" height=".5" depth=".5" color="yellow"></a-box>
    <a-box class="collectible" position="8 1 -7" width=".5" height=".5" depth=".5" color="lime"></a-box>

    <a-cylinder class="collectible" position="0 1 -18" radius=".25" height=".8" color="red"></a-cylinder>
    <a-cylinder class="collectible" position="-2 1 -4" radius=".25" height=".8" color="purple"></a-cylinder>
    <a-cylinder class="collectible" position="2 1 -4" radius=".25" height=".8" color="blue"></a-cylinder>

    <a-sphere class="collectible" position="-9 1 -16" radius=".3" color="white"></a-sphere>
    <a-sphere class="collectible" position="9 1 -16" radius=".3" color="cyan"></a-sphere>

    <a-box class="collectible" position="0 1 -1" width=".5" height=".5" depth=".5" color="orange"></a-box>

</a-scene>

<script>

let score = 0;
const totalItems = 10;

const scoreElement = document.getElementById("score");
const timerElement = document.getElementById("timer");
const messageElement = document.getElementById("message");

const itemMessages = [
    "👻 Ghost Compound Found!",
    "🧪 Witch's Elixir Found!",
    "💊 Zombie Antidote Found!",
    "🎃 Pumpkin Prescription Found!",
    "🦇 Vampire Vaccine Found!"
];

function collect(entity){

    if(entity.dataset.collected) return;

    entity.dataset.collected = "true";

    entity.parentNode.removeChild(entity);

    score++;

    scoreElement.textContent = score;

    messageElement.textContent =
        itemMessages
