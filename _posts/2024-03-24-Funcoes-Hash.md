---
title:  "Funções Hash"
date:   2024-03-24 13:04:01:01 -0300
categories: 
- ciberseguranca
tags: 
- nivel-basico 
- hashing
---

As funções hash mapeiam entradas de qualquer tamanho em saídas de tamanho fixo. São consideradas funções unidirecionais, pois é muito custoso ou impossível fazer a operação inversa. Essas funções possuem aplicação em diversas áreas da computação, dentre elas: tabelas hash, implementação de dicionários e conjuntos em linguagem de programação, busca de conteúdos semelhantes.

![Funções Hash](/blog/assets/images/hash_function.jpeg)

Essas funções são particularmente relevantes na área de segurança da informação, como por exemplo no armazenamento de senhas, onde o hash das senhas são armazenados ao invés das próprias senhas criptografadas; ou em criptografia, onde são utilizadas para assegurar a integridade dos dados e para a assinatura digital. 

O sistema operacional Debian possui comandos para geração de diversos algoritmos hash, conforme tabela abaixo:

| Algoritmo  | Comando   | 
|------------|-----------|
| MD5        | md5sum    |   
| SHA-1      | sha1sum   |   
| SHA-224    | sha224sum |   
| SHA-256    | sha256sum |      
| SHA-384    | sha384sum |  
| SHA-512    | sha512sum |
| BLAKE2     | b2sum     |  

Abaixo, temos a geração de hash do MD5 da palavra 'criptografia'. Independentemente do tamanho da palavra, o hash MD5 tem 32 caracteres hexadecimal. Observe que basta a mudança de um único caracter para que o hash seja completamente diferente. Esse é o efeito avalanche, que é uma característica dos algoritmos hash. 

{% highlight bash %}
% echo 'criptografia' | md5sum
388d4d2a53263c3f3939985a4feb8e38  -
$ echo 'criptografiA' | md5sum
8b9d031dc44d789a22281ec683a26fdb  -
{% endhighlight %}

Os algoritmos MD5 e SHA-1 não devem ser utilizados em aplicações de segurança da informação, pois **não são seguros**, conforme recomendação da seção BUGS do manual *md5sum* do Debian. Os algoritmos recomendados para aplicações de segurança são: SHA-2 (nas suas diversas variações sha-224, sha-384 ou sha-512) ou BLAKE2. <sup id="a1">[1](#f1)</sup>

Quando utilizadas em segurança da informação, os algoritmos **hash** devem possuir as seguintes características:
- unidirecional - a partir da saída é quase impossível identificar a entrada; 
- determinístico - dada uma entrada sempre gera a mesma saída;
- efeito avalanche - uma pequena alteração na entrada geram muitas alterações na saída;
- resistencia a colisão - é praticamente impossivel achar duas entradas com a mesma saída.

O comando nativo do python **hash** não foi projetado para ser usado em segurança da informação, como podemos ver nas linhas abaixo, que demonstra que o hash nativo não é determinístico. O valor do hash varia entre as execuções. <sup id="a2">[2](#f2)</sup>

{% highlight bash %}
% python3 -c'print(hash("criptografia"))'
7332731731800811246
% python3 -c'print(hash("criptografia"))'
2119057593691967906 
{% endhighlight %}

A biblioteca **hashlib** nativa do python implementa diversas funções **hash** que atendem as características listadas acima. Como podemos ver abaixo, essa função é determinística, pois as duas execuções do programa ex_hashlib.py gera o mesmo hash. 

{% highlight bash %}
% cat ex_hashlib.py 
import hashlib
print(hashlib.sha256(b"criptografia").hexdigest())
%
%
% python3 ex_hashlib.py
d93449f3e5b4bc1fb096a29c2fe7cb71b2694f1436f738741c35950fdb36fbaf
% python3 ex_hashlib.py
d93449f3e5b4bc1fb096a29c2fe7cb71b2694f1436f738741c35950fdb36fbaf
% 
{% endhighlight %}

As funções listadas em **hashlib.algorithms_guaranteed** são as que estão presentes nas diversas plataformas.

{% highlight python %}
% python3
>>> import hashlib
>>> hashlib.algorithms_available
{'shake_128', 'md5-sha1', 'sha384', 'shake_256', 'blake2s', 'sha256', 'dsaEncryption', 'whirlpool', 'GOST R 34-11-2012 (512 bit)', 'sha3_512', 'sha224', 'ecdsa-with-SHA1', 'sha1', 'sha3_384', 'sha512', 'sha3_224', 'sha3_256', 'GOST 28147-89 MAC', 'GOST R 34.11-94', 'ripemd160', 'md4', 'GOST R 34.11-2012 (256 bit)', 'dsaWithSHA', 'blake2b', 'md5'}
>>> hashlib.algorithms_guaranteed
{'shake_128', 'sha384', 'shake_256', 'blake2s', 'sha512', 'sha3_224', 'sha3_256', 'sha256', 'blake2b', 'sha3_512', 'sha224', 'md5', 'sha1', 'sha3_384'}
>>> 
{% endhighlight %}

A tabela abaixo sumariza as principais funções hash <sup id="a3">[3](#f3)</sup>

![Comparação entre Funções Hash](/blog/assets/images/Comparacao_Hash_Functions.png) 

<br>
<span style="font-size: 0.6em;">Fontes:<br>
<b id="f1">1 - [md5sum - Linux man page, Debian, 2024](https://manpages.debian.org/bookworm/coreutils/md5sum.1.en.html)</b> <br>
<b id="f2">2 - [Byrne, Dennis. Full Stack Python Security. Manning Publications, 2021. Web. 15 Oct. 2022](https://www.manning.com/books/full-stack-python-security)</b>  
<span>