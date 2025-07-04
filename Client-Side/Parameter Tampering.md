
## Lab 1: Excessive Trust in client-side Controls Portswigger


![[Pasted image 20250201164413.png]]

So we login:

![[Pasted image 20250201201439.png]]

We can see that our store credit is "100$"

![[Pasted image 20250202030246.png]]

Intercept the request and add to the card the jacket object:

![[Pasted image 20250202030406.png]]

Change the price of the object then click "forward" to end the request and then look if in your card the jacket costs less:

![[Pasted image 20250202030556.png]]

![[Pasted image 20250202030710.png]]

And then order it:

![[Pasted image 20250202030740.png]]