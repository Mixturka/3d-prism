# Дисперсия света в призме

## Задача
Моделирование преломления света в призме и визуализация спектра.

## Функциональность
- Задание угла призмы
- Задание показателя преломления в зависимости от длины волны
- Расчет углов отклонения для разных длин волн
- Визуализация хода лучей в 3D
- Построение выходящего спектра на экране
- Отображение спектра в виде цветной полосы (дополнительное задание)

## Скриншоты
![alt text](images/screen1.png)
![alt text](images/screen2.png)
## Теоретическое обоснование

### Преломление в призме
Луч света падает на первую грань призмы под углом α. Угол при вершине призмы - φ

![alt text](images/image.png)

Рассмотрим $\Delta AOC$:

Угол $\phi$ смежный с углом $AOC$ => $\phi = \angle AOC - \angle OCA = (\alpha - \beta) + (\delta - \gamma)$

$\phi = (\alpha + \delta) - (\beta + \gamma)$

Четырехугольник ABCD:

$\angle BAD = \angle BCD = 90^o$

$\angle ABC = 360^o - 90^o - 90^o-\phi = 180^o - \phi$

$\Delta ABC:$

$\beta + \gamma + 180^o - \phi = 180^o$

$\phi = \beta + \gamma$

$\theta = (\alpha + \delta) - (\beta + \gamma) = (\alpha + \delta) - \phi$

Закон преломления для выходящего из призмы луча:

$\frac{sin \gamma}{sin \delta} = \frac{1}{n}$ => $sin \delta = n sin \gamma$

$\delta = arcsin(n sin \gamma)$

$\phi = \beta + \gamma$ => $\gamma = \phi - \beta$

$\delta = arcsin(n sin (\phi - \beta))$

Закон преломления для падающего на призму луча:

$\frac{sin \alpha}{sin \beta} = n$ => $sin \beta = \frac{sin \alpha}{n}$

$\beta = arcsin(\frac{sin \alpha}{n})$

$\delta = arcsin(n sin (\phi - arcsin(\frac{sin \alpha}{n})))$

$\theta = \alpha + \delta - \phi$

$\theta = \alpha - \phi + arcsin(n sin (\phi - arcsin(\frac{sin \alpha}{n})))$

### Геометрические параметры призмы
Вершины призмы в системе координат:

![Vertices definition](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Cbg_white%20%5Cbegin%7Balign*%7D%20P_1%20%26%3D%20%28-w%2F2%2C%200%29%20%5C%5C%20P_2%20%26%3D%20%28w%2F2%2C%200%29%20%5C%5C%20P_3%20%26%3D%20%280%2C%20h%29%20%5Cend%7Balign*%7D)

где ![Height formula](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Cbg_white%20h%20%3D%20%5Cfrac%7Bw%7D%7B2%20%5Ctan%28%5Cphi%2F2%29%7D), 
![Width definition](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Cbg_white%20w) — ширина основания.

### Проекция на экран
Координаты точки пересечения с экраном:

![Screen projection formulas](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Cbg_white%20%5Cbegin%7Balign*%7D%20x_%7B%5Ctext%7Bscreen%7D%7D%20%26%3D%20x_%7B%5Ctext%7Bexit%7D%7D%20%5C%5C%20y_%7B%5Ctext%7Bscreen%7D%7D%20%26%3D%20y_%7B%5Ctext%7Bexit%7D%7D%20-%20%28d%20%2B%20z_%7B%5Ctext%7Bexit%7D%7D%29%20%5Ccdot%20%5Ctan%5Ctheta%20%5Cend%7Balign*%7D)

где $d$ — расстояние до экрана.

Программа использует набор из 7 характерных длин волн видимого диапазона, соответствующих основным цветам спектра:

```js
const spectrumColors = [
    { color: 0xff0000, lambda: 700 }, // Красный (700 нм)
    { color: 0xffa500, lambda: 600 }, // Оранжевый (600 нм)
    { color: 0xffff00, lambda: 580 }, // Жёлтый (580 нм)
    { color: 0x00ff00, lambda: 530 }, // Зелёный (530 нм)
    { color: 0x00ffff, lambda: 480 }, // Голубой (480 нм)
    { color: 0x0000ff, lambda: 450 }, // Синий (450 нм)
    { color: 0x8a2be2, lambda: 400 }  // Фиолетовый (400 нм)
];
```

### Расчёт показателя преломления
Для каждой длины волны рассчитывается свой показатель преломления по формуле:

`const n_lambda = n + 3000 / (lambda * lambda);`

Где:
n - базовый показатель преломления (заданный пользователем),
lambda - длина волны в нанометрах,
3000 - эмпирическая дисперсионная постоянная
