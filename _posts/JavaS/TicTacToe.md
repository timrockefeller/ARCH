---
title: Tic Tac Toe “我也不知道是什么”算法
date: 2018-05-22 22:39:00
tags: [coding,java]
---

为了巩固类、数组等知识，下来一个Assignment，定睛一看，居然是个四子棋！没想到人在江湖，也会遇上如此困难之题，随缘写个备注以防自己忘记。

完整代码已上传至 [timrockefeller/ExplotionEuler](https://github.com/timrockefeller/ExplotionEuler/blob/master/src/TicTacToe.java)。

<!--more-->

---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=411500393&auto=0&height=66"></iframe>


这次我请到了我在崔斯特姆遇见的朋友，Millie桑！（欢呼、鼓掌）

Millie: *大家好嘿，我是Millie，今天呢很高兴见到大家耶！*

对于这个棋局，你怎么看呢？

Millie: *什么棋局？*

那个啥、井字棋啊。

Millie: *噢那个我没输过诶~*

巧了我也没输过。回归正题，Millie你就坐旁边看就好了。

Millie: *这么没人权吗quq？是你让我过来的诶、*

导演开始吧。

---

## 构建棋盘

棋盘就用3\*3数组表示吧，数组类型可以记为枚举**ChessType**。
```java
enum ChessType{
    CROSS,
    CIRCLE,
    BLANK,
    NULL
}
```
这里分为四种：**叉、圈、空、未知**。这个“未知”只是用于检测是否越界来着，棋盘之外便是**虚空**。

接着我们需要写第一个类：**Board**，它需要完成存储、读写、判断、输出图形这些功能。让Miilie带我们看看这个类的组成部分：

```java
//嗨嗨，我是Millie~~电视台终于转到我这里了
class Board{
    public Board ();//构造函数，创建一个map被ChessType.BLANK填满的新地图。
    public Board (ChessType[][] _map);//构造函数，创建一个和传入_map完全相同的复制。
    private ChessType[][] map;//地图保存的地方，用[x][y]获取。
    public ChessType getBoard(Position pos);//检验是否越界，返回这个点的ChessType，越界则返回ChessType.NULL。
    public boolean setBoard(Position pos,ChessType type);//将pos位置的棋子设置为type，检测非空、不越界。成功就返回True~
    public void setBoardF(Position pos,ChessType type);//全称是setBoard Force！或者FFF，反正就是强行设置的意思啦~
    public void drawBoard();//画画画~画棋盘，怎么好看怎么画，疯狂println。
    private String gs(ChessType t);//为了drawBoard服务，把t变成圈圈○或叉叉×。
    public ChessType judge();//JUDGE~~!!会返回到底谁连成一条线了！没有的话就返回ChessType.BLANK。
    public boolean isFull();//棋盘是不是满了？被……灌满了❤？
    public Board clone();//返回一个复制体，用到上面的重载构造函数~
}
```
Millie: *细节就不放了，其中的Position类：*
```java
class Position{
    public Position(){
        this.x=0;this.y=0;
    }
    public Position(int x, int y){
        this.x = x;this.y = y;
    }
    public int x;//坐标x
    public int y;//坐标y
}
```
超简单的，传入pos等变量的时候直接 `new Position(x,y)` 就好了，当然开发大型游戏的时候它就不能叫Position了，而是**Vector2**(二维向量)，相关函数在[MizuhaPic](https://github.com/timrockefeller/MizuhaPiC/blob/v0.0/Vector2.h)这个写了一半~~（没有，没有一半，连1%都没有）~~的游戏引擎里出现过。

Millie: *这个类在后面经常被用到，不停地被克隆，是这个作业、呸游戏的重要成员。以及MizuhaPic怕是要弃坑了。*

棋盘有了，我们还需要玩家。玩家分为两类：**人**和**电脑**。他们都可以继承于同一个抽象类：**Player**。

```java
abstract class Player{
    //工作流程是：
    // 1. 轮到玩家了
    // 2. onTurn()执行并把hand调整到想要落子的地方
    // 3. 外面函数把这hand给落下去
    public abstract void onTurn(Board b);//这就是那个函数，每次回合开始都需要调用这个函数
    public Position hand;//临时存放准备落子位置
    public ChessType type;//确定玩家是叉♂叉还是圈♀圈
}
```
人类玩家就可以用`new Scanner().NextInt()`来获取棋子位置。
```java
class Player_Human extends Player{
    public Player_Human(ChessType _type){//构造函数呗
        this.type= _type; this.hand = new Position();
    }
    @Override
    public void onTurn(Board b){
        this.readInput();//直接跳到下面去了……
    }
    private void readInput(){//从上面跳水下来
        Scanner input = new Scanner (System.in);//垃圾话
        System.out.print("input x and y (1-3): ");//垃圾话2
        this.hand.x = input.nextInt() - 1;//输入棋子位置x
        this.hand.y = input.nextInt() - 1;//输入棋子位置y
    }
}
```
那么我们准备好了玩家、棋盘，就需要找个房间来玩，于是我们的老大**Game**类就来了。同样让Millie为我们解说。
```java
class Game {
    public Game();//构造函数，运行的时候顺便new几个player，可以选择一波~ 最后`this.run()`走起！
    private int round;//回合数
    private Board board;//棋盘咯？
    private Player[] players = new Player[2];//两位玩家的凉席
    private int turn;//谁的回合？你的？p，我的。
    private boolean gaming;//配合run()函数的使用，判断时候游戏结束。
    private void nextTurn();//轮到下个玩家啦~
    private ChessType getTurnType(int _turn);//把数字0、1变成ChessType
    private void run();//新的游戏~里面有个while(this.gaming)，来结束游戏。
    private void gameEnd(ChessType type);//赢啦~说些什么~
    private void retry();//老板，再来一局！！
}
```
巧妙的是这个类只用生成一个对象，也就是传说中的游戏开发设计模式里的**单例模式**，应该吧。
很棒，一个完整的TicTacToe游戏就完成啦，好好玩玩吧~

---

*以下是未知代码，谨慎阅读！*

Millie: _在设计机器人的时候，根本不知道怎么写，好难受、好难受，硬憋憋出一个不明效果的回溯方法。具体就是：检测所有的可走路径**Class.Way**，保存在**ArrayList<Ways>**里面，虽然有胜率、败率等权值，但是找不到一个合适的算法解析它们……不管了先来看吧(>^ω^<)_

Millie: _在那之前我们需要准备两个类，一个是路径Way，一个是节点Node。_
```java
class Node{
    public Node(){this.p= new Position(0,0);}
    public Node(Position pos,int i){this.p=pos;this.inspire=i;}
    public Position p;//节点位置
    public int inspire;//启发值，应该是这么叫的？
}
class Way {
    public Way() {
        this.step = new Node();
    }
    public Way(Node s){
        this.step = new Node(new Position(s.p.x, s.p.y), s.inspire);
    }
    public int stats;// ==0:draw , >0:win , <0:lose
    public Node step;//原来是ArrayList<Node>，但迷茫了。
    @Override
    public Way clone(){
        return new Way(this.step);
    }
}
```
Millie: _好的！结束！我写不懂了……接下来就是战争迷雾模式_
```java
class Player_PC extends Player{//同样是继承于Player之下，你怎么就这么鬼畜呢？？
    public Player_PC (ChessType _type);//构造函数，功能大致和Human相同
    public void onTurn(Board b);//有必胜走必胜，有必输走必输，两个都没有就刷nextStop()。
    private static ChessType getOppoType(ChessType _type);//小工具，获得对手方。（圈变叉、叉变圈）
    private ArrayList<Way> ways;//保存那几个路径
    private void nextStop(Board b,Way way,int depth,ChessType inType);//一个回溯递归算法，确实能测量步数启发值和胜率，但是结果还是很诡异。
}
```
Millie: _仔细看看那个nextStop()：_
```java
private void nextStop(Board b, Way way, int depth, ChessType inType){
    boolean newWay = false;//是否需要创建一条新路
    boolean full = true;//全下满了？
    for(int x = 0; x<3;x++){//遍历
        for(int y = 0; y<3;y++){//又遍历
            if(b.getBoard(new Position(x,y))==ChessType.BLANK){//如果可以落子
                full = false;//可以落子说明还没满啦~~
                Way _way;//新指针，用于各种传递
                if ( newWay ) {//第二次循环就会是true
                    _way = way.clone();//克隆一条路
                    this.ways.add(_way);//并把它添加到路的集合
                } else { 
                    newWay = true;//第一次不是新路，第二次就是了
                    _way = way; //第一次，很朴素，一条大路走到底
                }
                if(depth==0)//如果是这条路的第一步
                     _way.step=new Node(new Position(x, y), depth);//那你就是这回合接下来要走的那一步
                b.setBoardF(new Position(x, y), inType);//走一波，落子！！
                ChessType wms=b.judge();//看看谁赢了
                if(!b.isFull())//如果还没满
                    this.nextStop(b.clone(), _way, depth+(wms==ChessType.BLANK?1:0), getOppoType(inType));//接着走（为了概率相同）
                else if(wms != ChessType.BLANK)//满了好说，看看这样走谁会赢
                    _way.stats=wms==this.type?(10-depth):(depth-10);//谁赢谁加分
                b.setBoardF(new Position(x,y),ChessType.BLANK);//恢复棋盘，接着循环~~
            }//♪(^∇^*)
        }//\(^o^)/~
    }
}
```
Millie: _问题是有，但是不知道在哪里quq，难受_

Millie: _再您妈的见。_