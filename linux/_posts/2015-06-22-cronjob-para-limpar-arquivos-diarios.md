---
layout: post
title: Criando um cronjob para limpar arquivos diários
comments: true
---

As vezes, é necessário manter em alguma pasta arquivos de backup diários, gerados por ferramentas como o `mysqldump`, `zip` etc. Algumas aplicações também criam arquivos de log diários, ex. `logfile-2015-06-22.log`. O ideal no caso dos logs é alterar o comportamento da aplicação, para que crie apenas um arquivo de log, e utilizar a ferramenta `logrotate` ou algum utilitário de log para manter o tamanho do arquivo sob controle. Como o ideal nem sempre é possível, também podemos utilizar o `find` e um cronjob para não deixar os logs neste formato acumularem.

No caso dos arquivos de log, adicione uma linha como esta no final do arquivo `/etc/crontab`:

{% highlight bash %}
30  4  *  *  *  username  find /logs/dir/*.log -mtime +10 | xargs rm
{% endhighlight %}

Aqui estamos dizendo para que todo dia às 4:30 o cron execute um comando `find` que liste todos os arquivos com a última modificação há mais de 10 dias e utilize cada linha da saída deste como argumento para o comando `rm`.

O resultado do `find` é algo neste formato:

{% highlight bash %}
/logs/dir/app-2015-06-10.log
/logs/dir/app-2015-06-11.log
/logs/dir/app-2015-06-12.log
{% endhighlight %}

O parametro `-mtime` indica que queremos arquivos apenas com a data de modificação no período `N*24h` especificado. Os valores possível são, por exemplo: `2` quer dizer arquivos modificados há exatamente dois dias, `+2`, modificados a mais de dois dias, e `-2`, há menos de dois dias.

O `xargs` vai utilizar cada linha desta saída como argumento do `rm`, resultando em:

{% highlight bash %}
rm /logs/dir/app-2015-06-10.log
rm /logs/dir/app-2015-06-11.log
rm /logs/dir/app-2015-06-12.log
{% endhighlight %}

Para backups, se você mantem em um diretório os resultados diários de um dump do banco de dados com `mysqldump` e uma cópia compactada com o `zip` da pasta de uploads de um site, pode adicionar ao cron algo como isto:

{% highlight bash %}
30  4  *  *  *  username  find /backup/dir/* -mtime +10 | egrep '(.zip|.sql)$' | xargs rm
{% endhighlight %}

Estamos fazendo exatamente a mesma coisa do exemplo anterior, acrescentando um filtro com expressão regular para listar apenas os arquivos `.zip` ou `.log`, evitando assim qualquer exclusão acidental.
