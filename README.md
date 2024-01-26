# Wave
A SwiftUI View that implements a comfortable fluid-like animated wave with animated progress change.

一个用SwiftUI写的波浪动画，进度改变的时候也有美好的动效。

---

# Overview 概览

May you create marvelous waves with `Wave`!

使用`Wave`，你可以实现令人愉悦的波浪效果。


| Single Wave | Multiple Wave |
|---|---|
| ![single wave](https://github.com/richardjorne/Wave/blob/main/SingleWave.gif?raw=true) | ![double wave](https://github.com/richardjorne/Wave/blob/main/DoubleWave.gif?raw=true)

Let's see how it works. [English Document](#english)

话不多说，来看看怎么用吧。

# Installation 安装方法

直接复制`/Wave.swift`到项目即可。

Swift Package Manager 根本没法用，Xcode本身也有一些限制，因此放弃。

CocoaPods等我搞懂了就做。

# Usage 使用方法

```swift
Wave(progress: <#T##Binding<Float>#>,
     waveHeight: <#T##CGFloat#>,
     wavePhaseDegree: <#T##CGFloat#>,
     waveDuration: <#T##CGFloat#>,
     waveColor: <#T##Color#>,
     progressAnimation: <#T##Animation?#>)
```

其中必须提供的为`progress`。

将其绑定到你的Float变量即可。

## Parameter Explanation 参数解释

`waveHeight`: `CGFloat`，波浪的高度，简单来说就是正弦曲线的振幅A的两倍。默认值就挺好的了。

`wavePhaseDegree`: `CGFloat`，波浪的初始相位，角度制。这对于单一Wave来说用处不大，因为波浪始终都是可以无限运动的，因此看不出区别。稍后会详细提到。

`waveDuration`: `CGFloat`，波浪运动的快慢。默认值就挺好的了。

`waveColor`: `Color`，波浪的颜色。

`progressAnimation`: `Animation?`，波浪的progress在改变时的动画。不能接受这么大个东西在动的时候还没有动画。但你执意不要动画的话就设置为nil吧。



# A Better Wave 更舒适的波浪

如果你和我一样对效果有追求的话，你可能觉得一个Wave不够真实。受到[noa4021J](https://github.com/noa4021J/WaveAnimationView)的启发，创建多个Wave叠加可以让Wave看起来更加真实。

```swift
struct TestView: View {
    
    @State var progress: Float = 0.6
    
    var back = #colorLiteral(red: 0.4666666687, green: 0.7647058964, blue: 0.2666666806, alpha: 1)
    var front = #colorLiteral(red: 0.5843137503, green: 0.8235294223, blue: 0.4196078479, alpha: 1)
    
    var body: some View {
        ZStack{
            Wave(progress: $progress, waveHeight: 13, wavePhaseDegree: 0, waveDuration: 2.5, waveColor: Color(back).opacity(0.2))
                .edgesIgnoringSafeArea(.all)
            Wave(progress: $progress, waveHeight: 15, wavePhaseDegree: 180, waveDuration: 2.5, waveColor: Color(front).opacity(0.4))
                .edgesIgnoringSafeArea(.all)
        }
    }
}
```

（别问我为什么这么喜欢colorLiteral哈哈哈

你会发现我用到了`wavePhaseDegree`。由于SwiftUI的特性，你没法在`viewDidLoad()`之后过一会再放上一个初相位相同的Wave来达到实际相位不同的效果（不是真的有人这么做吗啊喂），因此你可以直接设置他们的初相位。与[noa4021J](https://github.com/noa4021J/WaveAnimationView)不同的是，由于我把Wave独立开了，因此实际用起来会更加自由。

你也可以加更多的Wave，至于初相位具体该怎么设置也要由你来决定。

![double wave](https://github.com/richardjorne/Wave/blob/main/DoubleWave.gif?raw=true)

这是叠加了两个Wave后的样子，非常漂亮。



# Animating Progress Change 进度变化动画

`Wave`的特别之处在于你可以让进度变化变得更好看。

![animating progress change](https://github.com/richardjorne/Wave/blob/main/DoubleWaveProgressChangeSmooth.gif?raw=true)

# About This Project 寻根溯源

这是我做的第二个SwiftUI开源项目。

作为[一杯](https://apps.apple.com/cn/app/一杯/id1595167189)的开发者，我发现自己越来越喜欢SwiftUI而不是UIKit了。

一杯最初是完全由UIKit写成的，当时的我也是完完全全的Swift新手。这几年我一直都在实践中不断地学习，不断发现更优雅的代码的写法。写一杯的时候逐渐熟悉了UIKit（但也没有那么熟悉），到后来写一杯的Widget和[阁楼](https://apps.apple.com/cn/app/阁楼-效率提升好帮手/id6447677830)， 我才接触到了SwiftUI。慢慢地，越写越上手，越写越感觉比UIKit方便很多。

当初选择UIKit的时候我还好像什么都不会， 所有地方都好像只能用别人的库。也是因为这些年积累下来库比较多我才选择的UIKit。可是后来我甚至有点感觉UIKit的程序有点写不下去了的时候，我感觉是时候改变一下了。

我发现自己好像有能力能开始写点东西。虽然不多，也不是很复杂， 但能让SwiftUI的社区变得更完善，我感觉是一件挺棒的事。

在用其他人的库的时候，我发现一些库用起来其实没有那么自由。因此当我在写自己的库的时候，我希望能尽量让它保持更大的自由度，同时又保证用起来尽量简单，比如由你来选择是使用单个Wave还是多个组合起来。希望我的库对你能有所帮助：） 



最后讲讲关于这个项目的小花絮

其实很早就想读懂[noa4021J](https://github.com/noa4021J/WaveAnimationView)的代码并进行改进， 但无奈一直都看不太懂。。。

后来就按照自己的想法写了一下这个代码， 很神奇的一点是我居然只用了不到100行？（虽然还是折腾了好几天）

然后关于动画，Wave涉及两个动画：一个是波浪本身要不断运动，第二个是进度变化的时候波浪的位置高度也要变化。 我很努力地花了很久，试图让进度变化的动画发生的时候波浪依旧运动， 可是没有找到同时进行两个动画的解决办法，网上也搜不到，无奈只好继续用Timer（）

但反正最后的结果就是我可以用SwiftUI原生的动画，而不是Timer来为进度变化添加动画。

有人反馈一杯的波浪在进度大幅度改变的时候会有掉帧的感觉...这个我当时很努力地尝试过，可是没有任何改进。我觉得在变为SwiftUI的原生动画之后应该可以解决这个问题，而且还可以更好地适配ProMotion。



第一个项目请参阅：[RingRangeSelector](https://github.com/richardjorne/RingRangeSelector)



祝你度过愉快的一天！

此致

Richard Jorne

---

<span id="english"></span>

# English Document

# Installation

Just copy `/Wave.swift` to your project.

Due to some Xcode limitations, I couldn't use SPM, so no SPM.

CocoaPods support is excepted in future.

# Usage

```swift
Wave(progress: <#T##Binding<Float>#>,
     waveHeight: <#T##CGFloat#>,
     wavePhaseDegree: <#T##CGFloat#>,
     waveDuration: <#T##CGFloat#>,
     waveColor: <#T##Color#>,
     progressAnimation: <#T##Animation?#>)
```

The only required property is a `Binding Float` `progress`.

Just bind it to your `Float` variable!

## Parameter Explanation

`waveHeight`: `CGFloat`, the height of the wave, it's simply twice the amplitude of a sine wave. The default value is pretty good.

`wavePhaseDegree`: `CGFloat`, the initial phase of the wave, in degree. This is not useful for just one wave, because the wave will move forever so you can't tell the difference. But there is a difference and we'll mention it later.

`waveDuration`: `CGFloat`, use this to control the speed of the wave. The default value is pretty good (TDPG).

`waveColor`: `Color`, the wave's color.

`progressAnimation`: `Animation?`, the animation when the progress changes. Can't stand if there's no animation when such a big stuff changes. Set to `nil` if you don't want animation, but, strongly discouraged.



# A Better Wave

If you pursue perfection like I do, you may think that one wave may not be real enough. Inspired by [noa4021J](https://github.com/noa4021J/WaveAnimationView), stacking multiple waves may make it look more genuine.

```swift
struct TestView: View {
    
    @State var progress: Float = 0.6
    
    var back = #colorLiteral(red: 0.4666666687, green: 0.7647058964, blue: 0.2666666806, alpha: 1)
    var front = #colorLiteral(red: 0.5843137503, green: 0.8235294223, blue: 0.4196078479, alpha: 1)
    
    var body: some View {
        ZStack{
            Wave(progress: $progress, waveHeight: 13, wavePhaseDegree: 0, waveDuration: 2.5, waveColor: Color(back).opacity(0.2))
                .edgesIgnoringSafeArea(.all)
            Wave(progress: $progress, waveHeight: 15, wavePhaseDegree: 180, waveDuration: 2.5, waveColor: Color(front).opacity(0.4))
                .edgesIgnoringSafeArea(.all)
        }
    }
}
```

(I'm a huge fan of colorLiteral lol

You'll find that I used `wavePhaseDegree`. This parameter allows you to create multiple waves with different initial phases so that you can make everything look better.

But about the value...Well...It's up to you :)

![double wave](https://github.com/richardjorne/Wave/blob/main/DoubleWave.gif?raw=true)

This is what it looks like when I stack two `Wave`s. Stunning.

# Animating Progress Change

`Wave` allows you to add beautiful non-linear animations to your progress change.

![animating progress change](https://github.com/richardjorne/Wave/blob/main/DoubleWaveProgressChangeSmooth.gif?raw=true)




# About This Project

This is my second open-source SwiftUI project.

As the developer of [A Cup](https://apps.apple.com/cn/app/一杯/id1595167189), I find myself loving SwiftUI more instead of UIKit.

A Cup was originally completely written in UIKit, when I was pretty new to Swift. I've been learning through practice for the past few years, and kept finding ways to write better and more elegant codes. I knew UIKit more through writing A Cup and started to get familiar with SwiftUI when writing widgets for A Cup and [Attic](https://apps.apple.com/cn/app/阁楼-效率提升好帮手/id6447677830). Little by little, I started to feel the convenience of SwiftUI.

When I chose to write A Cup with UIKit, I felt like knowing nothing, and I could only use libraries from others. Actually, one of the reasons I chose UIKit was because the amount of libraries was much greater than SwiftUI. But then I felt like it was tedious writing UIKit codes, and I couldn't stand more.

It's time to change.

I felt like I could finally start to write something, and make some contributions to the SwiftUI community, although it's not very sophisticated. I feel great.

When using other libraries, I found that some were not quite flexible. So when creating my own libraries, I hope to keep the flexibility while ensuring the usage to be simple. For example, you can choose to use a single wave or combine multiple waves together.

I hope my library can make a day for you :)



Another little story about this project:

A long time ago I tried to understand [noa4021J](https://github.com/noa4021J/WaveAnimationView)'s code and make some improvements, but it was a little hard for me to understand...

Then I wrote this code with my own thoughts, and, interestingly, it takes less than 100 lines(although it still took me pretty much time)

About the animation, there are two animations for the wave: one to keep the wave moving and the other to animate the progress change of the wave. It took me a lot of time to keep the wave moving while animating the progress change, but I couldn't find a solution on the Internet. The animation of the wave moving just gets interrupted. So finally I could only use a `scheduledTimer`.

But anyway, we can use SwiftUI's animation to animate the progress change now, instead of `Timer`, which is what I used on A Cup.

Some of my users told me that there was a frame drop when the progress changed rapidly. I did put a lot of effort into the improvement but nothing worked. I think this time, changing everything to SwiftUI's animation can solve this problem, and can work better with ProMotion.



My first project: [RingRangeSelector](https://github.com/richardjorne/RingRangeSelector)



Wish you a nice day!

Best,

Richard Jorne