# Ёбаный Git!!!

> Git сложен: легко всё проебать, и нереально понять как исправить. Документация Git - это финиш: чтобы найти решение, тебе заранее надо знать название фишки, которая вернет всё на место.

Git сложен: легко всё проебать, и нереально понять как исправить. Документация Git - это финиш: чтобы найти решение, _тебе заранее надо знать название фишки_, которая вернет всё на место.

_Популярно_ опишу несколько ситуаций, из которых мне пришлось выбираться.

[Бля, я накосячил, где у git волшебная машина времени!?!](#волшебная-машина-времени)
------------------------------------------------------------------------------------

    git reflog
    
    
    
    
    git reset HEAD@{index}
    

Используйте это чтобы вернуть случайно удалённые штуки, или убрать то чем Вы всё сломали, или восстановиться после неудачного слияния, или просто вернуться туда, когда всё работало. Я ОЧЕНЬ ЧАСТО использую `reflog`. Снимаю шляпу перед теми, кто предложил добавить это.

[Бля, я закоммитил и вспомнил, что кое-что забыл!](#изменить-последний-коммит)
------------------------------------------------------------------------------

    
    git add . # или добавьте файлы по отдельности
    git commit --amend --no-edit
    
    

Обычно я это использую когда коммичу, потом запускаю тесты/сканеры... и блин, я не поставил пробел после знака равно. Также можно сделать изменения в новом коммите и использовать `rebase -i` чтобы склеить оба коммита вместе, но так в миллион раз быстрее.

_Предупреждение: никогда не изменяйте коммиты, отправленные в публичную ветку! Изменяйте только коммиты в вашей локальной ветке, иначе Вам не поздоровится._

[Бля, мне нужно изменить сообщение моего последнего коммита!](#изменить-сообщение-последнего-коммита)
-----------------------------------------------------------------------------------------------------

    git commit --amend
    

Ёбаные требования по наименованию.

[Бля, Я случайно закоммитил что-то в мастер, хотя это должно быть в новой ветке!](#случайный-коммит-в-мастер)
-------------------------------------------------------------------------------------------------------------

    
    git branch какое-то-имя-новой-ветки
    
    git reset HEAD~ --hard
    git checkout какое-то-имя-новой-ветки
    

NB: это не будет работать, если вы уже отправили коммит в удалённую ветку, и если вы пробовали сделать это как-то по-другому, может помочь `git reset HEAD@{количество-коммитов-назад}` вместо `HEAD~`. Ёбушки-воробушки. Так же многие люди предлагали сделать то же самое, но короче. Спасибо всем!

[Бля, я случайно закоммитил не в ту ветку!](#случайный-коммит-не-в-ту-ветку)
----------------------------------------------------------------------------

    
    git reset HEAD~ --soft
    git stash
    
    git checkout имя-нужной-ветки
    git stash pop
    git add . # или добавьте отдельные файлы
    git commit -m "ваше сообщение здесь"
    

Многие люди предлагали использовать `cherry-pick` в такой ситуации, так что выбирайте, то что вам больше нравится!

    git checkout имя-нужной-ветки
    
    git cherry-pick master
    
    git checkout master
    git reset HEAD~ --hard

[Бля, я пытаюсь открыть diff, но ничего не происходит?!](#где-мой-diff)
-----------------------------------------------------------------------

Если вы уверены, что изменили файлы, но `diff` пуст, возможно вы индексировали изменения (`add`) и нужно добавить специальный флаг.

    git diff --staged

¯\\\_(ツ)\_/¯ (да, я знаю, что это не баг, а фича, но это нихуя не очевидно с первого раза!)

[Бля, мне нужно отменить коммит, который был 5 коммитов назад!](#отменить-коммит)
---------------------------------------------------------------------------------

    
    git log
    
    
    git revert [сохранённый хеш]
    
    
    

Вам не нужно откатываться назад и копипастить старый файл в существующий! Если вы закоммитили хрень, её можно убрать с `revert`.

Также можно отменить только один файл вместо целого коммита! Но конечно, (как всегда у git\`а) это совершенно другой набор чёртовых команд...

[Бля, мне нужно отменить изменения в файле!](#отменить-изменения)
-----------------------------------------------------------------

    
    git log
    
    
    git checkout [сохранённый хеш] -- путь/к/файлу
    
    git commit -m "Ого, теперь не придётся копипастить, чтобы отменить изменения!"

Когда до меня это дошло, это было ОХУЕННО. Если серьезно, то схуяли `checkout --` лучший способ отменять изменения? :угрожает-линусу-торвальдсу:

[Нахуй всё, я сдаюсь.](#к-чёрту-всё)
------------------------------------

    cd ..
    sudo rm -r чёртов-репозиторий-git
    git clone https://some.github.url/чёртов-репозиторий-git.git
    cd чёртов-репозиторий-git

Спасибо Eric V. за эту подсказку. Все жалобы по поводу использованию `sudo` в этой шутке могут быть направлены сразу ему.

Вообще говоря, если ваша ветка настооолько загажена, что нужно вернуться к удалённому состоянию в "git-корректном стиле" попробуйте это, но это необратимо!

    
    git fetch origin
    git checkout master
    git reset --hard origin/master
    
    git clean -d --force
    

\*Предупреждение: Этот сайт не является исчерпывающим руководством. Да, есть другие способы сделать тоже самое, но я пришёл к этому методом проб и ошибок, и теперь делюсь этим с большой дозой легкомыслия и ругани. Примите это или уйдите!

Большое спасибо людям, которые перевели этот сайт на другие языки: [Michael Botha](https://github.com/michaeljabotha) ([af](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/af)) · [Khaja Md Sher E Alam](https://github.com/sheralam) ([bn](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/bn)) · [Eduard Tomek](https://github.com/edee111) ([cs](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/cs)) · [Moritz Stückler](https://github.com/pReya) ([de](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/de)) · [Franco Fantini](https://github.com/francofantini) ([es](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/es)) · [Hamid Moheb](https://github.com/hamidmoheb1) ([fa](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/fa)) · [Senja Jarva](https://github.com/sjarva) ([fi](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/fi)) · [Michel](https://github.com/michelc) ([fr](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/fr)) · [Alex Tzimas](https://github.com/Tzal3x) ([gr](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/gr)) · [Elad Leev](https://github.com/eladleev) ([he](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/he)) · [Aryan Sarkar](https://github.com/aryansarkar13) ([hi](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/hi)) · [Ricky Gultom](https://github.com/quellcrist-falconer) ([id](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/id)) · [fedemcmac](https://github.com/fedemcmac) ([it](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/it)) · [Meiko Hori](https://github.com/meih) ([ja](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/ja)) · [Zhunisali Shanabek](https://github.com/zshanabek) ([kk](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/kk)) · [Gyeongjae Choi](https://github.com/ryanking13) ([ko](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/ko)) · [Rahul Dahal](https://github.com/rahuldahal) ([ne](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/ne)) · [Martijn ten Heuvel](https://github.com/MartijntenHeuvel) ([nl](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/nl)) · [Łukasz Wójcik](https://github.com/lwojcik) ([pl](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/pl)) · [Davi Alexandre](https://github.com/davialexandre) ([pt\_BR](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/pt_BR)) · [Catalina Focsa](https://github.com/catalinafox) ([ro](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/ro)) · [Daniil Golubev](https://github.com/dadyarri) ([ru](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/ru)) · [Nemanja Vasić](https://github.com/GoodbyePlanet) ([sr](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/sr)) · [Björn Söderqvist](https://github.com/cybear) ([sv](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/sv)) · [Kitt Tientanopajai](https://github.com/kitt-tientanopajai) ([th](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/th)) · [Taha Paksu](https://github.com/tpaksu) ([tr](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/tr)) · [Andriy Sultanov](https://github.com/LastGenius-edu) ([ua](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/ua)) · [Tao Jiayuan](https://github.com/taojy123) ([zh](chrome-extension://cjedbglnccaioiolemnfhjncicchinao/zh)) . С дополнительной помощью от [Allie Jones](https://github.com/alliejones) · [Artem Vorotnikov](https://github.com/vorot93) · [David Fyffe](https://github.com/davidfyffe) · [Frank Taillandier](https://github.com/DirtyF) · [Iain Murray](https://github.com/imurray) · [Lucas Larson](https://github.com/LucasLarson) · [Myrzabek Azil](https://github.com/mvrzvbvk)

Чтобы добавить перевод на свой язык, отправьте PR на [GitHub](https://github.com/ksylor/ohshitgit)


[Source](https://ohshitgit.com/ru)