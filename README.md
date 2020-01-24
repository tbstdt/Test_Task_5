Январь 2020
## Тестовое задание 5:

Сделать 2д топ-даун шутер:
- персонаж может перемещаться по сцене
- персонаж может стрелять хотя бы из двух видов оружия
- на сцене периодически появляются мишени
- за уничтожение мишеней начисляются очки
- количество заработанных очков отображается в HUD'е
- не использовать сторонние плагины
- творческий подход приветствуется

## Комментарии по реализации:

<a href="https://yapx.ru/v/GNzGn"><img src="https://i.yapx.ru/GNzGn.gif"></a>

1) Управление: WASD или стрелочки для движения, цифры 1,2,3 - для смены оружия (3 вида оружия), левая кнопка мыши, контрол или тап для стрельбы.
 
2) Реализована плавная камера, которая плавно движется за героем, при этом смещается в сторону прицела, если герой долго некоторое время смотрит в одну сторону.
 
3) Решил не делать банальную стрельбу через пул снарядов-объектов, а попробовал реализовать стрельбу полностью на системе частиц, что бы можно было легко и дешево реализовать стрельбу красивыми эффектами и думаю, что у меня удалось, т.к. все "снаряды" считаются на GPU + лишены проблем с пролетом очень быстрого снаряда через коллайдер врага.
Попадания отслеживаются через OnParticleCollision + GetCollisionEvents(для учета всех попаданий частиц за кадр)  на врагах, т.к. метод передает объект партиклСистемы которая выпустила попавший партикл, то что бы узнать урон оружия(системы частиц), нужно добраться до соответствующего компонента через GetComponent, поэтому я дополнительно реализовал систему кеширования атакующего партиклом оружия для всех врагов сразу - в итоге вызывается всего 1 GetComponent на всю группу врагов для каждого вида атакующего оружия (т.е. в текущей игре с 3 оружиями всего 3 раза за игру).
 
4) Спавнер врагов спавнит в указанном радиусе с учетом коллайдера спавнящегося юнита (юнит гарантированно не будет вылезать за радиус спавна никакими своими частями). Так же перед спавном проверяется не занято ли место в которое будет размещаться юнит. Создание врагов происходит заранее в пул, пул пополняется автоматически если врагов недостаточно. Радиус спавнера рисуется в эдиторе для удобства.
