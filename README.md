# DL-DSMA
Docs and demo for my Dampened Stochastic Momentum Attenuator


A DSMA system is a lot like a PID in the sense that it's built to dampen movement and resolve oscillation.

It differs from a PID in several ways, with key differences being:

Only arithmetic: it is much easier to implement into a video game or simple device.

Low latency: there isn't much in between the inputs and output, making it fast for slow systems.

2 variables: it only needs to keep track of 2 states, where a PID needs to keep track of large lists.


A DSMA is useful in situation where hardware is very limited, speed is a concern, or math complexity is your enemy.


Here is what goes on under the hood.


Variables:

S (switch): used to keep track of when the system oscillates.

A (accumulator): natural memory of oscillation



Constants:

D (dampen): like the P in PID, it's used to dampen the error scale. This is the one you will change the most - good ranges are from 0.5 (small adjustments) to 2.5 (large adjustments).

L (leakage): this determines how much the system "remembers". It should really never leave 0.9 - though the range is 0.2 to 0.9.

C (correction): this determines how aggressively it adjusts for oscillation. This should likely stay at 1 - though the likely range is 0 - 2

V (velocity): this is used to determine how much it dampens due to velocity. 0 for no adjustment, and upper values can depend on the rest of the system tuning.



Inputs:

targetPos: where you want to be

currentPos: where you are right now

velocity: your current velocity. Not 100% needed, but much recomended



Equations:

error = targetPos - currentPos

output = error - (velocity * V)

S = S * -1

A = (A * L) + (sign(output) * S)

output = output / (1 + abs(A) * C) * D





How to tune:

Good starting values:

D = 1

L = 0.9

C = 1

V = 1



--- D ---

This is where you start.

A good starting value is 1.

If your system goes over the target a lot, bring D down until it doesn't (or until you stop seeing change).

If your system is too slow, bring D up until it begins to overshoot or it perfectly reaches the target.



--- L ---

This should mostly stay at 0.9.

But, if it is adjusting for oscillation too much (could be an issue?), bring it down a little.

Setting this to 0 will stop all oscillation compensation, though it will not break the rest of the system.



--- C ---

This also shouldn't be changed much.

If it is oscillating too much, bring it up.

Though, just know, the more you bring it up, the more it could drag down the rest of the system.

Setting this to 0 will stop all compensation for oscillation.



--- V ---

This one is fairly simple, and the last one you should tune.

If your system slows down too much when approaching the target, bring it down.

If it isn't slowing down enough, bring it up.

If you have no velocity input or your V is 0, it will not break the rest of the system.



D and V are the two most influential constants to tune.





Note: this will not outperform a PID, it is meant to be a close replacement for when you cannot afford a full PID system.

I have included an HTML that runs a simulation of a DSMA system (the refresh rate for the math is intentionally slowed to better show it in action)

You may need to download it to run it.







©2026 Dunvillel Productions
