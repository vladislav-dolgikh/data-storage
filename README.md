# data-storage

## Конвейер для обработки сырых данных ChIP-seq

![image](https://user-images.githubusercontent.com/83860672/212612264-763ab5ae-50ee-4078-b048-3189f3b18446.png)

Стандартный конвейер для обработки сырых данных ***ChIP-seq*** включает в себя этапы **(1)** контроля качества сырых данных, **(2)** предобработки сырых данных, **(3)** выравнивания сырых данных на референсный геном и **(4)** определения пиков. Каждый этап конвейера обозначается определенной латинской буквой-переменной, которая, в зависимости от режимов запуска программ, может принимать соответствующее значение. 

### Контроль качества сырых данных

Этап контроля качества сырых данных позволяет оценить качество секвенирования образца и позволяет исследователю принять решение о целесообразности предобработки данных. Контроль качества осуществляется при помощи инструмента **FastQC**. Данный этап в конвейере обозначается как `qc` (***q***uality ***c***ontrol), чьё значение всегда равно единице, так как контроль качества является обязательным этапом, проводящимся единообразно для любого эксперимента.

### Предобработка сырых данных

Предобработка сырых данных позволяет **(1)** удалять из прочтений нуклеотиды низкого качества, **(2)** удалять адаптерные последовательности, **(3)** удалять прочтения, не соответствующие заданным параметрам. Эти операции позволяют улучшить качество образцов. Данная процедура может осуществляться при помощи пакетов **Trimmomatic** и **fastp** Предобработка данных не является обязательным этапом, и решение о её проведении исследователь принимает в результате контроля качества. Данный этап обозначается буквой `p` (***p***reprocessing). `p` может принимать значения `0` (предобработка не производилась) и `1` (предобработка производилась).

### Выравнивание сырых данных на референсный геном

Этап выравнивания сырых данных выполняется при помощи программных пакетов **Bowtie** (при длине прочтений менее 50 п.н.) или **Bowtie2** (при длине прочтений 50 п.н. и более). Обозначается буквой `a` (***a***lignment). `a` может принимать значения `0` (использовался пакет **Bowtie**) и `1` (использовался пакет **Bowtie2**).

### Определение пиков ChIP-seq

Этап определения пиков ***ChIP-seq*** необходим для локализации регионов связывания ТФ в геноме. В рамках данного конвейера определение пиков осуществляется при помощи инструмента **MACS3**. Существует несколько подходов к определению пиков, схематично изображенном на схеме:


Этап определения пиков обозначается буквой `c` (peak ***c***alling). `c` может принимать значения `1` (использовался режим ***simple callpeak***, включающий получение пиков при помощи запуска `MACS3 callpeak`), `2` (использовался режим ***callpeak with IDR***, включающий связку `MACS3 callpeak` + `IDR`) и `3` (использовался режим ***callpeak with bdgdiff***, включающий связку `MACS3 callpeak` + `MACS3 bdgdiff`)

### Определение пиков ChIP-seq

Этап определения пиков ***ChIP-seq*** необходим для локализации регионов связывания ТФ в геноме. В рамках данного конвейера определение пиков осуществляется при помощи инструмента **MACS3**. Существует несколько подходов к определению пиков, схематично изображенном на схеме:


Этап определения пиков обозначается буквой `c` (peak ***c***alling). `c` может принимать значения `1` (использовался режим ***simple callpeak***, включающий получение пиков при помощи запуска `MACS3 callpeak`), `2` (использовался режим ***callpeak with IDR***, включающий связку `MACS3 callpeak` + `IDR`) и `3` (использовался режим ***callpeak with bdgdiff***, включающий связку `MACS3 callpeak` + `MACS3 bdgdiff`)

### Формат хранения данных

Пики ***ChIP-seq*** должны храниться в формате `BED`. Допустим формат `BED3`, а также опционально допустимы его расширения.

### Стандарты аннотации данных ChIP-seq в хранилище MotiVerse

Обработанные данные ***ChIP-seq*** хранятся в хранилище данных. Сырые данные не хранятся, но присутствуют скрипты, использовавшиеся при их обработке. Метаданные в хранилище представлены для сырых и обработанных в виде соответствующих таблиц.

|Название столбца|Содержание столбца|
|----------------|:----------------:|
|Transcription factor name|Название изучаемого ТФ|
|Library|Тип библиотеки. Принимает значения `single` или `paired`|
|Platform|Платформа, на которой проводилось секвенирование|
|Ecotype|Экотип образца. Стандартно `Col0`, если мутант - указывается название мутации|
|Treatment|Тип обработки образца|
|Duration|Время обработки образца|
|Tissue|Ткань растения|
|Growth conditions|Условия прорастания|
|Treatment raw samples|Идентификационные номера сырых образцов, использованных в качестве `treatment` в данном эксперименте. Имеет вид `SRXXXXXXXX1, SRXXXXXXXX2, SRXXXXXXXX3 ... SRXXXXXXXXn`|
|Control raw samples|Идентификационные номера сырых образцов, использованных в качестве `control` в данном эксперименте. Имеет вид `SRXXXXXXXX1, SRXXXXXXXX2, SRXXXXXXXX3 ... SRXXXXXXXXn`|
|Input control raw samples|Идентификационные номера сырых образцов, использованных в качестве `input control` в данном эксперименте. Имеет вид `SRXXXXXXXX1, SRXXXXXXXX2, SRXXXXXXXX3 ... SRXXXXXXXXn`. При отсутствии внешнего контроля принимает значение `0`|
|Data processing conditions|Описание условий обработки данных в виде кортежа `qc, p, a, c`. Значения, которые может принимать каждая переменная, описаны в разделе **Конвейер для обработки сырых данных ChIP-seq**. Например, `1,1,2,3`|
|Peak number|Количество пиков в эксперименте|
|Average peak length|Средняя длина пика|
|Reference|`PMID` статьи, в которой был описан эксперимент|
|Data filename|Название файла, депонированного в хранилище. Имеет вид `Название изучаемого ТФ` + `Время обработки образца` + `Тип режима определения пиков` + `Первый автор статьи, в которой опубликован эксперимент`. Например, `ARR1_4h_bdgdiff_Xie.bed`|
|Script filename|Название скрипта, которым производилась обработка, депонированного в хранилище. Имеет вид `Название изучаемого ТФ` + `Время обработки образца` + `Тип режима определения пиков`. Если одним скриптом проводилась обработка нескольких образцов, это также указывается в названии файлов.|

## Конвейер для обработки сырых данных RNA-seq

![image](https://user-images.githubusercontent.com/83860672/212818589-0d5ff340-751f-4a72-a245-77d0a5da651f.png)

Стандартный конвейер для обработки сырых данных ***RNA-seq*** включает в себя этапы **(1)** контроля качества сырых данных, **(2)** предобработки сырых данных, **(3)** выравнивания сырых данных на референсный геном, **(4)** квантификации прочтений и **(5)** поиска дифференциально экспрессирующихся генов (ДЭГ). Каждый этап конвейера обозначается определенной латинской буквой-переменной, которая, в зависимости от режимов запуска программ, может принимать соответствующее значение.

### Контроль качества сырых данных

Этап контроля качества сырых данных позволяет оценить качество секвенирования образца и позволяет исследователю принять решение о целесообразности предобработки данных. Контроль качества осуществляется при помощи инструмента **FastQC**. Данный этап в конвейере обозначается как `qc` (***q***uality ***c***ontrol), чьё значение всегда равно единице, так как контроль качества является обязательным этапом, проводящимся единообразно для любого эксперимента.

### Предобработка сырых данных

Предобработка сырых данных позволяет **(1)** удалять из прочтений нуклеотиды низкого качества, **(2)** удалять адаптерные последовательности, **(3)** удалять прочтения, не соответствующие пороговому значению длины. Эти операции позволяют улучшить качество образцов. Данная процедура осуществляется при помощи пакета **Trimmomatic** Предобработка данных не является обязательным этапом, и решение о её проведении исследователь принимает в результате контроля качества. Данный этап обозначается буквой `p` (***p***reprocessing). `p` может принимать значения `0` (предобработка не производилась) и `1` (предобработка производилась).

### Выравнивание сырых данных на референсный геном

Этап выравнивания сырых данных выполняется при помощи программных пакетов **hisat2** или **STAR**. Обозначается буквой `a` (***a***lignment). `a` может принимать значения `1` (использовался пакет **hisat2**) и `2` (использовался пакет **STAR**).

### Квантификация выровненных прочтений

Этап квантификации вырованенных прочтений выполняется либо при помощи программы **htseq-count**, либо внутри R-пакетов **DESeq2** или **EdgeR**. Обозначается буквой `q` (***q***uantification). `q` может принимать значения `1` (использовался R-пакет **DESeq2**), `2` (использовался инструмент **htseq-count**) и `3` (использовался 
R-пакет **EdgeR**).

### Поиск дифференциально экспрессирующихся генов

Поиск ДЭГов выполняется внутри R-пакетов **DESeq2** или **EdgeR**. Обозначается буквой `d` (***d***ifferential expression analysis). `d` может принимать значения `1` (использовался R-пакет **DESeq2**) и `2` (использовался R-пакет **EdgeR**).

### Стандарты аннотации данных RNA-seq в хранилище

Обработанные данные ***RNA-seq*** хранятся в хранилище данных. Сырые данные не хранятся, но присутствуют скрипты, использовавшиеся при их обработке. Метаданные в хранилище представлены для сырых и обработанных в виде соответствующих таблиц.

|Название столбца|Содержание столбца|
|----------------|:----------------:|
|Phytohormone name|Название гормона, индуцирующего транскрипционный ответ|
|Library|Тип библиотеки. Принимает значения `single` или `paired`|
|Platform|Платформа, на которой проводилось секвенирование|
|Ecotype|Экотип образца. Стандартно `Col0`, если мутант - указывается название мутации|
|Duration|Время обработки образца|
|Tissue|Ткань растения|
|Growth conditions|Условия прорастания|
|Treatment raw samples|Идентификационные номера сырых образцов, использованных в качестве `treatment` в данном эксперименте. Имеет вид `SRXXXXXXXX1, SRXXXXXXXX2, SRXXXXXXXX3 ... SRXXXXXXXXn`|
|Control raw samples|Идентификационные номера сырых образцов, использованных в качестве `control` в данном эксперименте. Имеет вид `SRXXXXXXXX1, SRXXXXXXXX2, SRXXXXXXXX3 ... SRXXXXXXXXn`|
|Data processing conditions|Описание условий обработки данных в виде кортежа `qc, p, a, q, d`. Значения, которые может принимать каждая переменная, описаны в разделе **Конвейер для обработки сырых данных RNA-seq**. Например, `1,1,1,3,2`|
|Up-regulated DEG number|Количество ДЭГ, повышающих уровень экспрессии в ответ на обработку|
|Down-regulated DEG number|Количество ДЭГ, понижающих уровень экспрессии в ответ на обработку|
|Reference|`PMID` статьи, в которой был описан эксперимент|
|Data filename|Название файла, депонированного в хранилище. Имеет вид `Название фитогормона` + `Время обработки образца` + `Первый автор статьи, в которой опубликован эксперимент`. Например, `Ethylene_4h_Chang.txt`|
|Script filename|Название скрипта, которым производилась обработка, депонированного в хранилище. Имеет вид `Название изучаемого ТФ` + `Время обработки образца`. Если одним скриптом проводилась обработка нескольких образцов, это также указывается в названии файлов.|

## Требования к скриптам

Скрипт должен содержать шапку (в виде комментария), в которой содержится информация о: 
- типе эксперимента;
- количестве образцов;
- ФИО проводившего обработку;
- дате проведения обработки.

В комментариях к строкам команд запуска программ указывается версия программы.

