# SKILLED CARE IVR
KUTUNZA IVR (SKILLED CARE) API documentation

*URLs may change based on server/directory location

<hr />

<strong>Verify User</strong>

getUser = http<area>s://appbeta.hhmedsoftware.com/ivr/getTwil/?<strong>ext=u</strong>

Additional query string options: &u={USERID}

Example: http<area>s://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=u

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


Example: http<area>s://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=u&u=84106

<pre>
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
</pre>

<hr />

<strong>Verify Appointment</strong>

getAppt = http<area>s://appbeta.hhmedsoftware.com/ivr/getTwil/?<strong>ext=a</strong>&u={USERID}

Additional query string options: &a={APPTID}

Example: http<area>s://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=a&u=84106

<pre>
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
</pre>

Example: http<area>s://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=a&u=84106&a=1386799

<pre>
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
</pre>

<hr />

<strong>Capture GPS</strong>

Verify = http<area>s://appbeta.hhmedsoftware.com/ivr/verify/?u={USERID}&a={APPTID}&s={TRUE|FALSE}

This link is what Twilio sends to the user via SMS. This link will direct the user to a webpage that will capture GPS coordinates from the user. 
This link should be shortened (via Bitly) to obfuscate the query string.

Default value is FALSE.

The GPS coordinates are not included in the response. { "success": true } indicates the GPS location was captured on the server.

Example: http<area>s://appbeta.hhmedsoftware.com/ivr/verify/?s=false&u=84106&a=1386799

<pre>
Response:
{
"success": false
}
</pre>

Edit: 20240116 - modified query string to add flag for one-time use

http<area>s://appbeta.hhmedsoftware.com/ivr/verify/?u={USERID}&a={APPTID}&s={TRUE|FALSE}&c={TIMESTAMP}

&c=20240116150630 

The timestamp will be generated for 3 minutes from the time the link is sent. Time is in UTC.

For example, if the link was sent to the user on Jaunary 18, 2024 at 10:08:30, the link would be c=20240118151130.

-----

<strong>CHECK IN</strong>

pollCheckIn = http<area>s://appbeta.hhmedsoftware.com/ivr/getTwil/?<strong>ext=pi</strong>&s={TRUE|FALSE}&u={USERID}&a={APPTID}

This is what Twilio will use to poll/ping the server to await response of GPS coordinates being captured for <strong>CHECK IN</strong>. 

The True/False value for the "s" attribute in the query string is returned from getAppt {"status": true|false}. This establishes whether or not the selected visit has ALREADY been checked in. 

Default value is FALSE (NOT checked in).  If s=true, the user should directed to CHECK OUT.

Example: http<area>s://appbeta.hhmedsoftware.com/ivr/getTwil/?ext=pi&s=false&u=84106&a=1386799

<pre>
Response:
{
"success": false
}
</pre>

When response "success": true, GPS has been captured. The check in process is complete and the call is ended.

-----

<strong>CHECK OUT</strong>

pollCheckOut = http<area>s://appbeta.hhmedsoftware.com/ivr/getTwil/?<strong>ext=pt</strong>&s={TRUE|FALSE}&u={USERID}&a={APPTID}

This is what Twilio will use to poll/ping the server to await response of GPS coordinates being captured for <strong>CHECK OUT</strong>. 

The True/False value for the "s" attribute in the query string is returned from getAppt ( {"status": true|false} ). This establishes whether or not the selected visit has ALREADY been checked in. 

Default value is TRUE.  This link is not called unless the value of "s" is TRUE.

Example: http<area>s://appbeta.hhmedsoftware.com/ivr/getTwil/?<strong>ext=pt</strong>&s=true&u=84106&a=1386799

<pre>
Response:
{
"success": true
}
</pre>

When response "success": true, GPS has been captured. The check out process is complete and the call is ended.

