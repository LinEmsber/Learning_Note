# Interrupt Trigger Type

## Level triggered
As long as the IRQ line is asserted, you get an interrupt request. When you serve the interrupt and return, if the IRQ line is still asserted, you get the interrupt again immediately.

Level triggered interrupt is like a baby. If the baby cries, you have to abandon whatever you are doing
and feed the baby. You put her down. If she's still crying, you need to attend her immediately again.
As long as she's crying, you serve her needs. You can get back to your work only when she's quiet. If
you are in the garden (interrupt disabled) when she starts to cry, then when you get into the house
(enable interrupts) the first thing you will do is to see her. However, if she starts crying while
you're in the garden but goes back to sleep before you get back to the house, you will not even know
about this incident.

## Edge triggered
You get an interrupt when the line changes from inactive to active state, but only once. To get a new request, the line must go back to inactive and then to active again.

Edge trigger is like a baby cry monitor for deaf parents. The monitor has a red light which comes on
when the baby starts to cry (i.e. there is sudden increase in the sound level in the room) and remains
lit until you press a button on the device. If the baby starts to cry but stops it quicky, you will
still see that she cried, even if you were in the garden while that happened. However, if she starts
crying, the light comes on (interrupt request), then you press the button (interrupt acknowledge), the
light will remain dark even if she keeps crying. The sound level in the room must drop then increase
again for the light to come on.

The crying baby illustration:
level triggered = state
edge triggered = event

## Summary
Level triggered interrupt is an indication that a device needs attention. As long as it needs attention,
the line is asserted. Edge triggered interrupt is an event notification. When some particular thing
happens, the device generates an active edge on the interrupt line.

Normally you use edge-triggered interrupts to ensure that you get an interrupt
when a transitory event occurs and a level-sensitive interrupt when you are detecting a persistent event.
The most certain way to avoid missing interrupts is to latch the interrupt request at the source and use
level-sensitive interrupts (make sure that you clear the latch in the interrupt handler!).

## References
 - [edge triggered v.s. level triggered](http://venkateshabbarapu.blogspot.tw/2013/03/edge-triggered-vs-level-triggered.html)
