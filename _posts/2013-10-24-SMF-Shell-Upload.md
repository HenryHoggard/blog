---
layout: post
title: SMF 2.0.5 & 1.1.18 Shell upload
tag: 
---

* Reported to Theymos, BitcoinTalk Admin 4/10/2013 
* Theymos Response: 4/10/2013 
* Reported to Vendor: 10/10/2013 
* Vendor First Response: 10/10/2013 
* Vendor Patch: 22/10/2013

 
# About

After the [BitcoinTalk Hack](http://arstechnica.com/security/2013/10/bitcoin-talk-forum-hacked-hours-after-making-cameo-in-silk-road-takedown/), I was interested in finding out how the attackers did it. I began by auditing parts of the source, specifically the avatar upload. I found a bug, that providing the right environment and settings, would enable users to remotely upload a shell to the forum via the avatar upload form. The bug exists in the external avatar download, where a temporary is file is downloaded and written to the box without any checks performed on it. To demonstrate a PoC, I uploaded a Gist to https://gist.github.com with any filename, with any PHP code inside. Then on the user profile page on SMF, download an external avatar using the URL of the Gist. The vulnerable code is shown below. 

**Sources/Profile-Modify.php**

```php        
function profileSaveAvatarData(&$value)
        {
                global $modSettings, $sourcedir, $smcFunc, $profile_vars, $cur_profile, $context;

                $memID = $context['id_member'];
                if (empty($memID) && !empty($context['password_auth_failed']))
                        return false;

                require_once($sourcedir . '/ManageAttachments.php');

                // We need to know where we're going to be putting it..
                if (!empty($modSettings['custom_avatar_enabled']))
                {
                        $uploadDir = $modSettings['custom_avatar_dir'];
                        $id_folder = 1;
                }
                elseif (!empty($modSettings['currentAttachmentUploadDir']))
                {
                        if (!is_array($modSettings['attachmentUploadDir']))
                                $modSettings['attachmentUploadDir'] = unserialize($modSettings['attachmentUploadDir']);

                        // Just use the current path for temp files.
                        $uploadDir = $modSettings['attachmentUploadDir'][$modSettings['currentAttachmentUploadDir']];
                        $id_folder = $modSettings['currentAttachmentUploadDir'];
                }
                else
                {
                        $uploadDir = $modSettings['attachmentUploadDir'];
                        $id_folder = 1;
                }

                $downloadedExternalAvatar = false;
                if ($value == 'external' && allowedTo('profile_remote_avatar') && strtolower(substr($_POST['userpicpersonal'], 0, 7)) == 'http://' && strlen($_POST['userpicpersonal']) > 7 && !empty($modSettings['avatar_download_external']))
                {
                        if (!is_writable($uploadDir))
                                fatal_lang_error('attachments_no_write', 'critical');

                        require_once($sourcedir . '/Subs-Package.php');

                        $url = parse_url($_POST['userpicpersonal']);
                        $contents = fetch_web_data('http://' . $url['host'] . (empty($url['port']) ? '' : ':' . $url['port']) . str_replace(' ', '%20', trim($url['path'])));

                        if ($contents != false && $tmpAvatar = fopen($uploadDir . '/avatar_tmp_' . $memID, 'wb'))
                        {
                                fwrite($tmpAvatar, $contents);
                                fclose($tmpAvatar);

                                $downloadedExternalAvatar = true;
                                $_FILES['attachment']['tmp_name'] = $uploadDir . '/avatar_tmp_' . $memID;
                        }
```

Specifically, the url is directly taken from the users POST Request "($_POST['userpicpersonal']);", downloaded with `fetch_web_data,` and written with `fwrite($tmpAvatar, $contents);`. 

```php
$url = parse_url($_POST['userpicpersonal']);
$contents = fetch_web_data('http://' . $url['host'] . (empty($url['port']) ? '' : ':' . $url['port']) . str_replace(' ', '%20' trim($url['path'])));`
```
 If you are running an outdated SMF it is recommended to update to SMF 2.0.6 or 1.1.19.