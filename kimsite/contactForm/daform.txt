<?php	
$page_title = 'Comments';	
include ('includes/header.html');

			Main Conditional (till line 57)	

if (isset($_POST['submitted'])) {
require_once ('include/mysqli_connect.php');

			Spam Scrubber Defined	

function spam_scrubber($submitted_value) {
$very_bad = array('to:','cc','bcc','content-type','mime-version','multipart-mixed','content-transfer-encoding:');
foreach ($very_bad as $oneatatime) {
if (stripos($submitted_value,$oneatatime) !== false
return ' ';	
}	
$submitted_value = str_replace(array("\r", "\n", "%0a", "%0d"), ' ', $submitted_value);	
return trim($submitted_value);	
}	

			Spam Scrubber Used	

$scrubbed = array_map('spam_scrubber',$_POST);	
	
$errors = array();	

			First Name Check	

if (empty($scrubbed['first_name'])) {	
$errors[] = 'You forgot to enter your first name.';	
} else {
$fn = mysqli_real_escape_string($dbc,trim(strip_tags($scrubbed['first_name'])));	
}	

			Last Name Check (same coding as above)	

if (empty($scrubbed['last_name'])) {	
$errors[] = 'You forgot to enter your last name.';	
} else {	
$ln = mysqli_real_escape_string($dbc,trim(strip_tags($scrubbed['last_name'])));	
}	
			E-mail Check (same coding as above)	

if (empty($scrubbed['email'])) {	
$errors[] = 'You forgot to enter your email address.';	
} else {	
$em = mysqli_real_escape_string($dbc,trim(strip_tags($scrubbed['email'])));	
}

			Comments Check (same coding as above)	

if (empty($scrubbed['comments'])) {	
$errors[] = 'You forgot to enter a comment';	
} else {	
$cm = mysqli_real_escape_string($dbc,trim(strip_tags($scrubbed['comments'])));	
}	
			If All Fields Correct	

if (empty($errors)) {	
$q = "INSERT INTO form00 (first_name, last_name, email, comments, date) VALUES ('$fn', '$ln','$em','$cm', NOW() )";
$run = @mysqli_query ($dbc, $q);
if ($run) {	
echo '<h1>Thank you!</h1>
<p>The information you have sent has been received.<br />
Please continue to browse if you would like to!<br />
Or come again soon! <br />Thanks Again.</p><p><br /></p>';	
} else {	
echo '<h1>System Error</h1>
<p class="error">There was some problem running $r-- You could not be registered due to a system error.<br />
We apologize for any inconvenience.</p>';	
echo '<p>' . mysqli_error($dbc) . '<br /><br />Query: ' . $q . '</p>';	
}	
mysqli_close($dbc);	
include ('inc00/footer.html');	
exit();
	
			If All Fields NOT Correct	

} else {	
echo '<h1>Error!</h1> <p class="error">The following error(s) occurred:<br />';	
foreach ($errors as $msg) { 	
echo " - $msg<br />\n";	
}	
echo '</p><p>Please try again.</p><p><br /></p>';	
}	
mysqli_close($dbc);	
}	
?>	

			HTML Form	

<h1>Contact Me (Form 00)</h1>	
<p>I would love to hear from you... something must be entered into every space -- anything will work.)</p>	
<form action="form00.php" method="post">	?
<p>First Name: <input type="text" name="first_name" size="15" maxlength="20" value="<?php if
(isset($_POST['first_name'])) echo $_POST['first_name']; ?>" /></p>
<p>Last Name: <input type="text" name="last_name" size="15" maxlength="40" value="<?php if
(isset($_POST['last_name'])) echo $_POST['last_name']; ?>" /></p>	
<p>Email Address: <input type="text" name="email" size="20" maxlength="80" value="<?php if
(isset($_POST['email'])) echo $_POST['email']; ?>" /></p>	
<p>Comments: <textarea type="text" name="comments" rows="10" cols="60"><?php if (isset($_POST['comments'])) echo $_POST['comments']; ?></textarea>
<p><input type="submit" name="submit" value="SEND!" /></p>
<input type="hidden" name="submitted" value="TRUE" />
</form>	

			PHP Footer	
<?php	
include ('inc00/footer.html');	
?>