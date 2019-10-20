## Задачи ботов

Для чего нужны боты? Наверняка, именно этот вопрос вы зададите, услышав о них впервые. Чтобы ответить на него, нам придется обратиться к истории.

Одно из самых ранних упоминаний термина **игровой бот** встречается применительно к играм в жанре [**шутер от первого лица**](https://ru.wikipedia.org/wiki/%D0%A8%D1%83%D1%82%D0%B5%D1%80_%D0%BE%D1%82_%D0%BF%D0%B5%D1%80%D0%B2%D0%BE%D0%B3%D0%BE_%D0%BB%D0%B8%D1%86%D0%B0) (first-person Shooter или FPS). Проблема появилась с момента возникновения режима **игрок-против-игрока** (player versus player или PvP), в котором пользователи могли соревноваться друг с другом. Некоторым из них хотелось подготовиться к состязанию самостоятельно, либо у них не было возможности подключаться к сети регулярно. Для этой цели требовались оппоненты, управляемые компьютерной программой, а не человеком.

Важно отметить, что новый соревновательный режим значительно отличается от однопользовательского, который возник намного раньше. В одиночной игре пользователь проходит уровни один за другим, решая головоломки и сражаясь с противниками. Поведение этих противников крайне примитивно: они стоят в предопределенных точках уровня и реагируют только при приближении игрока. Эти задачи решаются [**игровым искусственным интеллектом**](https://ru.wikipedia.org/wiki/%D0%98%D0%B3%D1%80%D0%BE%D0%B2%D0%BE%D0%B9_%D0%B8%D1%81%D0%BA%D1%83%D1%81%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D1%8B%D0%B9_%D0%B8%D0%BD%D1%82%D0%B5%D0%BB%D0%BB%D0%B5%D0%BA%D1%82).

В соревновательном режиме жанра FPS от ИИ требуется большего. Он должен свободно перемещаться по игровому уровню, собирать оружие и боеприпасы, выбирать подходящий момент для нападения на противника и отступления. Другими словами ИИ должен вести себя или, по крайней мере, притворяться игроком-человеком. Именно такой вид ИИ получил название [**бот**](https://ru.wikipedia.org/wiki/%D0%91%D0%BE%D1%82_%28%D0%BA%D0%BE%D0%BC%D0%BF%D1%8C%D1%8E%D1%82%D0%B5%D1%80%D0%BD%D1%8B%D0%B5_%D0%B8%D0%B3%D1%80%D1%8B%29).

С развитием компьютерных игр возникли новые задачи для ИИ. Распространение интернета привело к росту популярности [**массовых многопользовательских ролевых онлайн-игр**](https://ru.wikipedia.org/wiki/%D0%9C%D0%B0%D1%81%D1%81%D0%BE%D0%B2%D0%B0%D1%8F_%D0%BC%D0%BD%D0%BE%D0%B3%D0%BE%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D1%81%D0%BA%D0%B0%D1%8F_%D1%80%D0%BE%D0%BB%D0%B5%D0%B2%D0%B0%D1%8F_%D0%BE%D0%BD%D0%BB%D0%B0%D0%B9%D0%BD-%D0%B8%D0%B3%D1%80%D0%B0) (massively multiplayer online role-playing game или MMORPG). Этот новый жанр имеет много общего с классическими [**ролевыми играми**](https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D0%BC%D0%BF%D1%8C%D1%8E%D1%82%D0%B5%D1%80%D0%BD%D0%B0%D1%8F_%D1%80%D0%BE%D0%BB%D0%B5%D0%B2%D0%B0%D1%8F_%D0%B8%D0%B3%D1%80%D0%B0) (role-playing game или RPG), но в отличие от них игровой процесс стал более растянутым по времени из-за большого числа участников. Кроме того, в MMORPG разработчики стремятся поддержать интерес пользователей как можно дольше. Эти особенности привели к более длительному развитию игрового персонажа. Теперь требуются недели, а иногда и месяцы, для выполнения квестов и добычи ресурсов. Благодаря этому повышается уровень персонажа, который важен для сражения с другими игроками. Именно этот соревновательный режим и является главной привлекательной чертой MMORPG.

Некоторым игрокам процесс развития персонажа может показаться скучным из-за постоянного повторения одних и тех же действий. Рано или поздно, они задумаются о способах его автоматизации. Разработчики некоторых MMORPG предоставляют средства для создания расширений, которые добавляют некоторую автономность персонажу. Но, как правило, таких средств нет или они оказываются недостаточными. Для расширения функциональности игры требуются возможности не предусмотренные разработчиками. Обычно такие расширения запрещены и блокируются программным путём, потому что издатель игры теряет из-за них деньги: благодаря автономности персонажа, игроки проводят меньше времени онлайн и совершают меньше внутриигровых покупок. Такие средства автоматизации в MMORPG были также названы ботами. Возможно, причина в том, что эти программы имитируют поведение игрока-человека точно так же, как в шутерах.

Автоматизация игрового процесса – не единственная задача, возникшая после появления новых жанров онлайн-игр. Некоторые увлеченные соперничеством пользователи начали искать пути обхода правил, чтобы получить нечестное преимущество. Например, приобрести необходимые ресурсы или дополнительную информацию о состоянии игры, изменить характеристики персонажей и т.д. Приложения для расширения функциональности игры с целью обхода её правил называются **читы** (cheats), **хаки** (hacks) и иногда боты. Это вызывает определённую путаницу. Жульничество в компьютерных играх – это не то же самое, что автоматизация. В этой книге мы проведем чёткую грань между читами и ботами. Боты – это средства для имитации поведения игрока, и именно их мы будем рассматривать.

Боты могут решать различные задачи. Они дают возможность пользователям тренироваться перед соревнованиями с другими людьми в шутерах и иных киберспортивных дисциплинах. Боты могут ускорять развитие персонажа в MMORPG. Они также дают конкурентное преимущество в соревновательных играх, путем модификации игрового процесса.