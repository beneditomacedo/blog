---
title:  "Variaveis de Ambiente do Shell"
date:   2024-03-15 01:13:00 -0300
categories:
- linux
tags:
- shell 
---
O shell é o interpretador de comandos do Linux. A criação de variáveis de ambiente do shell configuram e/ou controlam a execução de comandos e utilitários. Por convenção, as variáveis de ambiente são criadas em letras maiscúlas. O shell diferencia variáveis em letras maiscúlas e minúsculas. 

O comando *echo* permite exibir o conteúdo de uma variável de ambiente. **Quando a variável nao existe, o shell não exibe nenhuma mensagem de erro.** Nesse caso, é exibida uma linha em branco, como podemos ver no exemplo abaixo. Logo em seguida, um valor é atribuido à variável PORT, e seu valor é exibido.

{% highlight bash %}
# echo $PORT

# PORT=3000
# echo $PORT
3000
# 
{% endhighlight %}

Para remover uma variável do ambiente use o comando *unset*.

{% highlight bash %}
# echo $PORT
3000
# unset PORT
# echo $PORT

# 
{% endhighlight %}

Quando um comando ou utilitário é iniciado em sistemas Unix-like ou Linux, um processo shell filho é criado, que herda as variáveis **exportadas**. O comando *sh* inicia um shell filho. Simplesmente criar uma variável, como podemos ver no exemplo abaixo, não exporta a variável para o shell filho.  

{% highlight bash %}
# PORT=3000
# echo $PORT
3000
# sh
# echo $PORT

# 
{% endhighlight %}

O comando *export* torna a variável disponível para o seu filho, como podemos ver no exemplo abaixo. 

{% highlight bash %}
# PORT=3000
# echo $PORT
3000
# export PORT
# sh
# echo $PORT
3000
# 
{% endhighlight %}

<br><br>
<sup> 
A versão do Linux é LinuxKit 4.9.125
</sup>
