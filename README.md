# オブジェクト指向型プログラミングについて　備忘録

## 前提

### なぜオブジェクト指向は理解しにくいのか

- 抽象的な概念を扱うから。
- 2年前期扱ってきた書き方とは __プログラミングパラダイム__ が違うから。

---

### プログラミングパラダイムとは

プログラムを表現する上での考え方。

pythonでは以下の4つのパラダイムが共存している。

1. __構造化プログラミング__
1. __手続き型プログラミング__
1. __オブジェクト指向型プログラミング__ (クラスベース)
1. __関数型プログラミング__

パラダイムの移り変わり (__パラダイムシフト__) の前後では、考え方が全く違う。

2年前期までは __構造化__ と __手続き型__ のパラダイムで書いてきた為に __オブジェクト指向型__ (クラス) で躓くのは当然。

以下で __構造化__ __手続き型__ について簡単に説明すると

#### 構造化プログラミング

- __順次__ : 上から順番に実行
- __選択__ : 条件分岐でルートを選ぶ → __if文__
- __反復__ : 繰り返す → __for文__

の3つであらゆる処理は表すことができるという考え方。

#### 手続き型

プログラムを手続き(__関数__)に分割して呼び出すことで、コードの再利用性を高めようという考え方。

---

以上の通り、ここまではとても馴染みのある考え方で、難なく理解できると思う。

## オブジェクト指向とは

### オブジェクト指向型プログラミングの基本的な考え

めちゃめちゃ端折って説明すると、

__より自然言語っぽく短くプログラミングができるようにしようぜ__ という考えで、それを実現するために __クラス__ を使う。

---

### そもそもオブジェクトってなに

ざっくり言うと

__クラス(設計図)__ をもとに作られた __実体__。

抽象的で理解しがたいのでまずクラスについて解説する。

## クラス

__クラス__ とは、上記の通り __オブジェクト__ を表現するための設計図。


__クラス__ には以下の3つの方針がある。

- __カプセル化__ : コードが長くなるといっぱい色んなの増えて何がなんだかよくわからんくなるから、使わんときは隠したろ。(pythonはここが微妙)
- __継承__ : 大体同じでちょっと違うだけなのに、共通部分も全部書き直すのだるくね？ 違うとこだけ書き直せるようにしようぜ。
- __ポリモーフィズム(多態性)__ : 同じ命令で違う処理を呼べるようにしようぜ。

これらを活用することで、とても見やすく、かつ管理しやすいコードが書ける。(最近はまた違う考えが流行ってる。)

クラス内で使用する変数は、 __フィールド__

クラス内で定義した関数を、 __メソッド__

と呼ばれる。

---

### 具体的に何が嬉しい？

例として、__RPGゲーム__ を作る。

キャラクターとして、 __勇者__　__戦士__　__ゴブリン__　を用意する。

2年前期までのパラダイムで記述しようとすると、それぞれのキャラクターのHPや攻撃力 ( _warrior_hp_ 等 ) をそれぞれ用意して、_attack()_ 等の関数をそれぞれ用意して、ifやforを使用して制御することになる。

~~~python

def attack(attacker_name, victim_name, attacker_atk, victim_hp, victim_df):
    damage = attacker_atk - victim_df   # ダメージ = 攻撃者の攻撃力 - 被害者の防御力
    after_victim_HP = victim_hp - damage   # 被害者の体力 = 被害者の体力 - ダメージ
    print(f"{attacker_name}が{victim_name}に攻撃!　{victim_name}の体力が{attacker_atk - victim_df}減少!")   # メッセージ
    return after_victim_HP

hero_name = "勇者"
hero_hp = 150    # 体力
hero_atk = 100   # 攻撃力
hero_df = 10     # 防御力

warrior_name = "戦士"
warrior_hp = 150
warrior_atk = 80
warrior_df = 20

goblin_name = "ゴブリン"
goblin_hp = 80
goblin_atk = 30
goblin_df = 20

