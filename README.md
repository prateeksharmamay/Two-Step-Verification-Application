# Two-Step-Verification-Application
(This App has used Twilio’s “Two-Factor Authentication with Authy” API)

#Tech Stack :
Front - End: UI-routing, Angular.js, HTML, CSS
Middleware - Node.js, Express Framework, EJS
Database - MongoDB, Mongoose

#Features:
1) While signing up, verify User's Phone Number by sending her/him an OTP.
2) While signing in, send user an OTP on registered his registered number.

#What's New when compared with Twilio’s “Two-Factor Authentication with Authy” API:
1) I have used EJS view Engine in my appplication whereas Twilio uses Jade.
This makes application use AngularJS and traditional HTML. This will be easier for developers who has worked mostly with HTML.

2) Twilio provides API to verify mobile number while user sign up. This application goes a little further and uses the same verified number for Two step verification while user log in.

#Prepare your Application for Integration:
1)	Signup for Authy: https://www.twilio.com/try-twilio
2)	While you are logged In, Click on "New Application" Option at the end of the left Panel.
3)	Click on "Dashboard".
4)	From: Copy "Api Key for Production" from the Dashboard to the 
	To: Paste to "cfg.authyKey" in routes -> config.js
5)	Go to "https://www.twilio.com/console/account/settings"
	a)	From: copy your Account SID
		To: Paste to "cfg.accountSid" in routes -> config.js
	b)	From: copy your AUTH TOKEN
		To: Paste to "cfg.authToken" in routes -> config.js

#What to do on Front End:
#-------SIGNUP-----------------
	1) Create a custom signup module which asks user for:
	"email" : String,
	"password" : String,
	"fname" : String,
	"lname" : String,
	"phone" : String
	* All fields are required fields.

	2) When user clicks on Submit(can be anything you want),
	make a POST call to "/signup" with above parameters in  a single object "data".
	Ex- data : {
					"email" : $scope.email,
					"password" : $scope.password,
					"fname" : $scope.fname,
					"lname" : $scope.lname,
					"phone" : $scope.phone
				}
				
		Response from the server include "id", which we need to save in $scope.userId
	3) With this call the verification code would have been sent to the specified number.
	4) Now create a modal, that asks user to enter a verification he may have received.
	5) If the user does not receives the code, he can resend it by making a POST call to "/users/resend" with following data:
	Ex- data : {
				"id" : $scope.userId,
			}
		
	6) Once the user submits the verification code, send a POST call to "/users/verify" with following data:
	Ex- data : {
					"id" : $scope.userId,
					"code" : $scope.code
				}
	7) If the verification is successful, you can redirect the control to your desired page
	
#-------SIGNIN-----------------
	1) Create a sign in module which asks user for:
	"email" : String,
	"password" : $scope.password
	
	2)  When user clicks on Login
		,
	make a POST call to "/signin" with above parameters in  a single object "data".
	Ex- data : {
					"email" : $scope.email,
					"password" : $scope.password
				}
				
		Response from the server include "id", which we need to save in $scope.userId2
	3) With this call the verification code would have been sent to the specified number.
	4) Now create a modal, that asks user to enter a verification he may have received.
	5) If the user does not receives the code, he can resend it by making a POST call to "/users/resend" with following data:
	Ex- data : {
				"id" : $scope.userId,
			}
		
	6) Once the user submits the verification code, send a POST call to "/users/verify" with following data:
	Ex- data : {
					"id" : $scope.userId,
					"code" : $scope.code
				}
	7) If the verification is successful, you can redirect the control to your desired page.
	
P.S: 
1) For Front End, refer following functions in public->angularjs->controllers->indexController.js:
Signup: $scope.resendCode , $scope.verifyUser, $scope.registerUser
Signin: $scope.resendLoginCode , $scope.verifyUserLogin, $scope.signin<br>
2) You may reuse backend code in routes without any modification by just sending the required values in the service call.