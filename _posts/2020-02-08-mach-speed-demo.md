---
layout: post
title: Mach speed interactive demo
subtitle: Look at those shock waves!
tags: [p5js, JavaSript, Graphics]
comments: true
---
<div id="simple-sketch-holder"></div>
<script src="https://cdn.jsdelivr.net/npm/p5@1.0.0/lib/p5.js"></script>

<script>
    "use strict";
 
    new p5(p => {
    let mSlider;
    
    let waves = [];
    let next;
    let speed;
    
    p.setup = () => {
        p.createCanvas(700, 400);
        mSlider = p.createSlider(0, 200, 0).size(80, p.AUTO);
        mSlider.position(100, 300);
        p.windowResized();
        speed = 0.4;
        next = 0;
    };
    
    p.draw = () => {
    p.background(200);
    if (p.millis() > next) {

        // Add new particle
        waves.push(new Wave());
        
        // Schedule next circle
        next = p.millis() + 500;
    }

    // Draw all paths
    for( let i = 0; i < waves.length; i++) {
        waves[i].update();
        waves[i].display();
        if(waves[i].lifespan <= 0){
        waves.splice(i,1);
        }
    }
    p.text('Mach', mSlider.x + mSlider.width + 20, 35);
    p.text(mSlider.value()/100, mSlider.x + mSlider.width + 55, 35);
    };
    
    p.windowResized = () => {
        p._renderer.position(p.windowWidth  - p.width  >> 1,
                            p.windowHeight - p.height >> 1);
    
        const sliderX = (p.width  - mSlider.width  >> 4) + p._renderer.x,
            sliderY = (p.height - mSlider.height >> 4) + p._curElement.y;
    
        mSlider.position(sliderX, sliderY);
    };
    
    class Wave {
    constructor() {
        this.x = p.width/4;
        this.y = p.height/2;
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
        p.stroke(0, this.lifespan);
        p.fill(0,0);
        p.ellipse(this.x, this.y, this.diameter, this.diameter);
    }
    }
    });
</script>