wizard_HP = attack(goblin_name, warrior_name, goblin_atk, warrior_hp, warrior_df)   #ゴブリンが戦士に攻撃する


# ログ : ゴブリンが戦士に攻撃!　戦士の体力が10減少!
~~~

ここから更にキャラクターを増やしたり、関連するパラメーターが増えていくと、その分条件分岐や変数, 引数も増えていく。

例えば魔法使いを増やし、攻撃はMPを消費して防御力を無視するとしたらもっと複雑になるだろう。

オブジェクト指向を活用すれば、より読みやすく、キャラクターの変更や追加も容易にすることができる。

---

### カプセル化

上記の例では、それぞれのキャラクターのパラメーターを同じ場所に記述しており、管理がしにくい上、変数名も長くなりがちである。

__カプセル化__ によって、この悩みは解決することができる。

まずは勇者のクラスを定義してクラスの中で各パラメーターを設定してみる。

~~~python

class Hero():
    def __init__(self):
        self.name = "勇者"
        self.hp = 150
        self.atk = 100
        self.df = 10

~~~

そして、このクラスを __インスタンス化__ してパラメータを取り出してみる。

__インスタンス化__ とは、設計図から実体を作り出す工程であり、ここで初めて __オブジェクト__ が作られる。

~~~python

class Hero():
    def __init__(self):
        self.name = "勇者"
        self.hp = 150
        self.atk = 100
        self.df = 10

hero = Hero() # hero に、Heroのオブジェクトを代入 = インスタンス化
print(hero.hp)


# ログ : 150
~~~

hero.hpで勇者の体力を取り出す事ができた。

同じように戦士とゴブリンも定義する。

~~~python

class Hero():
    def __init__(self):
        self.name = "勇者"
        self.hp = 150
        self.atk = 100
        self.df = 10


class Warrior():
    def __init__(self):
        self.name = "戦士"
        self.hp = 150
        self.atk = 80
        self.df = 20


class Goblin():
    def __init__(self):
        self.name = "ゴブリン"
        self.hp = 80
        self.atk = 30
        self.df = 20

~~~

そして、ゴブリンのクラスにattackメソッドを定義し、先程と同じように戦士に攻撃させてみる。

~~~python

class Hero():
    def __init__(self):
        self.name = "勇者"
        self.hp = 150
        self.atk = 100
        self.df = 10


class Warrior():
    def __init__(self):
        self.name = "戦士"
        self.hp = 150
        self.atk = 80
        self.df = 20


class Goblin():
    def __init__(self):
        self.name = "ゴブリン"
        self.hp = 80
        self.atk = 30
        self.df = 20

    """
    メソッド追加
    """
    def attack(self, victim):
        damage = self.atk - victim.df   # ダメージ = ゴブリンの攻撃力 - 相手の防御力
        victim.hp = victim.hp - damage   # 相手の体力 = 相手の体力 - ダメージ
        print(f"{self.name}が{victim.name}に攻撃!　{victim.name}の体力が{self.atk - victim.df}減少!")   # メッセージ


warrior = Warrior()
goblin = Goblin()

print("攻撃前の体力 : ", warrior.hp)
goblin.attack(warrior)   # ゴブリンが戦士に攻撃
print("攻撃後の体力 : ", warrior.hp)


#ログ :
"""
攻撃前の体力 :  150
ゴブリンが戦士に攻撃!　戦士の体力が10減少!
攻撃後の体力 :  140
"""
~~~

攻撃処理の呼び出し部分に注目すると、

~~~python

#クラス未使用
wizard_HP = attack(goblin_name, warrior_name, goblin_atk, warrior_hp, warrior_df)   # ゴブリンが戦士に攻撃

#クラス使用
goblin.attack(warrior)   # ゴブリンが戦士に攻撃
~~~

明らかに短く、そして自然言語に近い書き方にできた。

ここでは、戦士のパラメーターを引数に渡す必要はなく、戦士( __オブジェクト__ )そのものを引数として渡し、attack メソッドの中で __オブジェクト名.パラメーター名__ として取得し、処理している。

