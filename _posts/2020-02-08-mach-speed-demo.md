---
layout: post
title: Mach speed interative demo
subtitle: Look at those shock waves!
tags: [p5js, JavaSript, Graphics]
comments: true
---

<div id="sketch-holder"></div>

Mach number demostration

---

<script src="https://cdn.jsdelivr.net/npm/p5@1.0.0/lib/p5.js"></script>
<script>
    let waves = [];
    let next;
    let speed;

    function setup() {
    createCanvas(720, 400);
    mSlider = createSlider(0, 200, 0);
    mSlider.position(20, 20);
    speed = 0.4;
    next = 0;
    }

    function draw() {
    background(200);
    if (millis() > next) {

        // Add new particle
        waves.push(new Wave());
        
        // Schedule next circle
        next = millis() + 500;
    }

    // Draw all paths
    for( let i = 0; i < waves.length; i++) {
        waves[i].update();
        waves[i].display();
        if(waves[i].lifespan <= 0){
        waves.splice(i,1);
        }
    }
    text('Mach', mSlider.x + mSlider.width + 20, 35);
    text(mSlider.value()/100, mSlider.x + mSlider.width + 55, 35);
    }

    class Wave {
    constructor() {
        this.x = width/4;
        this.y = height/2;
        this.diameter = 0;
        this.a = 1.2;
        this.lifespan = 255;
    }
    
    update() {
        this.diameter += speed*2;
        this.lifespan -= 0.5;
        this.x = this.x + speed * mSlider.value()/100;
    }
    
    display() {
        stroke(0, this.lifespan);
        fill(0,0);
        ellipse(this.x, this.y, this.diameter, this.diameter);
    }
    }
</script>
