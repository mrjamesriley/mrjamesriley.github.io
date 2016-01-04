---
layout: post
title: "Ruby Koans Scoring Project"
date: 2012-05-01 17:05
categories: blog
---

[Ruby Koans](http://rubykoans.com) has sat on my todo list for almost as long as I've been a Ruby developer and this week I've finally taken it upon myself to tackle it. Expectedly, there's little new to be expected for anyone who has served their time as a developer, but there's plenty of helpful tricks and Ruby intricacies to be reminded of. I've enjoyed looking at how others have [tackled the problem](http://stackoverflow.com/questions/4749973/ruby-greed-koan-how-can-i-improve-my-if-then-soup), so assuming you've already given the question a good attempt, here's my solution:

{% highlight ruby %}
def score(dice)
  score = 0

  1.upto(6).each do |num|
    amount = dice.count(num)
    if amount >= 3
      score += num == 1 ? 1000 : num * 100
      amount -= 3
    end

    score += 100 * amount if num == 1
    score += 50 * amount if num == 5
  end

  score
end
{% endhighlight %}
