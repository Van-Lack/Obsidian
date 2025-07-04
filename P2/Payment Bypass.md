


<iframe width="330" height="186" src="https://www.youtube.com/embed/4D-6nWIRZLU" title="Adding infinite funds to your Steam wallet - $7,500 bug bounty report" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>



---


## _Example of a noted API request:

_a **High**_ _severity finding from a private bug bounty program._

_So, this was a unique case hence decided to share it._

_This program had setup a staging environment for testing purpose. They had recently introduced an option to activate premium plan trial option. I activated it & started testing the premium features. I was able to find few bugs. Since I was testing this application for a few months I had saved the project in_ _**Burp Suite.**_ _This step was crucial in uncovering the payment bypass bug which we will see later._

_A few months passed & once again I started poking around this application in the hope of finding new bugs. As I logged into the application, I noticed that the activate trial option had been removed. This meant that they were no longer allowing users to activate primum trial plan. Users had to buy this by making payment._

_As I mentioned earlier, I had saved the project in_ _**Burp Suite.**_ _Thus, I had all the requests saved in it. I checked my repeater tab & found the_ _**API**_ _request which was used to activate the premium trial plan._

_Since my current account had premium already activated in it, I quickly created a new account in the application & replayed this_ _**API**_ _request with the cookie of the new_ _**Owner**_ _account. Surprisingly the server responded with a_ _**200 OK**_ _& the premium trial was activated in the account._

_To confirm the bug, I checked the same scenario on production server & it worked there as well._

_Turns out the trial option was only removed from the UI but the_ _**API**_ _request was not deactivated from the backend by the developer which allowed users to avail premium plan for free without making any payment._

_I quickly reported this to the program._ _**HackerOne**_ _triage team was able to reproduce the issue & triaged it. Later the company reviewed the report and accepted it as_ _**High**_ _severity finding._

_**Summary:**_

_This was a very simple bug which I came across unexpectedly. But the important thing here was saving the project in_ _**Burp Suite.**_ _This helped me find the "activate trial premium" request. So, always remember to save the project while testing any application. You never know when it could be handy._

