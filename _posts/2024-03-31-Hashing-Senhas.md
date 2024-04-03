---
title:  "Hashing de Senhas"
date:   2024-03-31 19:00:00:00 -0300
categories: 
- ciberseguranca
tags: 
- nivel-basico
- hashing 
- senhas
---

Antes da década de 1980, as senhas dos usuários de sistemas operacionais unix-like eram armazenadas no arquivo */etc/passwd*. A partir de 1980, as senhas passaram a ser armazenadas em um arquivo */etc/shadow* com acesso somente ao usuário root, acrescentando mais um nível de segurança ao sistema. 

O arquivo */etc/shadow/* possui diversos campos separados por dois pontos(:), como podemos ver no exemplo abaixo: 

{% highlight bash %}
% sudo cat /etc/shadow
root:!$y$j9T$wkFQVZ1JpGQCinpMpXtW91$bT1SupDrHVcqYUCf8Xpgm4BKe5rNaaRjKgkCA/9bGe8:19537:0:99999:7:::
daemon:*:19537:0:99999:7:::
bin:*:19537:0:99999:7:::
sys:*:19537:0:99999:7:::
sync:*:19537:0:99999:7:::
games:*:19537:0:99999:7:::
man:*:19537:0:99999:7:::
lp:*:19537:0:99999:7:::
mail:*:19537:0:99999:7:::
news:*:19537:0:99999:7:::
uucp:*:19537:0:99999:7:::
proxy:*:19537:0:99999:7:::
www-data:*:19537:0:99999:7:::
backup:*:19537:0:99999:7:::
list:*:19537:0:99999:7:::
irc:*:19537:0:99999:7:::
_apt:*:19537:0:99999:7:::
nobody:*:19537:0:99999:7:::
systemd-network:!*:19537::::::
tss:!:19537::::::
systemd-timesync:!*:19537::::::
messagebus:!:19537::::::
usbmux:!:19537::::::
dnsmasq:!:19537::::::
avahi:!:19537::::::
speech-dispatcher:!:19537::::::
fwupd-refresh:!:19537::::::
saned:!:19537::::::
geoclue:!:19537::::::
polkitd:!*:19537::::::
rtkit:!:19537::::::
colord:!:19537::::::
gnome-initial-setup:!:19537::::::
Debian-gdm:!:19537::::::
abc:$y$j9T$B05EC6RY73kndd7RsNTR41$c/Qxizve4HXk3yE7rzKmeWzR7rtmXDdKaCtH6eDrPD0:19810:0:99999:7:::
xyz:$y$j9T$gYl8mI4pdchHBr9tONgBB.$i5iwS5RMqqXO9toz9KZsdJ.WvMEzz.XIhU5knHP0r20:19813:0:99999:7:::
{% endhighlight %}

**Não é recomendável exibir o contéudo de arquivos contendo o hash de senhas, pois essas informações podem ser utilizadas em ataques.**

Os dois primeiros campos de cada linha são o nome do usuário e as informações de hashing das senhas. Os demais campos estão relacionados a alteração e expiração das senhas. No arquivo acima temos diversos usuários criados na instalação do sistema, que não permitem o login em uma interface de acesso, logo não tem informações de hashing, com exceção das duas últimas linhas, contendo os usuários *abc* e *xyz*.

O formato do campo de hash depende do algoritmo de hashing de senhas que o sistema operacional e/ou distribuição utiliza. Os algoritmos *blowfish(1993)* e *bcrypt(1999)* foram alguns dos primeiros algoritmos dos sistemas unix-like. Atualmente diversos algoritmos tais como: *yescrypt*, *script*, *pbkdf2*.

O *yescrypt* é um algoritmo utilizado ALT Linux, Arch Linux, Debian 11+, Fedora 35+, Kali Linux 2021.1+, Ubuntu 22.04+, é uma evolução do scrypt <sup id="a1">[1](#f1)</sup>. Possui grande resistencia a ataques offline quando comparado com os algoritmos *scrypt* <sup id="a2">[2](#f2)</sup> e *Argon2*.

Na distribuição Debian 6.1.27-1O, o formato do campo hash no arquivo */etc/shadow* tem 4 componentes: prefixo, opções, salto, hash, separados por um sinal de *\$* Os componentes opções, salto e hash dependem do prefixo. Dependendo do prefixo, os campos opções e salto podem ser vazios. 

Na tabela abaixo, temos os algoritmos suportados em ordem de resistencia a ataques<sup id="a3">[3](#f3)</sup>.

| Algoritmo      | Prefixo | Tamanho do Salto | Tamanho do Hash |
|----------------|  :---:  |      :---:       |      :---:      |
| yescrypt       |       y |   até 512 bits   |     256 bits    |
| gost-yescrypt  |      gy |   até 512 bits   |     256 bits    |
| scrypt         |       7 |   até 512 bits   |     256 bits    |
| bcrypt         |      2b |      128 bits    |     184 bits    |
| sha512crypt    |       6 |   6 a 96 bits    |     512 bits    |
| sha256crypt    |       5 |   6 a 96 bits    |     256 bits    |
| sha1crypt      |    sha1 |   6 a 384 bits   |     160 bits    |
| sunMD5         |     md5 |      48 bits     |     128 bits    |
| md5crypt       |       1 |   6 a 48 bits    |     128 bits    |
| bsdicrypt      |       - |      24 bits     |      64 bits    |
| bigcrypt       |      "" |      12 bits     |    1024 bits    |
| descrypt       |      "" |      12 bits     |      64 bits    |
| NT             |       3 |         -        |     256 bits    |

No ínicio do campo hashing dos usuários *abc* e *xyz*, podemos notar *\$y\$*, onde o \$ é o separador de campo do hashing e o prefixo *y*, que informa que o algoritmo é o *yescript*. 

<br>
<span style="font-size: 0.6em;">Fontes:<br>
<b id="f1">1 - [C. Percival, The scrypt Password-Based Key Derivation Function, RFC 7914, Internet Engineering Task Force, Aug. 2016.] (https://datatracker.ietf.org/doc/html/rfc7914)</b><br>
<b id="f2">2 - [yescrypt - scalable KDF and password hashing scheme] (https://www.openwall.com/yescrypt/)</b><br>
<b id="f3">3 - [crypt(5) - Linux man page, Debian, 2024] (https://manpages.debian.org/bookworm/libcrypt-dev/crypt.5.en.html)</b>
</span>
