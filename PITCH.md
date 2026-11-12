# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built:
We built a support agent based upon a framework for handling customer requests around disrupted flights.  It had the ability to check on other available flights, select those flights.  It had built in function to escalate to a human handler if needed.  We added 2 new locally defined tools, fare_rules and next_available_day to help with resolutions.  We also have get_entitlements.
Does:
It can find the provided booking by matching booking number with surname.  It can find flights with free seats, reserve it, and explain any fare rules or entitlements.  It can offer rebooking options as defined in the policy.
Number:
The tokens in increased from 53,621 to 82,468, approx a 60% increase.  Supposition is that this is due to adding the new tools in.
Safety check:
In the output from the python M3XR8T we can see the explicit prompting that it won't finalise the change without the client pressing confirm
Next:
Still broken:
Lever: <cost | speed | intelligence>

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
