# IVR
KUTUNZA IVR API documentation




getUser = https://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=u
Additional query string options: &u={USERID}

Example: https://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=u
<pre>
Response:
{
	"userID": "",
	"Response": {
		"Gather": {
			"numDigits": 8,
			"input": "dtmf",
			"action": "https://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=u",
			"finishOnKey": "#",
			"Say": {
				"message": "Welcome to H MED MANAGER IVR. Please enter your user ID followed by the pound sign."
			},
			"timeout": 5
		}
	}
}
</pre>

Example: https://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=u&u=84106
Response:
{
	"userID": "84106",
	"Response": {
		"Gather": {
			"numDigits": 1,
			"input": "dtmf",
			"action": "https://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=uc",
			"finishOnKey": "#",
			"Say": {
				"message": "You entered, 8 4 1 0 6. If this is correct, press 1 for Yes or 2 for No, followed by the pound sign."
			},
			"timeout": 5
		}
	}
}


------


getAppt = https://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=a&u={USERID}
Additional query string options: &a={APPTID}

Example: https://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=a&u=84106
Response:
{
	"userID": "84106",
	"apptID": "",
	"Response": {
		"Gather": {
			"numDigits": 8,
			"input": "dtmf",
			"action": "https://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=a",
			"finishOnKey": "#",
			"Say": {
				"message": "Please enter the appointment ID, followed by the pound sign."
			},
			"timeout": 5
		}
	}
}

Example: https://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=a&u=84106&a=1386799
Response:
{
	"status": false,
	"userID": "84106",
	"apptID": "1353401",
	"Response": {
		"Gather": {
			"numDigits": 1,
			"input": "dtmf",
			"action": "https://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=ac",
			"finishOnKey": "#",
			"Say": {
				"message": "You entered, 1 3 5 3 4 0 1. If this is correct, press 1 for Yes or 2 for No, followed by the pound sign."
			},
			"timeout": 5
		}
	}
}

------


Poll = https://appbeta.hhmedsoftware.com/ivr/poll/?s={TRUE/FALSE}&u={USERID}&a={APPTID}

The True/False value for the "s" attribute in the query string is returned from getAppt ( {"status": true/false} ). 
This sets wether or not the selected visit has already been checked in. Default value is FALSE.  

Example: https://appbeta.hhmedsoftware.com/ivr/poll/?s=false&u=84106&a=1386799
Response:
{
	"success": false
}

-----


Verify = https://appbeta.hhmedsoftware.com/ivr/verify/?u={USERID}&a={APPTID}&s={TRUE|FALSE}
This link will be what Twilio sends to the user

Example: https://appbeta.hhmedsoftware.com/ivr/poll/?s=false&u=84106&a=1386799

This link will direct to webpage that will capture GPS coordinates. 
These coordinates are not included in response, only status of { "success": true/false }, which indicates the GPS location was captured.
In the case of a CHECK IN, the response would trigger the IVR to inform the user they have checked in and to end the call.
In the case of a CHECK OUT, the response would trigger the IVR to prompt the user to answer a YES(1)/NO(2) question before being able to complete the call. [PENDING]