ゴブリンのパラメーターに至っては、__self.パラメーター名__ で取得できる為、引数に入れる必要すらない。(__self__ = __自分自身のオブジェクト__)


このように、表に出す必要の無い情報を、隠しておく (__情報隠蔽__) という方針が __カプセル化__ である。

---

### 継承

続いて、attack メソッドを勇者と戦士にも定義したいが、それぞれの attack メソッドの処理は同じである。

同じことをそれぞれで書いていては、非効率的で、修正や変更が発生するとキャラクター分工数が増えてしまう。

__継承__ を使うとその問題が解決する。

まず、__キャラクター__ という 勇者 戦士 ゴブリン よりもぼんやりした、抽象的なクラスを定義してみる。

~~~python

class Character():
    def __init__(self):
        pass

~~~

そして、ここに attack メソッドを追加する。

~~~python

class Character():
    def __init__(self):
        pass

    def attack(self, victim):
        damage = self.atk - victim.df
        victim.hp = victim.hp - damage
        print(f"{self.name}が{victim.name}に攻撃!　{victim.name}の体力が{self.atk - victim.df}減少!")

~~~

そしてこのクラスを、勇者 戦士 ゴブリン で __継承__ する。

~~~python

class Character():
    def __init__(self):
        pass

    def attack(self, victim):
        damage = self.atk - victim.df
        victim.hp = victim.hp - damage
        print(f"{self.name}が{victim.name}に攻撃!　{victim.name}の体力が{self.atk - victim.df}減少!")


class Hero(Character):   # クラス定義時の引数にクラスを渡すことで、継承することができる。
    def __init__(self):
        self.name = "勇者"
        self.hp = 150
        self.atk = 100
        self.df = 10


class Warrior(Character):
    def __init__(self):
        self.name = "戦士"
        self.hp = 150
        self.atk = 80
        self.df = 20


class Goblin(Character):
    def __init__(self):
        self.name = "ゴブリン"
        self.hp = 80
        self.atk = 30
        self.df = 20
~~~

__継承__ することで、__親クラス (基底クラス)__ で定義されたメソッドをすべて引き継ぐことができる。

つまり、それぞれキャラクターのパラメーターだけ設定していれば、attack メソッドを共通で使うことができる。

実際に 勇者 から ゴブリン に対して攻撃してみる。

~~~python

class Character():
    def __init__(self):
        pass

    def attack(self, victim):
        damage = self.atk - victim.df
        victim.hp = victim.hp - damage
        print(f"{self.name}が{victim.name}に攻撃!　{victim.name}の体力が{self.atk - victim.df}減少!")


class Hero(Character):   # クラス定義時の引数にクラスを渡すことで、継承することができる。
    def __init__(self):
        self.name = "勇者"
        self.hp = 150
        self.atk = 100
        self.df = 10


class Warrior(Character):
    def __init__(self):
        self.name = "戦士"
        self.hp = 150
        self.atk = 80
        self.df = 20


class Goblin(Character):
    def __init__(self):
        self.name = "ゴブリン"
        self.hp = 80
        self.atk = 30
        self.df = 20


hero = Hero()
goblin = Goblin()

print("攻撃前の体力 : ", goblin.hp)
hero.attack(goblin)   # 勇者がゴブリンに攻撃
print("攻撃後の体力 : ", goblin.hp)


#ログ :
"""
攻撃前の体力 :  80
勇者がゴブリンに攻撃!　ゴブリンの体力が80減少!
攻撃後の体力 :  0
"""
~~~

問題なく実行できた。

この構造にしておけば、attack メソッドに変更を加えたい場合、Characterのattack メソッドを変更するだけで継承先でも反映することができる。

メソッドやキャラクターが増えるほど、この恩恵は大きくなる。

ここからさらに、__魔法使い__ を追加し、パラメーターとして __MP__ を追加、魔法使いの攻撃は相手の防御力を無視して、MPを消費することにする。

このように、一部キャラクターのメソッド (この場合 attack) の振る舞いを変えたい場合、メソッドの __オーバーライド__ をすることで実現できる。

