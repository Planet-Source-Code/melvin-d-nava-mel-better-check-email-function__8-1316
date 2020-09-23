<div align="center">

## MEL :: Better Check Email Function


</div>

### Description

**UPDATED** This function will double check and validate an E-mail address by checking the sintaxis first and the domain's MX, A and CNAME records to be valid and active. It will return TRUE if the email is valid or FALSE if not, very simple. The best approach I've made to validate an Email. Let me know this has been useful, your comments and suggestions are very much appreciate it. **Please Vote**
 
### More Info
 
Relies on the checkdnsrr PHP Function to do the DNS work. Not available for Windows. I've included a replacement (ONLY WIN32)


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Melvin D\. Nava](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/melvin-d-nava.md)
**Level**          |Intermediate
**User Rating**    |4.4 (31 globes from 7 users)
**Compatibility**  |PHP 3\.0, PHP 4\.0
**Category**       |[Validation/ Processing](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/validation-processing__8-16.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/melvin-d-nava-mel-better-check-email-function__8-1316/archive/master.zip)





### Source Code

```
<?php
//
// CHECK EMAIL FUNCTION
//***********************
function check_email_mx($email) {
	if( (preg_match('/(@.*@)|(\.\.)|(@\.)|(\.@)|(^\.)/', $email)) ||
		(preg_match('/^.+\@(\[?)[a-zA-Z0-9\-\.]+\.([a-zA-Z]{2,3}|[0-9]{1,3})(\]?)$/',$email)) ) {
		$host = explode('@', $email);
		if(checkdnsrr($host[1].'.', 'MX') ) return true;
		if(checkdnsrr($host[1].'.', 'A') ) return true;
		if(checkdnsrr($host[1].'.', 'CNAME') ) return true;
	}
	return false;
}
//
//EXAMPLE
//***********************
if (check_email_mx("bill.gates@linux.com")) {
	echo "<p><font color=blue>";
	echo "<p>Email is valid, domain exists and has a valid MX host";
	echo "</font>";
} else {
	echo "<p><font color=red>";
	echo "Either your email is not valid, domain doesn't exists or there is no valid MX host available";
	echo "</font>";
}
//
// FIX FOR WINDOWS
// PROGRAMMERS
//***********************
// checkdnsrr is not available
// under windows I so included
// the next replacement. You
// may remove this if you are
// gonna publish later over
// any Linux/Unix OS
//
// thanx Jon Kriek for next snippet
// and Rickard Sjöquist for notice
// Melvin D. Nava.
//
if (!function_exists('checkdnsrr')) {
	function checkdnsrr($host, $type = '') {
		if(!empty($host)) {
			if($type == '') $type = "MX";
			@exec("nslookup -type=$type $host", $output);
			while(list($k, $line) = each($output)) {
				if(eregi("^$host", $line)) {
					return true;
				}
			}
			return false;
		}
	}
}
?>
```

