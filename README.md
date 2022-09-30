# lab2_super

Рыбкин Павел м21-502 


Приложение представляет себя клиент сервер на базе tcp соект соединения

## Сервер
В первую очередь запускается сервер и вводится количество мегабайт сгенерированных данных
Генрируется матрица с цифрами от 0 до 100 
Колчиество стро и столбцов в зависимости от мегайбайт высичтывается по формуле 
int res = (dataInMb * 1024) / sqrt(3*dataInMb);
По сути в среднем имеем 3 бита на одно число (пробел + само число в 90: случаев состоящее из 2х цифр)

Сгенерированные данные грузятся файл

Для старта работы сервер должен принять от клиента команду "start"

После чего данные берутся с файла по 80 симоволов и отправляются к клиенту


Сервер принимает от клиента команду то что он начал отпарвлять даннные "start" получившиеся цифры вектора

При команде exit будет сформировано время выполнения а так же принят размео буффера

Все записывается в res.txt

Данные егнерируются в matrix.txt

## Клиент
Клиент для запуска должен отправить команду "start"

Парсинг происходит по алгоритмму:

Если пробел - значит число закончилось => преобразоввываем val в число

Если '\n' то это переност строки => чистаем среднюю

Если не пробел не \n и не \t => формируем строку числа

Если e => уонец приема

## Приницип работы
<img width="1343" alt="image" src="https://user-images.githubusercontent.com/72603507/193226323-79f2e306-d2b0-4ef0-b842-8b655f61a259.png">

При использовании технологии OpenMP программа, как показано на рисунке 1, начинает свое выполнение как один процесс, называемый главной нитью. Главная нить выполняется последовательно до тех пор, пока не встретится первая параллельная область программы. Начало параллельной области определяется директивой процессору #pragma omp parallel, а её границы задаются фигурными скобками. При входе в параллельную область главная нить порождает некоторое число подчиненных ей нитей, которые вместе с ней образуют текущую группу нитей. Все операторы программы, находящиеся в параллельной конструкции, включая вызываемые изнутри нее функции, выполняются всеми нитями текущей группы параллельно до выхода из этой области. При выходе из параллельной области все порожденные на её входе нити сливаются с главной нитью, которая продолжает свое дальнейшее выполнение.


При распараллеливании получается алгоритм ленточного вычисления. При данном подходе матрица “нарезается” на ленты, а те в свою очередь параллельно вычисляются. 


### pragma omp parallel for num_threads(6) shared(res_buf, res_arr, arr) private (ii, jj) reduction(+: rowsum) schedule(static)

__num_threads__ число потоков

__shared__ Явным образом определяет список переменных, которые должны быть общими для всех нитей параллельной области.

__private__ Определяет список переменных, которые должны быть локальными для каждой нити.

__reduction__  Операция редукции производится над копиями переменных во всех нитях, и результат присваивается базовой переменной.

__shedule__ Чтобы максимально сокращать время ожидания, необходимо распределить общую работу, чтобы все потоки поступали в барьер примерно в то же время.




### График времени выполнения

| Метод.        | Время (мс)на 10мб  | Время (мс)на 50мб  | Время (мс)на 100мб | Время (мс)на 500мб | Время (мс)на 1000мб|
| ------------- |:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Линейный      |         7          |         33         |        74          |       539.         |       1287         |
| OMP 4 thread  |         6          |         32         |        88          |       633          |        758         |
| OMP 6 thread  |         6          |         34         |        66          |       349          |        654         |

<img width="624" alt="Снимок экрана 2022-09-30 в 11 06 47" src="https://user-images.githubusercontent.com/72603507/193223312-3eb418a5-b0a7-4c31-96a5-1535b719de05.png">


