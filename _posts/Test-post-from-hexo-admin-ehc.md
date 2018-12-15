title: 节奏游戏开发时的万般阻难
author: Kitekii
tags: []
categories: []

date: 2018-10-16 14:29:00
---
施工中……

## 音符同步

[senator](http://gad.qq.com/article/detail/34880)

你需要`[RequireComponent(typeof(AudioSource))]`来规定该元件可被作为音游元件。

1. 使用`AudioSettings.dspTime` 跟踪歌曲的位置，而不是使用	`Time.timeSinceLevelLoad`。

2. 使用歌曲的**位置**来更新移动。

3. 不要通过每帧的时差来更新音符。

#### 位置跟踪

在所有节奏游戏中，我们必须跟踪歌曲位置，以确定是否应该生成音符。如下所示：

```c#
float songPosition;
// 节拍当前位置
float songPosInBeats;
// 节拍的持续时间
float secPerBeat;
float dsptimesong;
// We initialize these fields in the Start() function:
void Start() {
    secPerBeat = 60f / bpm;
    dsptimesong = (float) AudioSettings.dspTime;
    // 开始播放音乐
    GetComponent<AudioSource>().Play();
}
```

我们转换`bpm`到`secPerBeat`后面方便使用。`secPerBeat`将用于计算节拍中的歌曲位置，这对音符生成非常重要。

我们用`dsptimesong`记录歌曲的开始时间。使用`AudioSettings.dspTime`跟踪歌曲的位置，而不是使用`Time.timeSinceLevelLoad`。因为`Time.timeSinceLevelLoad`只在每个帧中更新，而`AudioSettings.dspTime`更新频率更高，因为它是音频系统的定时器。为了保持歌曲的速度，我们必须使用音频系统的定时器来避免帧更新和音频更新之间的时间差引起的延迟。

在`Update()`函数中，我们通过`AudioSettings.dspTime`计算歌曲的位置：

```c#
void Update()
{
    songPosition = (float) (AudioSettings.dspTime - dsptimesong);
    songPosInBeats = songPosition / secPerBeat;
}
```

#### 歌曲信息

移动到我们记录歌曲信息的字段：

```c#
float bpm;
float [] notes;
int nextIndex = 0;
```

为了简单起见，我演示的歌曲只有一个轨迹的音符。

`bpm`是一首歌的每分钟的节拍数。正如我们所看到的，`secPerBeat`为了方便起见被转换了。

`notes`是一个数组，保持歌曲中音符的位置。例如，`notes`将初始化为`{1f，2f， 2.5f， 3f，3.5f， 4.5f}`

#### 产生音符

我们确定是否应该在`Update()`函数中产生一个音符前。但我们应该首先确定显示节拍的数量。

例如曲谱中，当前的歌曲position-in-beats为1，但是节拍3已经产生，意味着提前显示3个节拍。

```c#
songPosInBeats = songPosition / secPerBeat;, add the following lines:
if (nextIndex < notes.Length && notes[nextIndex] < songPosInBeats beatsShownInAdvance)
{
    Instantiate( /* Music Note Prefab */ );
    nextIndex ;
}
```

我们首先检查歌曲（`nextIndex < notes.Length`）中是否已经没有音符，如果还有音符，那么我们再看看歌曲是否到达要播放下一个音符的节奏（`notes[nextIndex] < songPosInBeats + beatsShownInAdvance`）。如果满足条件，产生音符，`nextIndex`自增，以便`nextIndex`保持下一个音符的产生。

#### 移动音符

最后，如何按照这首歌来移动我们产生的音符。不要通过帧的时差来移动它们。

始终按照歌曲的位置更新移动，因为

1. 音频定时器与帧定时器有时差

2. 节拍可能在两帧中间

那么，怎么移动音符呢？插入法！为了简单起见，我将把所有的代码放在`MusicNote`类上，并且只在`Update()`移动我们的音符：

```c#
void Update() {
    transform.position = Vector2.Lerp(
        SpawnPos,
        RemovePos,
        (BeatsShownInAdvance - (beatOfThisNote - songPosInBeats) ) / BeatsShownInAdvance
    );    
}
```



## 判定提前

姑且定义以下几种判定时间点:
- CHECK_PERFECT
- CHECK_GREAT
- CHECK_MISS

每个音符到达__dspTime - CHECK_MISS__时触发miss判定
南梦宫所提出的判定方案是


## 视觉反馈

## 额外音效

processing