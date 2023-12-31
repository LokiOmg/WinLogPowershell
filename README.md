# WinLogPowershell
Функция записи кастомных логов в Event Log Windows

<#
# Пример записей событий в наш лог
### Пример 1
Объявляем список логируемых атрибутов
$EventData = @(
    "Номер группы в списке1: $($List_Groups1.GroupID)",
    "Номер группы в списке2: $($List_Groups2.GroupID)",
    "Название группы в списке1: $($List_Groups1.Name)",
    "Название группы в списке2: $($List_Groups2.Name)"
    )
$EventHead = @("Группа $($List_Groups1.GroupID) присутствует в двух списках.Имена групп совпадают")
# Вызываем запись в лог где evnID - код события, EvType - уровень события, Head - имя события, EventData - тело события
WinLog -evtID 7010 -EvType 'INFO' -Head $EventHead -EventData $EventData


### Пример 2
Объявляем список логируемых атрибутов
$EventData = @(
    "Имя пользователя: $($user.Name)",
    "Название группы в AD: $($find_group.Name)",
    )
$EventHead = @("Пользователь $($user.Name) не является членом группы в AD $($find_act_group.name).Необходимо добавить в группу!")
# Пишем в лог
WinLog -evtID 1001 -EvType 'WARN' -Head $EventHead -EventData $EventData
# Выполняем необходимое действие
try{
    Add-ADGroupMember -Identity $find_group -Members $user
    $EventHead = @("Пользователь $($user.Name) добавлен в группу AD $($find_group.Name)")
    WinLog -evtID 1002 -EvType 'INFO' -Head $EventHead -EventData $EventData
}
catch{
    $EventHead = @("Ошибка добавления пользователя $($user.Name) в группу $($find_group.Name): $($_.Exception.ToString())")
    WinLog -evtID 1003 -EvType 'ERR' -Head $EventHead
}
### Конец примеров
#>
