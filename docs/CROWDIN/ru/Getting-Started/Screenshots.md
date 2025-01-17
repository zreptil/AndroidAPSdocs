# Экраны AndroidAPS

## Главный экран

![Главный экран V2.7](../images/Home2020_Homescreen.png)

Это первый экран, который вы увидите, когда откроете AndroidAPS, и он содержит большую часть текущей информации.

### Раздел А - Вкладки

* Переход между различными модулями AndroidAPS.
* Также можно переходить между экранами свайпом влево или вправо.
* Отображаемые вкладки можно выбрать в [Конфигураторе](../Configuration/Config-Builder#tab-or-hamburger-menu).

### Раздел B - Профиль & Цель

#### Текущий профиль

![Оставшееся время замененного профиля](../images/Home2020_ProfileSwitch.png)

* Текущий профиль отображается на левой панели.
* Короткое нажатие открывает подробности о текущем профиле
* Долгое нажатие позволяет [переключаться между разными профилями](../Usage/Profiles#profile-switch).
* Если профиль был изменен на время - в скобках отображается оставшееся время его активности в минутах.

#### Целевое значение ГК

![Оставшаяся продолжительность временной цели](../images/Home2020_TT.png)

* Текущее целевое значение глюкозы крови (ГК) отображается на правой панели.
* Короткое нажатие позволяет установить [временную цель](../Usage/temptarget.md).
* Если установлена временная цель - панель будет желтой, а в скобках отображается оставшееся время ее активности в минутах.

#### Визуализация динамического изменения цели

![Визуализация динамического изменения цели](../images/Home2020_DynamicTargetAdjustment.png)

* AAPS может динамически изменять установленную цель основываясь на чувствительности если используется алгоритм СМБ (SMB).
* Активируйте один или оба [варианта](../Configuration/Preferences#openaps-smb-settings): 
   * "чувствительность повышает цель" и/или 
   * "сопротивляемость понижает цель" 
* Если AAPS обнаружит повышенную сопротивляемость или чувствительность - цель будет изменена. 
* При таком изменении панель будет зеленой.

### Раздел C - ГК & статус петли

#### Текущий уровень глюкозы крови

* Последнее значение ГК, переданное вашим НМГ, отображается в левой части экрана.
* Цвет показаний ГК соответствует настроенному [даипазону](../Configuration/Preferences#range-for-visualization): 
   * зеленый = в заданном диапазоне
   * красный = ниже заданного диапазона
   * желтый = выше заданного диапазона
* Сероватый блок в центре экрана отображает изменение текущего уровня ГК относительно предыдущего чтения, и изменение за последние 15 и 40 минут.

#### Статус цикла

![Статус цикла](../images/Home2020_LoopStatus.png)

* Новые иконки отображают статус цикла:
   
   * зеленый круг = активна замкнутая петля
   * зеленый круг с пунктирной линией =[приостановка помпы на низкой ГК (LGS)](../Usage/Objectives#objective-6-starting-to-close-the-loop-with-low-glucose-suspend)
   * красный круг = цикл деактивирован (постоянно отключен)
   * желтый круг = цикл приостановлен (временно приостановлен, но базальный инсулин будет подаваться) - оставшееся время паузы отображается под иконкой
   * серый круг = помпа отключена (временно отключена любая подача инсулина) - оставшееся время остановки отображается под иконкой
   * оранжевый круг = запущен суперболюс - оставшееся время отображаешься под иконкой
   * синий круг с пунктирной линией = активна открытая петля

* Короткое или длинное нажатие на иконку откроет диалоговое окно для переключения между режимами петли (Закрытая, Предотвращение низкой ГК, Открытая или Отключена), для отключения / возобновления цикла или отключения / подключения помпы обратно.
   
   * Если диалоговое окно было вызвано которким нажатием - после смены режима появится запрос на подтверждение. Если долгим нажатием - смена режима применится сразу.
   
   ![Меню состояния цикла](../images/Home2020_Loop_Dialog.png)

#### предупреждающий знак ГК

Начиная с Android 3.0, вы можете увидеть предупреждающий знак под числом ГК на главном экране.

*Примечание*: Расчеты AAPS учитывают до 30 часов. Поэтому даже после устранения проблемы нерегулярной передачи данных, может потребоваться до 30 часов, чтобы исчез желтый треугольник.

Чтобы немедленно удалить его, вам нужно вручную удалить несколько записей из вкладки Dexcom/xDrip+.

Однако, когда есть много дубликатов, может быть проще

* [создать резервную копию настроек](../Usage/ExportImportSettings.rst),
* сбросить базу данных в меню обслуживания и
* заново [импортировать настройки](../Usage/ExportImportSettings.rst)

##### Красный предупреждающий знак: Дублирующиеся данные ГК

Красный предупреждающий знак служит сигналом для немедленных действий: поступают дублирующиеся данные ГК, не позволяющие правильно работать алгоритму цикла. Поэтому замкнутый цикл будет отключен до устранения проблемы.

![Red BG warning](../images/bg_warn_red.png)

Следует выяснить, почему поступают дубликаты ГК:

* Активирован ли мост Dexcom на вашем сайте NS? Отключите мост, перейдя в Heroku (или к другому провайдеру хостинга), отредактируйте переменную «enable» и удалите "bridge". (Для Heroku [подробности можно найти здесь](https://nightscout.github.io/troubleshoot/troublehoot/#heroku-settings).)
* Имеется ли несколько источников передачи данных ГК в Nightscout (NS)? Если вы используете самостоятельно собранное приложение BYODA, включите загрузку в AAPS, но не включайте его в xDrip+.
* Есть ли подписчики, followers, которые могут получать вашу ГК и также снова передавать ее на ваш сайт NS?
* Последний вариант: В AAPS перейдите в настройки NS Client выберите настройки синхронизации и отключите опцию "Принимать данные CGM из NS".

##### Желтый предупреждающий знак

* Желтый сигнал предупреждения указывает, что значения вашей ГК получены через нерегулярные промежутки времени или имеются пропуски значений.
   
   ![Yellow BG warning](../images/bg_warn_yellow.png)

* Обычно предпринимать никаких действий не требуется. Замкнутый цикл продолжит работать!

* Так как замена сенсора изменяет поток данных, предупреждающий знак после смены - явление нормальное и не должно вызывать беспокойства.
* Специально для пользователей libre:
   
   * Каждый сенсор libre пропускает данные раз или два в несколько часов, а это означает, что идеального потока значений ГК не получится.
   * Непрерывный поток также нарушается скачками данных.
   * Поэтому желтый предупреждающий знак будет «всегда включен» для пользователей libre.

### Раздел D - IOB, COD, BR и AS

![Раздел D](../images/Home2020_TBR.png)

* Иконка шприца: инсулин "на борту" (IOB) - количество активного инсулина в теле
   
   * Значение активного инсулина Iob будет нулевым при подаче только стандартного базального и когда нет остатков от предыдущих болюсов. 
   * IOB может быть отрицательным если был период с пониженным относительно текущего профиля базалом.
   * Нажмите на иконку (только короткое нажатие), чтобы увидеть как IOB распределяется между базой и болюсом.

* Иконка колоска: [углеводы "на борту"](../Usage/COB-calculation.rst) - количество еще не усвоенных углеводов из тех, чтоб были введены ранее. Иконка мерцает, если необходимо съесть дополнительные углеводы.

* Иконка с фиолетовой линией: базальная скорость - иконка изменяется в соответствии с текущими настройками базала (прямая линия при базе 100%) 
   * Нажмите на иконку (только короткое нажатие), чтобы увидеть подробности базала (значение текущего базала, время начала, остаток/общая продолжительность в минутах)
* Иконка со стрелочками вверх и вниз: отображает текущий статус Автосенса (AS, Autosens) (включен или отключен) и его значение

#### Требуются углеводы

![Требуются углеводы](../images/Home2020_CarbsRequired.png)

* Требование углеводов появляется, когда расчеты определяют их недостаток.
* Это происходит, когда алгоритм Oref думает: "Я не спасу тебя отключением всего инсулина и тебе необходимы углеводы, чтобы не загиповать". 
* Уведомления об углеводах значительно сложнее, чем уведомления калькулятора болюса. Вы можете увидеть требование углеводов даже когда калькулятор болюса не показывает их нехватку.
* При желании уведомления об углеводах могут быть переданы в Nightscout. В этом случае сработают стандартные настройки оповещения NS. 

### Раздел E- Индикаторы состояния

![Раздел E](../images/Home2020_StatusLights.png)

* Индикаторы состояния сообщают о 
   * Времени работы катетера помпы
   * Времени работы инсулина в резервуаре помпы
   * Уровне заполнения резервуара (единицы)
   * Времени, отработанном сенсором
   * Времени, отработанном батареей и о ее уровне (%)
* Если превышено пороговое значение, данные показываются желтым цветом.
* Если превышено критическое пороговое значение, значения будут показаны красным цветом.
* Настройки могут быть сделаны в [параметрах](../Configuration/Preferences#status-lights).

### Раздел F - Основной график

![Раздел F](../images/Home2020_MainGraph.png)

* График показывает уровень глюкозы в крови (ГК) считываемый с мониторинга глюкозы (CGM). 
* Здесь показаны заметки, введенные на вкладке действия, такие как калибровка с глюкометра и записи углеводов, а также переключения профиля. 
* Длительное нажатие на графике изменит шкалу времени. Можно выбрать 6, 8, 12, 18 или 24 часа.
* Зеленая область отражает ваш целевой диапазон. Его можно настроить в [настройках](../Configuration/Preferences#range-for-visualization).
* Голубые треугольники показывают [микроболюсы SMB](../Usage/Open-APS-features#super-micro-bolus-smb) - если они активированы в [настройках](../Configuration/Preferences#openaps-smb-settings).
* Дополнительная информация:
   
   * Прогнозирование
   * Базал
   * Активность-кривая действия инсулина

#### Активация необязательной информации

* Щелкните по треугольнику в правой части основного графика, чтобы выбрать, какая информация будет показана на главной диаграмме.
* Для главного графика доступны три варианта выше строки "\---\---- График 1 \---\----".
   
   ![Настройка главного графика](../images/Home2020_MainGraphSetting.png)

#### Линии прогнозирования

* ** Оранжевая ** линия: [активные углеводы COB ](../Usage/COB-calculation.rst) (цвет используется обычно для представления активных углеводов COB и углеводов)
   
   Линия предсказания показывает, где будет ГК (а не сами активные углеводы COB) на основе текущих настроек помпы с учетом того, что отклонения вследствие усвоения углеводов останутся постоянными. Эта линия появляется только при известном наличии активных углеводов COB.

* **Темно-синяя ** линия: активный инсулин IOB (цвет обычно используется для отображения активного инсулина IOB и инсулина)
   
   Линия предсказания показывает, что будет происходить только под воздействием инсулина. Например, если вы ввели инсулин, а потом не ели никаких углеводы.

* ** Голубая ** линия: нулевой временный базал (предсказанная ГК, если будет установлена временная базальная скорость в 0%)
   
   В строке прогноза показано, как изменится линия траектории активного инсулина IOB, если помпа прекратит подачу инсулина (0% TBR).

* ** Темно-желтая ** линия: [ непредвиденный прием пищи UAM ](../Configuration/Sensitivity-detection-and-COB#sensitivity-oref1)
   
   Незапланированный прием пищи - обнаружение значительного повышения уровня глюкозы, как следствие приема пищи, выброса адреналина или других факторов. Линия предсказания аналогична оранжевой линии активных углеводов COB, но предполагает, что отклонения будут понижаться с постоянной скоростью (за счет увеличения текущей скорости сокращения).

Обычно реальная кривая глюкозы заканчивается в середине этих линий, или близка к той, которая ближе всего описывает вашу ситуацию.

#### Базал

* **Сплошная синяя** линия показывает базальную скорость помпы и отражает фактическую подачу инсулина с течением времени.
* **пунктирная синяя** линия - это средняя скорость базы, если не было временных настроек базальной скорости (TBR).
* При стандартной базальной скорости область под кривой показывается в темно-синем цвете.
* When the basal rate is temporarily adjusted (increased or decreased) the area under the curve is shown in light blue.

#### Activity

* The **thin yellow** line shows the activity of Insulin. 
* It is based on the expected drop in BG of the insulin in your system if no other factors (like carbs) were present.

### Раздел G - дополнительные графики

* You can activate up to four additional graphs below the main graph.
* To open settings for additional graphs click the triangle on the right side of the [main graph](../Getting-Started/Screenshots#section-f-main-graph) and scroll down.

![Additional graph settings](../images/Home2020_AdditionalGraphSetting.png)

* To add an additional graph check the box on the left side of its name (i.e. \---\---- Graph 1 \---\----).

#### Absolute insulin

* Active insulin including boluses **and basal**.

#### Insulin on board

* Shows the insulin you have on board (= active insulin in your body). It includes insulin from bolus and temporary basal (**but excludes basal rates set in your profile**).
* If there were no [SMBs](../Usage/Open-APS-features#super-micro-bolus-smb), no boluses and no TBR during DIA time this would be zero.
* IOB can be negative if you have no remaining bolus and zero/low temp for a longer time.
* Decaying depends on your [DIA and insulin profile settings](../Configuration/Config-Builder#local-profile). 

#### Carbs On Board

* Shows the carbs you have on board (= active, not yet decayed carbs in your body). 
* Decaying depends on the deviations the algorithm detects. 
* If it detects a higher carb absorption than expected, insulin would be given and this will increase IOB (more or less, depending on your safety settings). 

#### Deviations

* **GREY** bars show a deviation due to carbs. 
* **GREEN** bars show that BG is higher than the algorithm expected it to be. Green bars are used to increase resistance in [Autosens](../Usage/Open-APS-features#autosens).
* **RED** bars show that BG is lower than the algorithm expected. Red bars are used to increase sensitivity in [Autosens](../Usage/Open-APS-features#autosens).
* **YELLOW** bars show a deviation due to UAM.
* **BLACK** bars show small deviations not taken into account for sensitivity

#### Sensitivity

* Shows the sensitivity that [Autosens](../Usage/Open-APS-features#autosens) has detected. 
* Sensitivity is a calculation of sensitivity to insulin as a result of exercise, hormones etc.

#### Activity

* Shows the activity of insulin, calculated by your insulin profile (it's not derivative of IOB). 
* The value is higher for insulin closer to peak time.
* It would mean to be negative when IOB is decreasing. 

#### Deviation slope

* Internal value used in algorithm.

### Раздел H-Кнопки

![Homescreen buttons](../images/Home2020_Buttons.png)

* Кнопки инсулина, углеводов и калькулятора почти 'всегда активны'.
   
   * При потере подключения к помпе кнопка инсулина не видна.

* Other Buttons have to be setup in [preferences](../Configuration/Preferences#buttons).

#### Инсулин

![Insulin button](../images/Home2020_ButtonInsulin.png)

* To give a certain amount of insulin without using [bolus calculator](#bolus-wizard).
* By checking the box you can automatically start your [eating soon temp target](../Configuration/Preferences#default-temp-targets).
* If you do not want to bolus through pump but record insulin amount (i.e. insulin given by syringe) check the corresponding box.

#### Углеводы

![Carbs button](../images/Home2020_ButtonCarbs.png)

* To record carbs without bolusing.
* Certain [pre-set temporary targets](../Configuration/Preferences#default-temp-targets) can be set directly by checking the box.
* Time offset: When will you / have you been eaten carbs (in minutes).
* Duration: To be used for ["extended carbs"](../Usage/Extended-Carbs.rst)
* You can use the buttons to quickly increase carb amount.
* Notes will be uploaded to Nightscout - depending on your settings for [NS client](../Configuration/Preferences#nsclient).

#### Калькулятор

* See Bolus Wizard [section below](#bolus-wizard)

#### Calibrations

* Sends a calibration to xDrip+ or opens Dexcom calibration dialogue.
* Must be activated in [preferences](../Configuration/Preferences#buttons).

#### CGM/Непрерывный мониторинг ГК

* Opens xDrip+.
* Back button returns to AAPS.
* Must be activated in [preferences](../Configuration/Preferences#buttons).

#### Мастер быстрых настроек

* Easily enter amount of carbs and set calculation basics.
* Details are setup in [preferences](../Configuration/Preferences#quick-wizard).

## Мастер Болюса

![Bolus wizard](../images/Home2020_BolusWizard_v2.png)

When you want to make a meal bolus this is where you will normally make it from.

### Раздел I

* BG field is normally already populated with the latest reading from your CGM. If you don't have a working CGM then it will be blank. 
* In the CARBS field you add your estimate of the amount of carbs - or equivalent - that you want to bolus for. 
* The CORR field is if you want to modify the end dosage for some reason.
* The CARB TIME field is for pre-bolusing so you can tell the system that there will be a delay before the carbs are to be expected. You can put a negative number in this field if you are bolusing for past carbs.

#### Eating reminder

* For carbs in the future the alarm checkbox can be selected (and is by default when a time in the future is entered) so that you can be reminded at a time in the future of when to eat the carbs you have input into AndroidAPS
   
   ![BolusWizard with Eating Reminder](../images/Home2021_BolusWizard_EatingReminder.png)

### Раздел J

* SUPER BOLUS is where the basal insulin for the next two hours is added to the immediate bolus and a zero TBR is issued for the following two hours to take back the extra insulin. The option only shows when "Enable [superbolus](../Configuration/Preferences#superbolus) in wizard" is set in the [preferences overview](../Configuration/Preferences#overview).
* The idea is to deliver the insulin sooner and hopefully reduce spikes.
* For details visit [diabetesnet.com](https://www.diabetesnet.com/diabetes-technology/blue-skying/super-bolus/).

### Раздел K

* Shows the calculated bolus. 
* If the amount of insulin on board already exceeds the calculated bolus then it will just display the amount of carbs still required.
* Notes will be uploaded to Nightscout - depending on your settings for [NS client](../Configuration/Preferences#nsclient).

### Раздел L

* Details of wizard's bolus calculation.
* You can deselect any that you do not want to include but you normally wouldn't want to.
* For safety reasons the **TT box must be ticked manually** if you want the bolus wizard to calculate based on an existing temporary target.

#### Combinations of COB and IOB and what they mean

* For safety reasons IOB boxed cannot be unticked when COB box is ticked as you might run the risk of too much insulin as AAPS is not accounting for what’s already given.
* If you tick COB and IOB unabsorbed carbs that are not already covered with insulin + all insulin that has been delivered as TBR or SMB will be taken into account.
* If you tick IOB without COB, AAPS takes account of already delivered insulin but won’t cover that off against any carbs still to be absorbed. This leads to a 'missing carbs' notice.
* If you bolus for **additional food** shortly after a meal bolus (i.e. additional desert) it can be helpful to **untick all boxes**. This way just the new carbs are being added as the main meal won't necessarily be absorbed so IOB won't match COB accurately shortly after a meal bolus.

#### Wrong COB detection

![Slow carb absorption](../images/Calculator_SlowCarbAbsorption.png)

* If you see the warning above after using bolus wizard, AndroidAPS has detected that the calculated COB value maybe wrong. 
* So, if you want to bolus again after a previous meal with COB you should be aware of overdosing! 
* For details see the hints on [COB calculation page](../Usage/COB-calculation#detection-of-wrong-cob-values).

## Вкладка "Действия"

![Actions tab](../images/Home2021_Action.png)

### Действия-раздел M

* Button [profile switch](../Usage/Profiles#profile-switch) as an alternative to pressing the [current profile](../Getting-Started/Screenshots#section-b-profile-target) on homescreen.
* Button [temporary target](../Usage/temptarget#temp-targets) as an alternative to pressing the [current target](../Getting-Started/Screenshots#section-b-profile-target) on homescreen.
* Button to start or cancel a temporary basal rate. Please note that the button changes from “TEMPBASAL” to “CANCEL x%” when a temporary basal rate is set.
* Even though [extended boluses](../Usage/Extended-Carbs#extended boluses) do not really work in a closed loop environment some people were asking for an option to use extended bolus anyway.
   
   * This option is only available for Dana RS and Insight pumps. 
   * Closed loop will automatically be stopped and switched to open loop mode for the time running extended bolus.
   * Make sure to read the [details](../Usage/Extended-Carbs#extended boluses) before using this option.

### Портал терапии-раздел N

* Displays information on
   
   * sensor age & level (battery percentage)
   * insulin age & level (units)
   * cannula age
   * pump battery age & level (percentage

* Less information will be shown if [low resolution skin](../Configuration/Preferences#skin) is used.

#### Sensor level (battery)

* Needs xDrip+ nightly build Dec. 10, 2020 or newer.
* Works for CGM with additional transmitter such as MiaoMiao 2. (Technically sensor has to send cat level information to xDrip+.)
* Thresholds can be set in [preferences](../Configuration/Preferences#status-lights).
* If sensor level is the same as phone battery level you xDrip+ version is probably too old and needs an update.
   
   ![Sensor levels equals phone battery level](../images/Home2021_ActionSensorBat.png)

### Портал терапии-раздел О

* BG check, prime/fill, sensor insert and pump battery change are the base for the data displayed in [section N](#careportal-section-n).
* Prime/Fill allows you to record pump site and insulin cartridge change.
* Section O reflects the Nightscout careportal. So exercise, announcement and question are special forms of notes.

### Инструменты - раздел P

#### History Browser

* Allows you to ride back in AAPS history.

#### TDD/общая суточная доза инсулина

* Total daily dose = bolus + basal per day
* Some doctors use - especially for new pumpers - a basal-bolus-ratio of 50:50. 
* Therefore ratio is calculated as TDD / 2 * TBB (Total base basal = sum of basal rate within 24 hours). 
* Others prefer range of 32% to 37% of TDD for TBB. 
* Like most of these rules-of-thumb it is of limited real validity. Note: Your diabetes may vary!

![Histroy browser + TDD](../images/Home2021_Action_HB_TDD.png)

## Профиль Инсулина

![Профиль Инсулина](../images/Screenshot_insulin_profile.png)

* This shows the activity profile of the insulin you have chosen in [config builder](../Configuration/Config-Builder#insulin). 
* The PURPLE line shows how much insulin remains after it has been injected as it decays with time and the BLUE line shows how active it is.
* The important thing to note is that the decay has a long tail. 
* If you have been used to manual pumping you have probably been used to assuming that insulin decays over about 3.5 hours. 
* However, when you are looping the long tail matters as the calculations are far more precise and these small amounts add up when they are subjected to the recursive calculations in the AndroidAPS algorithm.

For a more detailed discussion of the different types of insulin, their activity profiles and why all this matters you can read an article here on [Understanding the New IOB Curves Based on Exponential Activity Curves](https://openaps.readthedocs.io/en/latest/docs/While%20You%20Wait%20For%20Gear/understanding-insulin-on-board-calculations.html#understanding-the-new-iob-curves-based-on-exponential-activity-curves)

And you can read an excellent blog article about it here: [Why we are regularly wrong in the duration of insulin action (DIA) times we use, and why it matters…](https://www.diabettech.com/insulin/why-we-are-regularly-wrong-in-the-duration-of-insulin-action-dia-times-we-use-and-why-it-matters/)

And even more at: [Exponential Insulin Curves + Fiasp](https://seemycgm.com/2017/10/21/exponential-insulin-curves-fiasp/)

## Статус помпы

![Статус помпы](../images/Screenshot_PumpStatus.png)

* Different information on pump status. Displayed information depends on your pump model.
* See [pumps page](../Hardware/pumps.md) for details.

## Портал терапии

Careportal replicated the functions you will find on your Nightscout screen under the “+” symbol which allows you to add notes to your records.

### Просмотреть расчет углеводов

![Review carb calculation on treatment tab](../images/Screenshots_TreatCalc.png)

* If you have used the [Bolus Wizard](../Getting-Started/Screenshots#bolus-wizard) to calculate insulin dosage you can review this calculation later on treatments tab.
* Just press the green Calc link. (Depending on pump used insulin and carbs can also be shown in one single line in treatments.)

### Коррекция углеводов

![Treatment in 1 or 2 lines](../images/Treatment_1or2_lines.png)

Treatment tab can be used to correct faulty carb entries (i.e. you over- or underestimated carbs).

1. Проверьте и запомните фактические активные углеводы COB и активный инсулин IOB на главном экране.
2. В зависимости от помпы углеводы на вкладке терапии могут быть показаны одной линией с инсулином или в виде отдельной записи (например, для Dana RS).
3. Удалите запись с неверным количеством углеводов.
4. Убедитесь, что углеводы удалены успешно, повторно проверив активные углеводы COB на главном экране.
5. Сделайте то же для активного инсулина IOB, если на вкладке терапии только одна линия для углеводов и инсулина.
   
   -> Если углеводы не удаляются должным образом, а вы добавили дополнительные углеводы, как описано здесь (6.), активных углеводов COB окажется слишком много, и это может привести к передозировке инсулина.

6. Введите правильное количество углеводов при помощи кнопки углеводов на главном экране и убедитесь, что точное время события также введено.

7. Если на вкладке терапии только одна линия для углеводов и инсулина, следует также добавить и запись о количестве инсулина. Убедитесь в том, чтобы установить правильное время событие и проверить активный инсулин IOB на главном экране после подтверждения новой записи.

## Замкнутый цикл, помощник болюса AMA / микроболюсы SMB

* These tabs show details about the algorithm's calculations and why AAPS acts the way it does.
* Calculations are each time the system gets a fresh reading from the CGM.
* For more details see [APS section on config builder page](../Configuration/Config-Builder#aps).

## Профиль

![Профиль](../images/Screenshots_Profile.png)

* Profile contains information on your individual diabetes settings:
   
   * DIA (Duration of Insulin Action)
   * IC or I:C: Insulin to Carb ratio
   * ISF: Коэффициент чувствительности к инсулину
   * Скорость базала
   * Цель: Уровень глюкозы крови для AAPS

* Начиная с версии 3.0 возможен только [локальный профиль](../Configuration/Config-Builder#local-profile). Локальный профиль может быть отредактирован на вашем смартфоне и синхронизирован с сайтом Nightscout.

## Терапия

История следующих терапевтических действий:

* Bolus & Carbs -> возможность [удалить записи](../Getting-Started/Screenshots#carb-correction) для исправления истории
* [Пролонгированный болюс](../Usage/Extended-Carbs#extended-bolus-and-switch-to-open-loop-dana-and-insight-pump-only)
* Временная базальная скорость
* [Временная цель](../Usage/temptarget.md)
* [Profile switch/смена профиля](../Usage/Profiles.md)
* [Портал терапии](../Usage/CPbefore26#careportal-discontinued) - примечания, введенные через вкладку действий и примечания в диалогах

## Источник ГК - xDrip+, BYODA...

![Вкладка Источник ГК- здесь xDrip](../images/Screenshots_BGSource.png)

* В зависимости от заданного в параметрах источника ГК эта вкладка называется по-разному.
* Показывает хронологию показаний мониторинга и предлагает возможность удаления данных при сбое (например, при компрессии сенсора).

## клиент NS

![клиент NS](../images/Screenshots_NSClient.png)

* Показывает состояние соединения с сайтом Nightscout.
* Настройки могут быть сделаны в [параметрах](../Configuration/Preferences#nsclient). Вы можете открыть соответствующий раздел, щелкнув по значку шестеренки в верхней правой части экрана.
* Для устранения неполадок смотрите эту [страницу](../Usage/Troubleshooting-NSClient.md).