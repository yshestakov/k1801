## 1801ВП1-128

### Фотографии кристалла высокого разрешения
[1801ВП1-128, 68M](http://www.1801bm1.com/files/retro/1801/images/vp1-128.jpg)

### Условное графическое обозначение
![Symbol](/128/img/128.png)

### Назначение выводов
| Номер | Название   | Конфигурация | Назначение
|-------|------------|--------------|-----------------------------------------
| 1-16  | nAD0-nAD15 | Вых-3/Вход   | Мультиплексированные адрес данные шины МПИ
| 17    | nSYNC      | Вход         | Строб адреса МПИ
| 18    | nDIN       | Вход         | Строб чтения данных МПИ
| 19    | nDOUT      | Вход         | Строб записи данных МПИ
| 20    | nDCLO      | Вход         | Системный сброс, инициализация контроллера
| 21    | GND        | Питание      | Нулевой потенциал (земля)
| 22    | CLC        | Вход         | Основной тактовый сигнал, 4МГц
| 23    | nRPLY      | Выход ОК     | Строб подтверждения транзакции МПИ
| 24    | IND        | Вход         | Вход индексного сигнала с НГМД, высокий уровень активных импульсов
| 25-27 | nD03-nD01  | Выход        | Выходные записываемые данные в разных фазах (для реализации предкомпенсации записи)
| 28    | nWRE       | Выход        | Разрешение записи на НГМД, низкий активный уровень
| 29    | DI         | Вход         | Входные данные читаемые с НГМД
| 30    | REZ        | Выход        | Выход бита 10 регистра управления, используется для управления предкомпенсацией записи
| 31    | WPR        | Вход         | Защита от записи, выскоий уровень запрещает запись
| 32    | RDY        | Вход         | Готовность накопителя/диск сменен
| 33    | IR0        | Вход         | Признак нулевой дорожки от НГМД
| 34    | nST        | Выход        | Команда шаг
| 35    | DIR        | Выход        | Направление шага
| 36    | HS         | Выход        | Выбор поверхности диска
| 37    | nMSW       | Выход        | Включение двигателя НГМД
| 38-41 | DS3-DS0    | Выход        | Выбор накопителя 3-0
| 42    | VCC        | Питание      | Потенциал +5В (источник питания)

### Структурная схема 1801ВП1-128
![Struct](/128/img/struct_128.png)

### Описание
Микросхема 1801ВП1-128 является контроллером накопителей на гибких магнитных
дисках (в быту используется более простое название - "дисковод"). Ранее
компьютеры БК-001х использовали для сохранения программ и данных магнитофон.
С появлением 1801ВП1-128 стала возможной разработка простого, компактного
и массового контроллера НГМД.

Микросхема ВП1-128 фактически является венцом советского БМК-строения
на основе матрицы серии 1801ВП1. Проект 128 - это последний широкоизвестный
номер в серии который пошел в массовое производство. Микросхема является
относительно сложной, использует 308 ячеек матрицы и содержит 525 связей.

Микросхема 1801ВП1-128 обеспечивает запись и чтение информации в(из) 
накопителя на гибких магнитных дисках по методу МФМ. При программной поддержке
со стороны центральной ЭВМ, обеспечивает выработку управляющих сигналов на
накопитель, позволяет осуществлять форматирование дискет по стандарту IBM.
При частоте 4 МГц, подаваемой на вход CLC, микросхема обеспечивает битовую
скорость принимаемых и передаваемых данных 250 килобит в секунду.

1801ВП1-128 содержит следующие функциональные блоки (нумерация ячеек
приведена по восстановленной схеме):
- РС, регистр состояния, содержит всего два триггера - для фиксации сигнала
  готовности (M12) и результатов контрольной суммы/окончания записи (J34).
  Остальные сигналы регистра состояния считываются непосредственно с входов
  микросхемы. Сигналы TR0, RDY вообще никак не влияют на работу остальных 
  блоков микросхемы. Высокий уровень на входе WRP запрещает запись. 
  Сигнал IND на работу микросхемы также влияет очень слабо - никак не прерывает
  режимы поиска маркера, чтения или записи данных. По крайней мере, об этом
  говорят результаты моделирования. Поскольку они несколько странны, то были
  приняты дополнительные усилия по поиску ошибок восстановления схемы,
  никаких ошибок на данный момент не выявлено.
- РДЗ, регистр данных записи, 16-битный региcтр, предназначен для буферизации
  данных при записи информации на диск. На схеме представлен шестнадцатью
  D-триггерами N4, N5, M4, M5, L5, J4, H5. H4, G5, G4, F5, F4, D5. D4, C5 и C4.
  Данные в этот регистр переписываются непосредственно с шины МПИ при обращении
  на запись по адресу 177132. Выход этого регистра через мультиплексор 
  данных записи подключен к сдвиговому регистру, через который записываемые
  данные будут последовательно поступать для записи на гибкий диск.
- РДЧ, регистр данных чтения, шестнадцатибитный регистр, предназначен для
  буферизации данных, принимает два байта данных из сдвигового регистра.
  В процессе считывания данных с диска сначала сохраняется старший байт,
  затем младший данных. Регистр реализован на шестнадцати D-триггерах,
  разбитых на две группы, для старшего (I0, H0, F0, D0, B0, A0, B2, B1)
  и младшего (O10, O6, O4, O1, O0, N0, M0, J0) байта данных соответственно.
- входные элементы, обеспечивают подключение микросхемы двунаправленым
  линиям nAD0-nAD15 системной шины МПИ.
- интерфейсный узел, осуществляет дешифрацию адресов регистров и выработку
  управляющих сигналов для записи и чтения соответствующих регистров,
  а также вырабатывает на шину сигнал RPLY. Основная часть декодера адреса
  реализована на ячейках A3, N3, B3, L4, B4, A5, A6, A7. Из особенностей
  стоить отметить, что микросхема не защелкивает по ниспадающиему фронту
  nSYNC адрес обращения целиком, а только результаты предварительного его
  декодирования. В связи с чем выдвигаются требования к времени предустановки
  адреса относительно ниспадающего фронта nSYNC. Декодируется всего два адреса,
  177130<sub>8</sub> (регистр управления при записи и регистр состояния
  при чтении) и 177132<sub>8</sub> (регистр для чтения-записи 16-битного
  слова данных). Cхема управления сигналом RPLY вырабатывает сигнал в ответ
  на обращение к регистрам контроллера по шине МПИ. Установка данного
  сигнала в активный (низкий) уровень синхронизирована с фронтом тактового
  сигнала CLK. Снятие происходит немедленно после снятия сигналов nDIN
  или nDOUT. Реализована на ячейках B6, B7, B9, B10, B11. Триггер на
  элементах B6, B7, B9 является интересным схемотехническим образчиком,
  на этих трех элементах реализован D-триггер (вход данных на контакт 8
  элемента B6), срабатывающий на ниспадающем фронте тактового сигнала (CLK).
- мультиплексор РДЗ, мультиплексор регистра данных записи. Реализован на
  элементах C7, D7, F7, G7, H7, I7, J7, L7 и представляет собой восьмиканальный
  мультиплексор два-в-один, коммутирует младший и старший байты регистра
  данных записи ко входам сдвигового регистра. 
- сдвиговый регистр, в режиме "запись" преобразовывает параллельный код из
  РДЗ в последовательный, в режиме "чтение" выполняется обратное преобразование.
  Представляет собой восьмибитный сдвиговый регистр, реализован на элементах
  L9, L11, L12, I9, I11, I12, J9, J11, J12, H9, H11, H12, G9, G11, G12, F9,
  F11, F12, D9, D11, D12, C9, C11, C12. Может работать или в режиме сдвига
  (только в одну сторону - от младших разрядов к старшим) или в режиме
  параллельной загрузки. Стоит отметить, что старший бит байта данных
  записывается или считывается первым.
- демультиплексор данных чтения, поочередно подключает старший и младший
  байты шестнадцатибитового регистра данных чтения.
- демультиплексор регистра данных чтения к выходам
  воcьмибитного сдвигового регистра. Наличие этого блока явно избыточно,
  достаточно просто соединить регистры напрямую, поскольку байт сдвигового
  регистра непосредственно выдается одновременно на оба байта регистра данных
  чтения, а логика работы реализуется через стробы записи. Поэтому,
  по мнению автора, элементы C2, D2, F2, G2, H2, I2. L2, M2 являются
  избыточными и могли бы быть удалены из схемы.
- мультиплексор чтения (O11, O7, O3, L0, A1, A2) подключает к шине данных МПИ
  или регистр данных чтения или регистр/флаги состояния в зависимости от
  адреса обращения
- CRC генератор, вырабатывает циклический контрольный код в соответствии
  с полиномом G(x) = x<sup>0</sup>+x<sup>5</sup>+x<sup>12</sup>+x<sup>16</sup>.
  Контрольный код приписывается в конце процедуры записи к порции информации.
  При чтении CRC-генератор осуществляет проверку считанной информации на
  достоверность. Генератор CRC занимает он целых 48 ячеек, почти одну шестую 
  часть всей микросхемы, и представляет собой шестнадцатибитный сдвиговый
  регистр с обратными связями, реализован на длинной цепочке RS триггеров
  с двумя противофазными тактами.
- схему кодирования МФМ, кодирует последовательный битовый поток данных
  по методу МФМ в импульсы длительностью 500нс на выходах DO1-DO3.
  Данные выходы, соответствуют значениям "номинальный импульс", 
  "запаздывающий импульс" и "опережающий импульс" и предназначены для
  организации работы схемы предкомпенсации записи. Результаты моделирования
  показывают что фаза импульсов является постоянной и импульс может появиться
  только на одном из этих выходов. Поэтому использование элементов "И"
  в схемах многих контроллеров НГМД на ВП1-128 выглядит ненужным и избыточным,
  вполне достаточно одного сдвигового регистра. Также имеется возможность
  пропуска синхроимпульсов при записи адресного маркера или маркера данных.
- схему декодирования МФМ (G32) преобразует закодированную информацию в вид,
  удобный для дальнейшего преобразования и является достаточно простой,
  схема синхронизации вырабатывает меандр, фаза которого привязана к потоку
  импульсов данных чтения. Частота этого меандра соответствует битовой
  частоте 250кГц. Если в течение высокого уровня данного сигнала поступил
  импульс данных чтения, то фиксируется единичное значение бита данных,
  если импульса на входе nDI не было, то фиксируется нулевое значение.
  Таким образом, только МФМ-импульсы в середине битового интервала влияют
  на формирование данных. МФМ-импульсы на границе интервала участвуют
  только в управлении схемой ФАПЧ для подстройки битовой частоты.
- схема синхронизации импульсов данных чтения собрана на элементах C39,
  D39, D38, G27, F35. На вход поступают положительные импульсы
  с инвертированного входа nDI. На выходе генерируются импульсы длительностью
  один период тактовой частоты CLK (250нс), передний фронт которых
  синхронизирован с ниспадающим фронтом CLK. Также содержит содержит генератор
  с подсистемой фазовой автоподстройки частоты и схемы формирования различных
  синхроимпульсов. В режиме чтения выход ФАПЧ находится в фазовом соотвествии
  с импульсами данных чтения. Если таковые импульсы отсутствуют или схема
  находится в режиме записи, то схема находится в свободном режиме 
  (free-running) и на выходах генерируются повторяющиеся сигналы с точным 
  периодом 250кГц.
- схему распознавания маркера, постоянно проверяет значение сдвигового
  регистра на соответствие коду 0xA1 (элементы I5, D13). Также имеется
  теневой четырехбитный сдвиговый регистр (F25, H23, I23, J23), который
  детектирует имульсы данных чтения на границе битовых интервалов и работает
  на том же принципе что и схема декодирования читаемых данных, только
  захватывает импульсы в противофазе. Захваченное значение также постоянно
  анализируется на предмет пропущенного синхроимпульса в младших четырех
  битах маркера 0xA1. Таким образом, можно утверждать что при чтении
  микросхема распознает только маркер 0xA1 с пропущенным синхробитом.
  При обнаружении маркера происходит запуск микросхемы на преобразование
  информации.
- регистр управления, содержит десять D-триггеров (O23, O27, O32, O36, O38,
  O39, N39, G39, F39, G36), которые сохраняют данные записываемые при
  обращении по адресу 177130<sub>8</sub>. Разряды DS0-DS3, MSW, HS, DIR,
  REZ отображаются с инвертированием на соответствующие внешние выходы
  микросхемы и более никакого влияния на работу остальных блоков не оказывают.
  Разряд ST фактически в отдельном триггере не запоминается, вместо этого
  на выходе ST формируется импульс отрицательной полярности в момент
  записи единичного значения в данный разряд. Поэтому последующая запись
  в регистр управления с обнулением значения разряда не нужна. Состояние
  разряда GDR дополнительно стробируется импульсами данных чтения.
  Поэтому если ранее был установлен GDR и с дисковода более не поступают
  импульсы чтения (допустим, вставили неформатированную дискету), то запись
  осуществить невозможно (по крайней мере без подачи активного низкого nINIT),
  так как внутрений стробируемый GDR зафиксировался в активном состоянии
  и удерживает часть схемы синхронизации в сбросе.

Микросхема 1801ВП1-128 имеет вход nINIT, подача низкого уровня на который
вызывает приведение всех узлов микросхемы в исходное состояние. В частности,
при этом разряды 7(TR) и 14(CRC), а также все  доступные по записи разряды
регистра состояния сбрасываются в нулевые значения.

Микросхема подключается к системе при помощи шины МПИ. По отношению к 
центральному процессору контрроллер является пассивным устройством,
имеющим  регистры состояния и данных. Разрядность регистров 16 бит
со словным режимом обмена.

#### Внутренние регистры 1801ВП1-128
| Адрес | Описание
|-------|--------------------------------------
| 177130<sub>8</sub> | Регистр состояния
| 177132<sub>8</sub> | Буферный регистр данных

#### Формат регистра состояния 177130<sub>8</sub>, запись
| Бит | Описание
|-----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| 0   | DS0, выбор накопителя 0
| 1   | DS1, выбор накопителя 1
| 2   | DS2, выбор накопителя 2
| 3   | DS3, выбор накопителя 3
| 4   | MSW, включение двигателя накопителя
| 5   | HS, выбор поверхности 
| 6   | DIR, выбор направления шага головки
| 7   | ST, шаг, запись единичного значения генерирует импульс
| 8   | GDR, признак "начало чтения"
| 9   | WM, признак "запись маркера"
| 10  | REZ, резервный, на некоторых платах контроллеров управляет предкомпенсацией записи

#### Формат регистра состояния 177130<sub>8</sub>, чтение
| Бит | Описание
|-----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| 0   | TR0, признак "0-я дорожка"
| 1   | RDY, признак "накопитель готов к работе"
| 2   | WPR, признак "запрет записи"
| 7   | TR, признак "готовность" регистра данных
| 14  | CRC, при чтении данных с диска признак "прочлось без ошибок", при записи на диск признак "записывается сумма CRC на диск"
| 15  | IND, сигнал индекса с НГМД

#### Особенности поведения флагов в регистре состояния
В режиме "запись на диск" 7-й разряд регистра состояния (TR) устанавливается
в единицу после того, как младший байт регистра данных записи переписался в
сдвиговый регистр.

В режиме "чтение с диска" 7-й разряд регистра состояния (TR) устанавливается
в единицу после того, как в РДЧ сформировалось очередное считанное из
накопителя слово.

Сброс TR происходит при обращении к регистру данных по записи или по чтению
со стороны МПИ, а также по активному низкому уровню на входе nINIT.

При тактовой частоте 4 МГц, подаваемой на вход CLC слово преобразуется за 64
мкс. С этим периодом по признаку TR центральный процессор должен прочитать
или записать дданные из/в буферного регистра данных. Иначе операции 
запись/чтение диска производятся некорректно.

В режиме "запись на диск" 14-й разряд регистра состояния (CRC) служит для
индикации ситуации, когда текущее требование не было обслужено, а возникли
условия на формирование нового требования. Она является сбойной, 
если возникает в середине цикла записи на диск. При этом микросхема
прекращает формирование циклического кода и записывает полученный
контрольный код на диск.

В режиме "чтение с диска" 14-й разряд РС (CRC) служит признаком некорректности
выполненного чтения.

Микросхема 1801ВП1-128 формирует пропуск синхроимрульсов информации
при наличии "1" в 9-ом  разряде регистра состояния (WM).

Микросхема опознает маркер, и, тем самым, запускается на чтение 
информации с диска, если обнаружен пропуск синхроимпульсов в 
коде А1 (10100001).

При записи "1" в 8-й разряд регистра состояния (GDR) микросхема частично 
приводится в исходное состояние, а по ее снятию готова осуществлять поиск
маркера. Этим разрядом рекомендуется пользоваться для принудительной
синхронизации схемы синхронизации со считываемыми данными, причем желательно
записать "1", а затем "0" в этот разряд в области зоны нулей
(зона обычно предшествующая маркеру).

### Дополнительные подробности
При реверсе cхема ФАПЧ оказалась особенно сложной. Эта схема содержит около
двух десятков отдельных триггеров, каждый из которых собран на "рассыпных"
элементах "ИЛИ-НЕ". Все это густо переплетено непростыми обратными связями,
которые зачастую являются распределенными по нескольким дополнительным ячейкам
функциями нескольких триггерных выходов. 

Несмотря на блестящую схему фазовой автоподстройки, авторы ВП1-128 не смогли
обеспечить надежное распознавание маркера и переход в режим чтения данных.
Дело в том, что ФАПЧ подстраивает генератор на удвоенную частоту битового
интервала. И результирующая битовая частота может иметь две фазы. Если
повезет и первыми битами будут нулевые данные (часто это так и есть,
поскольку перед маркером записывается преамбула из нескольких нулевых байт)
то фаза будет корректной и маркер будет распознан. А вот если сначала
попадется несколько единичных бит (МФМ-импульсы в середине интервала),
то фаза будет повернута на 180 градусов и маркер не будет распознан.
Это подтверждается моделированием и экспериментально.