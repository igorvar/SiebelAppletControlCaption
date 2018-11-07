# SiebelAppletControlCaption
Часто разрабатывая на Сибеле приходится в коде показывать сообщения о полях BC. При этом возникает проблема указать в сообщении имя контроля как он отображается на GUI.
Особенно остро это ощущается в многоязычных проектах.
Предлагается брать названия полей с апплета, который базируется на BC.
1. Создается BC [VIF Repository View Web Template Item]
2. Создается BO [VIF Fields Caption] c двумя BC: [ITO Repository View Web Template Item] и [SRF Applet Controls for Task Admin]
3. Создаем BS [VIF Fields Caption BS].
Все объекты находятся в архиве.
В коде вызывается функция GetFieldsCaption с тремя параметрами: ViewName - название View для в котором находится интересующий апплет и BcName - имя BC на котором этот апплет базируется. Третий параметр: FieldList - это Siebel PS, в который будут записаны саойства с именами поле BC на апплете, значение - Caption для Control и Display Name для List Column. 
Примечания:
1. Для корректной работы View именем ViewName должен быть в репозитории на сервере, BC и апплет не обязательно, достаточно наличие в SRF.
2. Поиск выполняется по всем апплетам базируюшимся на BC, следовательно если на одном или нескольких апплетах встречается одно и тоже поле (даже если оно не отображается на апплете), но с разными подписями, будет возвращено только одно из них в независимости от того Control это или List Column.
3. Поиск выполняется для апплетов явно прописанных для View, поэтому Toggle Applets учитываться не будут. Для решения проблемы можно использовать неотображаемый элементы на апплетпах, например List Column для Form Applets и Controls для List Applet.

Пример использования на BC:
declaration section:
var _FieldsCaption:PropertySet; //Property set global for this BC

function GetFieldCaption(FieldName)
{
    if (_FieldsCaption == null)
    {
        _FieldsCaption = TheApplication().NewPropertySet();
        TheApplication().GetService("VIF Fields Caption BS").GetFieldsCaption(
            TheApplication().ActiveViewName(), 
            this.Name(), 
            _FieldsCaption)
     }

    return _FieldsCaption.GetProperty(FieldName);
}

function BusComp_PreWriteRecord()
{
    if (this.GetFieldValue("PhoneNumber") == ""
        TheApplication().RaiseErrorText("Field " + GetFieldCaption("PhoneNumber") + " is mandatory")
}