~~~python

class Character():
    def __init__(self):
        pass

    def attack(self, victim):
        damage = self.atk - victim.df
        victim.hp = victim.hp - damage
        print(f"{self.name}が{victim.name}に攻撃!　{victim.name}の体力が{self.atk - victim.df}減少!")


class Hero(Character):
    def __init__(self):
        self.name = "勇者"
        self.hp = 150
        self.atk = 100
        self.df = 10
        self.mp = 50


class Warrior(Character):
    def __init__(self):
        self.name = "戦士"
        self.hp = 150
        self.atk = 80
        self.df = 20
        self.mp = 50


class Goblin(Character):
    def __init__(self):
        self.name = "ゴブリン"
        self.hp = 80
        self.atk = 30
        self.df = 20
        self.mp = 30


"""
魔法使い追加
"""
class Magician(Character):
    def __init__(self):
        self.name = "魔法使い"
        self.hp = 80
        self.atk = 80
        self.df = 5
        self.mp = 100   # MPを追加

    def attack(self, victim):   # attackメソッドをオーバーライド(上書き)して独自の処理にする
        if self.mp <= 0:   # mpが0以下の場合、攻撃失敗
            print("MPが足りません!")
            return

        damage = self.atk
        victim.hp = victim.hp - damage
        self.mp -= 10  # 攻撃後MPを10減らす
        print(f"{self.name}が{victim.name}に攻撃!　{victim.name}の体力が{self.atk}減少!")


magician = Magician()
goblin = Goblin()

print("攻撃前のMP : ", magician.mp)
magician.attack(goblin)   # 魔法使いがゴブリンに攻撃
print("攻撃後のMP : ", magician.mp)


#ログ :
"""
攻撃前のMP :  100
魔法使いがゴブリンに攻撃!　ゴブリンの体力が80減少!
攻撃後のMP :  90
"""
~~~

無事に実行することができた。

次は、敵キャラクター __吸血鬼__ を追加し、攻撃すると体力が回復する使用にする。

魔法使いと同じようにメソッドを __オーバーライド__ してもいいが、攻撃自体はゴブリンなどと同じ挙動で、攻撃後に処理を追加したい。

この場合、__super__ という機能を使う。

__super__ を使用すると、オーバーライドしたメソッド内で、オーバーライド前の機能を呼び出すことができる。

~~~python
class Character():
    def __init__(self):
        pass

    def attack(self, victim):
        damage = self.atk - victim.df
        victim.hp = victim.hp - damage
        print(f"{self.name}が{victim.name}に攻撃!　{victim.name}の体力が{self.atk - victim.df}減少!")


class Hero(Character):
    def __init__(self):
        self.name = "勇者"
        self.hp = 150
        self.atk = 100
        self.df = 10
        self.mp = 50

"""
~略~
"""

class Vampire(Character):
    def __init__(self):
        self.name = "吸血鬼"
        self.hp = 60
        self.atk = 70
        self.df = 10
        self.mp = 20

    def attack(self, victim):
        super().attack(victim)   # 継承元の処理を呼び出す
        self.hp += 10              # 以下独自の処理
        print(f"{self.name}の体力が10回復!")


vampire = Vampire()
hero = Hero()

vampire.attack(hero)   # 吸血鬼が勇者に攻撃


#ログ :
"""
吸血鬼が勇者に攻撃!　勇者の体力が60減少!
吸血鬼の体力が10回復!
"""
~~~

このように、__super__ によって元メソッドの処理を呼び出すことができ、元メソッドに変更が加われば Vampire クラスも影響を受けることになる。

---

### ポリモーフィズム

以上のサンプルコードでは、キャラクターが変わっても攻撃の命令を常に __攻撃者.attack(被害者)__ の形式で呼び出している。

魔法使いや吸血鬼のように attack メソッドの振る舞いが異なっていても、常に同じ命令で表現できる性質のことを、__ポリモーフィズム(多態性)__ と呼ぶ。

---

## 以上
