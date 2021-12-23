# solution_clash_new
 The latest backend of the solution clash project
day1 2021-11-28
solution clash是一个化学小游戏，玩家消耗费用，通过将卡牌（化合物/单质）加入自己或对方烧杯中进行进攻防御。
## 1. 游戏部分
每个人有8张卡牌，轮换使用。
## 2. 化学部分
呜咕指数：每0.1mol/L氢离子=1呜咕指数，每0.1mol/L氢氧根离子作为=-1呜咕指数，0.5mol/L以下氢离子不计
弱碱/弱酸/强碱弱酸盐/强酸弱碱盐视作强酸强碱的一半效果（0.5呜咕指数）
难溶性弱碱在呜咕指数>0时视作强酸，否则无效
水量：玩家拥有的防御力，初始为6，双水解、碱金属扣分，水合物加分。
判定胜负的方式是呜咕指数的绝对值>剩余水量
## 3. 游戏部分
游戏分回合，每回合两个用户在同一台电脑上玩。所以回合也分玩家1的部分与玩家2部分。
## 设计
Beaker(cH, water, Ions, owner)
cH：呜咕指数
water：防御力
Ions：离子，仅包含弱酸根弱碱根
owner：烧杯的所有者（玩家）

add_ion：加入离子/改变离子数量/去除数量变为0的离子
ioni：计算加入物品之后的烧杯状况，改变cH, water, Ions
    这个的具体实现很简单。
    首先我们的烧杯的cH默认是0，
    而每张卡牌都写成了[1,0,["Na+"]]这种形式，第0项表示它对cH的改变量，第1项是water的改变量，第2项是对Ions的改变量
    每次放牌叠加上去就可以了
check_DH：检查是否发生双水解反应并处理
    首先单独提取溶液中的阴阳离子列表，然后使用类似辗转相减的方法抵消到只剩下阴阳离子中的一种为止。
check_end：检查本烧杯的呜咕指数是否超出防御力

Player(name, card_comb, gold)
name：玩家的名字，输入
card_comb：玩家的卡牌组合
gold：玩家可以用于出卡的“费用”，每回合会初始化。


Gameplay(turn, curPlayer)
turn：回合
curPlayer：当前出牌的玩家

endgame：指出失败的一方
nxt_turn：进入下一回合
nxt_player：切换到下一个玩家


---
player: [{
    score: 0,
    cards: [],
    credit: 3,
    water: 6
}]