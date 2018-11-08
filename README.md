# SiebelAppletControlCaption
Often when developing in Sibel requirement to show messages about the Applet control in the code. When this occurs, the problem is to indicate in the message the name of the control as it is displayed on the GUI.
This is especially acute in multilingual projects.
It is proposed to take the field captions from the applet, which is based on BC. To do this, perform the following actions:
1. Createion BC [VIF Repository View Web Template Item], based on [Repository View Web Template Item] with the addition of the [BusComp] field.
2. Creation BO [VIF Fields Caption] with two BCs: [VIF Repository View Web Template Item] and [SRF Applet Controls for Task Admin]
3. Create a BS [VIF Fields Caption BS] with a function that searches in the repository for all the applets that are based on the interesting BC and are used in the interesting view. After that, for each applet found, run through all the GUI elements related to the fields and find the need one.
All objects are in the archive.
In BS The code calls the GetFieldsCaption function with three parameters: 
- ViewName is the name of the View for which the applet of interest is located 
- BcName is the name of the BC on which this applet is based
- FieldList is Siebel PS, in which the properties with the BC field on the applet will be recorded, the value is Caption for Control and Display Name for List Column.
Notes:
1. For this work correctly, View with name ViewName name must be in the repository on the server, BC and the applet is not necessary, the presence in the SRF is sufficient.
2. The search is executed on all applets based on BC in view, therefore if one or several applets contain the same field (even if it is not displayed on the applet), but with different signatures, only one of them will be returned, regardless of whether Control is or List Column.
3. A search is executed for applets explicitly prescribed for View, so Toggle Applets will not be counted. To solve the problem, you can use non-displayable elements on applets, for example List Column for Form Applets and Controls for List Applet.
Example of use on BC in a separate file.

-------------------------------------------------------------

Часто разрабатывая на Сибеле приходится в коде показывать сообщения о полях BC. При этом возникает проблема указать в сообщении имя контроля как он отображается на GUI.
Особенно остро это ощущается в многоязычных проектах.
Предлагается брать названия полей с апплета, который базируется на BC. Для этого выполняем следующие действия:
1. Создается BC [VIF Repository View Web Template Item] на базе [Repository View Web Template Item] с добавлением поля [BusComp]
2. Создается BO [VIF Fields Caption] c двумя BC: [VIF Repository View Web Template Item] и [SRF Applet Controls for Task Admin]
3. Создаем BS [VIF Fields Caption BS] с функцией, которая ищет в репозитории все апплеты, стоящие на мнтересуюшем BC и используемые в интересуюшем view. После этого для каждого найденого апплета пробегаемся по всем элементам GUI, относящимся к полям и находим нужное.
Все объекты находятся в архиве.
В коде вызывается функция GetFieldsCaption с тремя параметрами: ViewName - название View для в котором находится интересующий апплет и BcName - имя BC на котором этот апплет базируется. Третий параметр: FieldList - это Siebel PS, в который будут записаны саойства с именами поле BC на апплете, значение - Caption для Control и Display Name для List Column. 

Примечания:
1. Для корректной работы View с именем ViewName должен быть в репозитории на сервере, BC и апплет не обязательно, достаточно наличие в SRF.
2. Поиск выполняется по всем апплетам базируюшимся на BC, следовательно если на одном или нескольких апплетах встречается одно и тоже поле (даже если оно не отображается на апплете), но с разными подписями, будет возвращено только одно из них в независимости от того Control это или List Column.
3. Поиск выполняется для апплетов явно прописанных для View, поэтому Toggle Applets учитываться не будут. Для решения проблемы можно использовать неотображаемый элементы на апплетпах, например List Column для Form Applets и Controls для List Applet.
Пример использования на BC в отдельном файле.
