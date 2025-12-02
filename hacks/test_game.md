---
layout: opencs
title: platformer
permalink: /platformer/
---

<canvas id="platformer" width="600" height="300" style="background:#e8e8e8; border:2px solid #444; border-radius:10px; margin-top:20px;"></canvas>

<script>
const canvas = document.getElementById("platformer");
const ctx = canvas.getContext("2d");

// Player
let player = {
    x: 50,
    y: 0,
    width: 30,
    height: 30,
    dx: 0,
    dy: 0,
    speed: 4,
    jumping: false
};

// Physics
const gravity = 0.5;
const ground = 250;

// Goal
let goal = {
    x: 520,
    y: 200,
    width: 40,
    height: 50
};

// Input keys
let keys = {};

document.addEventListener("keydown", e => keys[e.key] = true);
document.addEventListener("keyup", e => keys[e.key] = false);

function update() {
    // Horizontal movement
    if (keys["ArrowRight"] || keys["d"]) player.dx = player.speed;
    else if (keys["ArrowLeft"] || keys["a"]) player.dx = -player.speed;
    else player.dx = 0;

    // Jump
    if ((keys["ArrowUp"] || keys["w"]) && !player.jumping) {
        player.dy = -10;
        player.jumping = true;
    }

    // Apply physics
    player.dy += gravity;
    player.x += player.dx;
    player.y += player.dy;

    // Ground collision
    if (player.y + player.height >= ground) {
        player.y = ground - player.height;
        player.dy = 0;
        player.jumping = false;
    }

    // Canvas boundaries
    if (player.x < 0) player.x = 0;
    if (player.x + player.width > canvas.width) player.x = canvas.width - player.width;

    // Win condition
    if (player.x + player.width > goal.x &&
        player.x < goal.x + goal.width &&
        player.y + player.height > goal.y &&
        player.y < goal.y + goal.height) {
            alert("you won. wow. are you proud of yourself. you just wasted 10 seconds of your life on this dumb game that you will never get back. nice job! I'm proud, don't worry. the game was fun, right? best 10 seconds of your life for sure. oh well, good job. if you died... i dont even know how you did. massive skill issue fs breh lock in. yea ik this is kinda bad but ion think u can do this either. oh, u used chatgpt? thats great, academic dishoonesty. uh. i used chatgpt too. its all good. oh well, bye now u can go back to ur regular life. i know ur a better person now that you have played this masterpiece of a game. ok now well im yapping i jsut want to fill this text box fully so it looks like it has a bunch of text eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee ok im done now bye have a great time bye.");
            player.x = 50;
            player.y = 0;
    }

    draw();
    requestAnimationFrame(update);
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Ground
    ctx.fillStyle = "#444";
    ctx.fillRect(0, ground, canvas.width, 50);

    // Player
    ctx.fillStyle = "#1e90ff";
    ctx.fillRect(player.x, player.y, player.width, player.height);

    // Goal object
    ctx.fillStyle = "#ff4444";
    ctx.fillRect(goal.x, goal.y, goal.width, goal.height);
}

update();
</script>