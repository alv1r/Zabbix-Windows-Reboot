# Zabbix-Windows-Reboot
Утилиты для получения от машин под управлением OS Windows информации о необходимости перезагрузки.
Скрипт взят у https://social.technet.microsoft.com/profile/brian%20wilhite/
За помощь спасибо @count0ru
Мониторинг необходимости перезагрузки Windows.

Бывает полезно знать, каким серверам требуется перезагрузка, а когда их много, то удобно об этом узнавать, не заходя на сервер.
Для мониторинга нам понадобится шаблон, клиент, настроенный для активных проверок и соответствующий скрипт.

Если на клиенте не разрешено выполнение скриптов, выполните нижеследующую команду:
set-executionpolicy remotesigned

Полученный определяем в папку \zabbix_agent\scripts на клиенте, в конфигурации пишем:
>UserParameter=Reboot.IsNedeed,powershell -NoProfile -ExecutionPolicy Bypass -command "$ErrorActionPreference = 'silentlycontinue';  $eval = get-pendingreboot; if ($eval.RebootPending) { Write-Host '1'; } else { Write-Host '0' };

Импортируем шаблоне в Zabbix. Шаблон настроен так, чтобы, получив информацию о необходимости перезагрузки, выдать оповещение, а затем, когда полученная информаця говорит о том, что перезагрузка больше не требуется - сам закроет проблему.
