# crispy-telegram
CAPTCHA in Java Application using reCAPTCHA
Step 1: Import classes, Load UI and display CAPTCHA
This is the first page where we display the generated CAPTCHA image and get input from the user. We can also do this using JavaScript directly.

<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>

<%@ page import="net.tanesha.recaptcha.ReCaptcha" %>
<%@ page import="net.tanesha.recaptcha.ReCaptchaFactory" %>

<html>
<head>
<title>CAPTCHA in Java using reCAPTCHA</title>
</head>
<body>
<h2>CAPTCHA in Java Application using reCAPTCHA</h2>
<form action="./validate.jsp" method="post">
<p>Username: <input type="text" name="user"></p>
<p>Password: <input type="password" name="password"></p>
<p>
<%
	ReCaptcha c = ReCaptchaFactory.newReCaptcha(
       		  	"6LdlHOsSAAAAAM8ypy8W2KXvgMtY2dFsiQT3HVq-", 
        		"6LdlHOsSAAAAACe2WYaGCjU2sc95EZqCI9wLcLXY", false);
	out.print(c.createRecaptchaHtml(null, null));
%>
   <input type="submit" value="submit" />
</p>        
        </form>
</body>
</html>
Step 2: Import, get input, checkAnswer
Second page where we get the user input, user’s IP address and other required parameters, to pass on to the reCAPTCHA server for input validation. reCAPTCHA server validates the input and returns true or false.

<%@ page import="net.tanesha.recaptcha.ReCaptchaImpl"%>
<%@ page import="net.tanesha.recaptcha.ReCaptchaResponse"%>

<html>
<head>
<title>CAPTCHA in Java using reCAPTCHA</title>
</head>
<body>
<h2>CAPTCHA in Java Application using reCAPTCHA</h2>
<p>
	<%
		String remoteAddr = request.getRemoteAddr();
		ReCaptchaImpl reCaptcha = new ReCaptchaImpl();
		reCaptcha.setPrivateKey("6LdlHOsSAAAAACe2WYaGCjU2sc95EZqCI9wLcLXY");

		String challenge = request
				.getParameter("recaptcha_challenge_field");
		String uresponse = request.getParameter("recaptcha_response_field");
		ReCaptchaResponse reCaptchaResponse = reCaptcha.checkAnswer(
				remoteAddr, challenge, uresponse);

		if (reCaptchaResponse.isValid()) {
			String user = request
					.getParameter("user");
			out.print("CAPTCHA Validation Success! User "+user+" registered.");
		} else {
			out.print("CAPTCHA Validation Failed! Try Again.");
		}
	%>
</p>
<a href=".">Home</a>	
</body>
</html>
