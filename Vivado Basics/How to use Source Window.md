# Окно исходников проекта Vivado

Данный документ расскажет вам об одном из основных окон программы Vivado: `Sources`. Данное оно расположено в левом верхнем углу. Если вы его не видите, данное окно можно активировать через меню: `Window –> Sources`. Окно состоит из следующих вкладок:

1. Hierarchy;
2. Libraries;
3. Compile Order.

В определенных ситуациях в данном окне может появиться и вкладка `IP Cores`, но в рамках данного курса она нас не интересует.

Рассмотрим первые три вкладки.

## Иерархия модулей проекта

Рассмотрим _рис. 1_.

![../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_01.png](../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_01.png)

_Рисунок 1. Окно `Sources`, открытое на вкладке `Hierarchy`._

Данная вкладка состоит из четырех "папок":

1. Design Sources;
2. Constraints;
3. Simulation Sources;
4. Utility Sources.

В рамках текущего курса лабораторных работ мы будем взаимодействовать только с тремя из них.

Помните, что несмотря на использование слова "папка", речь идет не о директориях операционной системы. Папки проекта — это всего лишь удобная абстракция для управления иерархией проекта.

В папке `Design Sources` строится иерархия проектируемых модулей (реальных схем, которые в будущем могут быть воспроизведены в ПЛИС или заказной микросхеме).

Папка `Constraints` содержит файлы ограничений, помогающих реализовать проект на конкретной ПЛИС (см. ["Этапы реализации проекта в ПЛИС"](../Introduction/Implementation%20steps.md#implementation)).

`Simulation Sources` хранит в себе иерархию верификационного окружения, включая модули из папки `Design Sources` — т.е. все модули (как синтезируемые, так и не синтезируемые), которые будут использованы пр моделировании.

> Обратите внимание на то, вкладка `Hierarchy` не содержит файлов. Здесь отображается иерархия модулей проекта. Один модуль может быть использован несколько раз — и в этом случае он будет столько же раз отображён в иерархии, хотя файл, хранящий описание этого модуля останется один (см. _рис. 5_).

Допустим, мы создали модуль полного однобитного сумматора `fulladder`, а также создали модуль полного четырехбитного сумматора `fulladder4`, содержимое которого мы только планируем описать, подключив внутри него четыре однобитных сумматора.

Раскрыв папку `Design Sources` мы увидим два модуля – `fulladder` и `fulladder4`, которые пока что никак друг с другом не связаны. Двойное нажатие на название модуля приведёт к открытию файла, содержащего описание этого модуля.

![../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_02.png](../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_02.png)

_Рисунок 2. Содержимое папки `Design Sources`._

Модуль `fulladder4` является модулем верхнего уровня (top-level module). Это значит, что при попытке запуска моделирования или синтеза, Vivado будет работать именно с этим модулем. Чтобы сменить модуль верхнего уровня, необходимо нажать правой кнопкой мыши на интересующий модуль и выбрать `Set a top`.

![../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_03.png](../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_03.png)

_Рисунок 3. Пример смены модуля верхнего уровня._

Опишем логику работы четырехбитного сумматора таким образом, чтобы тот содержал четыре однобитных сумматора. После сохранения окно изменится так:

![../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_04.png](../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_04.png)

_Рисунок 4. Обновленное содержимое папки `Design Sources`._

После раскрытия ветки `fulladder4` будет отображено 4 подключенных модуля `fulladder`.

![../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_05.png](../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_05.png)

_Рисунок 5. Иерархия проекта с четырьмя копиями модуля `fulladder`._

В `Simulation Sources` мы видим один файл тестбенча, к которому что-то подключено, и модуль `fulladder4` с подключенными к нему другими модулями:

![../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_06.png](../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_06.png)

_Рисунок 6. Иерархия модулей `Simulation Sources`.

Модули из `Design Sources` автоматически попадают в `Simulation Sources`, так как эти модули используются при моделировании.

Помните, что здесь отображается иерархия модулей. В реальности модуль `fulladder` описан всего один раз.

Каждый раз, когда вы меняете что-то в модулях разрабатываемого устройства, это отражается как во вкладке `Design Sources`, так и в `Simulation Sources`. Раскроем вкладку с модулем `tb`:

![../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_07.png](../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_07.png)

_Рисунок 7. Пример иерархии с отсутствующим модулем._

Такая картина говорит нам о попытке подключить модуль, которого нет в проекте. Часто это связано с неправильным указанием подключаемого модуля. В данном случае мы хотим подключить модуль `half_adder` и Vivado не может его найти.

```SystemVerilog
module tb();

//...

half_adder DUT(
  .A    (a),
  .B    (b),
  .P    (p),
  .S    (s)
);

// ...

endmodule
```

Переименуем название подключаемого модуля на `fulladder4` и сохраним.

```SystemVerilog
module tb();

//...

fulladder4 DUT(
  .A    (a),
  .B    (b),
  .P    (p),
  .S    (s)
);

// ...

endmodule
```

После обновления в окне `Sources` модуль `fulladder4` "спрячется" под `tb`. Если раскрыть вкладку, будет видно, что `fulladder4` подключен к `tb`, а четыре модуля `fulladder` – к `fulladder4`. Также отметим, что `tb` является модулем верхнего уровня, значит, если мы захотим запустить симуляцию, то Vivado выполнит симуляцию именно для модуля `tb`. Изменить модуль верхнего уровня можно так же, как было описано ранее.

![../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_08.png](../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_08.png)

_Рисунок 8. Пример исправленной иерархии верификационного окружения._

После каждого сохранения файла проекта, иерархия проекта будет какое-то время обновляться. Это можно заметить по надписи `Updating` вверху окна (см. _рис 9_). Во время обновления иерархии не стоит выполнять операции синтеза или моделирования — это приведет к ошибке.

![../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_09.png](../.pic/Vivado%20Basics/How%20to%20use%20Source%20Window/fig_09.png)

_Рисунок 9. Окно `Sources` во время обновления иерархии проекта._

Одной из частой ошибок студентов бывает прикрепление файла не к той папке. Например, создание модуля проекта в папке `Simulation Sources` (из-за чего тот не появится в папке `Design Sources`), или создание модуля верификационного окружения в папке `Design Sources` (он же наоборот — окажется и в папке `Simulation Sources`, но при этом в папке Design Sources окажется несинтезируемый модуль, который может оказаться еще и модулем верхнего уровня, что приведет к ошибке).

В случае, если произошла такая ошибка, она может быть легко исправлена нажатием правой кнопкой мыши по неправильно расположеному модулю и выбору кнопки: "Move to Design[или Simulation] sources".

## Библиотеки проекта

В данной вкладке находятся файлы проекта, сгруппированные по библиотекам. Обычно данная вкладка практически не используется.

## Порядок компиляции сущностей проекта

Обычно Vivado сам определяет порядок компиляции по иерархии проекта. Однако, в некоторых ситуациях он может определить что-то неправильно. На данной вкладке вы можете исправить порядок компиляции (скорее всего, вам может потребоваться эта вкладка, для указания порядка компиляции пакетов SystemVerilog).

## Дополнительные материалы

Более подробную информацию по окну `Sources` вы можете найти в руководстве пользователя Vivado: ["Vivado Design Suite User Guide: Using the Vivado IDE (UG893)"](https://docs.xilinx.com/r/en-US/ug893-vivado-ide) (раздел ["Using the Sources Window"](https://docs.xilinx.com/r/en-US/ug893-vivado-ide/Using-the-Sources-Window)).