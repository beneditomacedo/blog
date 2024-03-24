---
title:  "Funções Hash"
date:   2024-03-14 13:04:01:01 -0300
categories: 
- ciberseguranca
tags: 
- nivel-basico hashing
---

Em geral, as funções **hash** mapeiam entradas de qualquer tamanho em saídas de tamanho fixo. Essas funções possuem aplicação em diversas áreas da computação, desde as tabelas **hash**, passando pelo uso interno nas estruturas de dicionários e conjuntos, e terminando na busca de conteúdos semelhantes.

![Funções Hash](/assets/images/hash_function.jpeg)

Essas funções são particularmente relevantes na área de segurança da informação, como por exemplo no armazenamento de senhas, onde o hash das senhas são armazenados ao invés das próprias senhas criptografadas; ou em criptografia, onde são utilizadas para assegurar a integridade dos dados, e em mecanismo de assinatura digital. 

Quando utilizadas em segurança da informação, as funções **hash** devem possuir as seguintes características adicionais:
- unidirecional - a partir da saída é quase impossível identificar a entrada; 
- determinística - dada uma entrada sempre gera a mesma saída;
- efeito avalanche - uma pequena alteração na entrada geram muitas alterações na saída;
- resistencia a colisão - é praticamente impossivel achar duas entradas com a mesma saída.

O comando nativo do python **hash** não foi projetado para ser usado em segurança da informação, como podemos ver nas linhas abaixo, que demonstra que o hash nativo não é determinístico. O valor do hash varia entre as execuções.

{% highlight bash %}
% python3 -c'print(hash("criptografia"))'
7332731731800811246
% python3 -c'print(hash("criptografia"))'
2119057593691967906 
{% endhighlight %}

A biblioteca **hashlib** nativa do python implementa diversas funções **hash** que atendem as características listadas acima. As funções listadas em **hashlib.algorithms_guaranteed** são as que estão presentes nas diversas plataformas.

{% highlight python %}
% python3
>>> import hashlib
>>> hashlib.algorithms_available
{'shake_128', 'md5-sha1', 'sha384', 'shake_256', 'blake2s', 'sha256', 'dsaEncryption', 'whirlpool', 'GOST R 34-11-2012 (512 bit)', 'sha3_512', 'sha224', 'ecdsa-with-SHA1', 'sha1', 'sha3_384', 'sha512', 'sha3_224', 'sha3_256', 'GOST 28147-89 MAC', 'GOST R 34.11-94', 'ripemd160', 'md4', 'GOST R 34.11-2012 (256 bit)', 'dsaWithSHA', 'blake2b', 'md5'}
>>> hashlib.algorithms_guaranteed
{'shake_128', 'sha384', 'shake_256', 'blake2s', 'sha512', 'sha3_224', 'sha3_256', 'sha256', 'blake2b', 'sha3_512', 'sha224', 'md5', 'sha1', 'sha3_384'}
>>> 
{% endhighlight %}

A tabela abaixo sumariza as principais funções **hash** <sup id="a1">[1](#f1)</sup>

![Comparação entre Funções Hash](/assets/images/Comparacao_Hash_Functions.png) 

As funções MD5 e SHA-1 não devem ser utilizadas em aplicações de segurança da informação, pois já foi demonstrando que **não são seguras**. 

A recomendação de uso é SHA-256, pois possui um bom suporte e é considerada segura. Para aplicações que demandam um nível de segurança mais alto, deve ser usado o SHA3-256. A função BLAKE2 é recomendada para grandes volumes de dados.  <sup id="a2">[2](#f2)</sup>

<br>
<span style="font-size: 0.6em;">Fontes:<br>
<b id="f1">1</b> - [Wikipedia SHA-3](https://en.wikipedia.org/wiki/SHA-3<br>
<b id="f2">2</b> - [Byrne, Dennis. Full Stack Python Security. Manning Publications, 2021. Web. 15 Oct. 2022](https://www.manning.com/books/full-stack-python-security)