### linux中联合ps -ef与kill -9杀掉进程

ps -ef|grep artisan|awk '{print $2}'|xargs kill -9