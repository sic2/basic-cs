# RoboCode extra 2016

**Advancing Velocity** is the speed at which your opponent is moving toward you. For example, a bot moving full speed straight at you would have an advancing velocity of 8, a robot moving full speed perpendicular to you would have an advancing velocity of 0, and a robot moving full speed straight away from you would have an advancing Velocity of -8. A stationary bot will always have an advancing velocity of 0, no matter which direction it's moving.

```
double advancingVelocity = -Math.cos(e.getHeadingRadians() - 
    (e.getBearingRadians() + getHeadingRadians())) * e.getVelocity();
```


A **bullet** in Robocode is a line segment from the point where the bullet starts a tick, to the point where it finishes it. If that segment intercepts a robot the bullet is destroyed and the robot hit takes damage. The robot that fired the bullet gains three times the bullet's power in energy. 
A bullet can have a power between 0.1 and 3. When it is fired the firing robot's energy reduces by the power. If a bullet hits another bullet, they are both destroyed. 
The damage done to the opponent uses the following formula. Unless the opponent has less energy then this, in which it destroys the enemy outright. 

```
damage = power * 4;
if(power > 1)
    damage += ( power - 1 ) * 2;
The speed at which a bullet travels is defined by the following formula. 
speed = 20 - 3 * power;
```

**Bullet Shielding** is a strategy where your robot shoots down incoming bullets, thereby 'shielding' itself from enemy fire. 

The concept is very simple. The idea is to make your bullets collide with the enemy bullets. When the enemy fires a bullet, aim your gun to shoot that bullet down, but only fire bullets with 0.1 power. When intercepting a bullet, both bullets explode; but while the enemy is firing high-powered bullets in an attempt at hitting you, you are firing only 0.1 power bullets. The enemy's energy drains much faster than yours, and eventually the enemy runs out of energy and dies from inactivity time. Voila, you win! 

To **intercept a bullet**, you need to know three things: its speed, its heading, and its position. 

* Speed: first find the bullet power by taking the drop in the enemy's energy, then use the power formula to calculate its speed. 
* Heading: simply assume the bullet is going to hit you. This assumption is valid because if the bullet isn't going to hit you, then it doesn't matter if your shield catches it because it will miss you anyway! The algorithm to assume this is explained below. 
* Position. It's given by the enemy's position at the frame when it was fired, so one turn before you scanned it, and then iterated twice from there (i.e. it moved 2*speed pixels in the direction it's heading). 

So now that you know its position, speed and heading, just use a linear lead fire algorithm to intercept it, the same way you would lead-fire an enemy; the only difference is that you are lead-firing a bullet rather than an enemy. 
The actual implementation of intercepting the bullet is more complicated and it is up to you to find techniques to achieve this.

**Wall Avoidance**: Hitting walls makes an AdvancedRobot take damage and it also brings any robot to a stop. It is usually a good idea to try not to hit walls. 
A common form of wall avoidance is Wall Smoothing. This involves detecting when you are moving towards a nearby wall and turning so that you'll move along the wall instead. A simpler solution is to stop and reverse direction, but that makes your movement much more predictable. 

**Anti-Gravity Movement** is a medium-level movement that is focused on keeping your robot as far as possible from other ones. It involves the using the law of gravity, but reversed; you assign objects (in this case robots and walls) a certain amount of 'gravity', then let the gravity push your robot away from it. 

## Scoring
- **Total Score** - This is everything else added up, and determines each robot's rank in this battle.
- **Survival Score** - Each robot that's still alive scores 50 points every time another robot dies.
- **Last Survivor Bonus** - The last robot alive scores 10 additional points for each robot that died before it.
- **Bullet Damage** - Robots score 1 point for each point of damage they do to enemies.
- **Bullet Damage Bonus** - When a robot kills an enemy, it scores an additional 20% of all the damage it did to that enemy.
- **Ram Damage** - Robots score 2 points for each point of damage they cause by ramming enemies.
- **Ram Damage Bonus** - When a robot kills an enemy by ramming, it scores an additional 30% of all the damage it did to that enemy.
- **1sts, 2nds, 3rds** - These do not actually contribute to score, but are there to show how long the robot survived, i.e. the number of rounds the robot was placed 1st, 2nd, and 3rd. 

## Q & A
**Q**: My robot is not winning with the highest score, even though it is the only one left on the battlefield. Why is that the case?
**A**: Your robot will not necessarily win, just by being the last robot left on the battlefield (i.e. last survivor). A robot that does not fire much, but "just" saves its energy is getting a lesser score than a robot that hits other robots with a lot of bullets. So, do not save your bullets forever just to survive. Make sure you hit with as many of your bullets as possible. The better your robot is at hitting other robots, the better your score will become. 

**Q**: Can I fire bullets with power higher than 3.0 or lower than 1.0?
**A**: No and yes. You can't fire bullets with power greater than 3.0, but you can fire bullets with power as low as 0.1. If you call a firing function (i.e. setFire()) with a value greater than 3.0, Robocode will adjust it to 3.0, and if you call it with a power lower than 0.1 (except 0.0 which will not fire) it will adjust it to 0.1. Additionally, you can fire bullets with power less than 0.1 under one condition: when your robot has less than 0.1 energy left, in which case a bullet is fired with however much energy your robot had left. 

**Q**: How fast does a bullet travel?
**A**: A bullet travels at a speed between 11.0 and 19.7 depending on the power. The more powerful the bullet, the slower. The formula to calculate it is velocity = 20 - (3 * power)

**Q**: Does the robot velocity get added to the bullet velocity on firing? 
**A**: No, bullet velocity is not affected by robot velocity. It's kind of like the speed-of-light thing. =) 

**Q**: Which is the range of a bullet?
**A**: A bullet has no range. It keeps going until it hits a robot or a wall. 

**Q**: What is the size of a bot?
**A**: The size of a bot is 36x36. Note, this is slightly smaller than the image of the bot. It is modeled as a non rotating square, so it's always the same regardless of its heading.