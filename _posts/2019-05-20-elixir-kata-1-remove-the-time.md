---
layout: post
title: "Elixir Kata 1: Remove the time"
date: 2019-05-21 00:00:00 +0700
read_time: true
author: pnnutkung
categories: [programming]
tags: [elixir, programming, codewars]
---

บทความนี้เกิดเนื่องจากผมสนใจในภาษา Elixir ซึ่งเป็น Functional Programming Language  
และผมยังเป็นมือใหม่อยู่เลยจะลองนั่งเล่น kata เพื่อฝึกใช้ library พื้นฐานของภาษาให้คล่อง  
อย่างที่เขาว่ากันว่าถ้าฐานเราไม่ดีสิ่งที่ต่อขึ้นไปข้างบนก็พังทลายลงมาได้ง่าย ลองมาเล่นข้อแรกแบบง่าย ๆ กันดูดีกว่า

## Let's roll

โจทย์ [Remove the time](https://www.codewars.com/kata/56b0ff16d4aa33e5bb00008e) ระดับ 8 Kyu

```text
You're re-designing a blog and the blog's posts have the following format for showing the date and time a post was made:

Weekday Month Day, time e.g., Friday May 2, 7pm

You're running out of screen real estate, and on some pages you want to display a shorter format, Weekday Month Day that omits the time.

Write a function, shortenToDate, that takes the Website date/time in its original string format, and returns the shortened format.

Assume shortenToDate's input will always be a string, e.g. "Friday May 2, 7pm". Assume shortenToDate's output will be the shortened string, e.g., "Friday May 2".
```

จากที่ลองอ่านโจทย์ดูแล้วโคตร EZ เลยว่าไหม `แค่เอาเวลาที่อยู่หลัง comma กับ , ออกไป`  
ลองนั่งคิดดูแล้วเราก็หา index ของ `comma` สิแล้วก็ `return string index ตั้งแต่ 0 ไปถึงก่อน comma`  
ว่าแล้วก็เขียน code

```elixir
defmodule Datemizer do
  def shorten_to_date(datetime) do
    { comma_index, _ } = :binary.match datetime, "," # ใช้ pattern matching เอา index ของ comma
    String.slice(datetime, 0..comma_index - 1) # return string index 0 จนไปถึงก่อน comma
  end
end
```

หน้าตาก็จะออกมาประมาณนี้  
**แต่** วิธีนี้มันไม่ Functional เลย  
ลองเปลี่ยนเป็น**วิธีที่มัน Functional**ดูหน่อย

ขั้นตอน

1. ใช้ [split/3](https://hexdocs.pm/elixir/String.html#split/3) แยกระหว่าง Date กับ time ออกจากกัน
2. คืนค่าเฉพาะ index ที่ 0 โดยใช้คำสั่ง [hd/1](https://hexdocs.pm/elixir/Kernel.html?#hd/1)

```elixir
defmodule Datemizer do
  def shorten_to_date(datetime) do
    datetime
      |> String.split(",") # ใช้ split แยก string ออกมาเป็น list หน้าตาแบบนี้ ["Friday May 2", " 7 pm"]
      |> hd # hd มาจาก head ของ list หรือก็คือ index ที่ 0 นั่นเอง
  end
end
```

## Summary

จากที่เคยเขียน Imperative Programming Language มาตลอดเราก็จะเขียนแบบลุย ๆ ซึ่งมันทำงานได้เหมือนกันแหละ แต่การที่ได้ลองมาเขียน Functional Programming ดูทำให้ค้นพบท่าอะไรใหม่ ๆ อย่างใช้ base module ที่เขามีมาให้อยู่แล้วมาแก้ปัญหา code ก็ดูอ่านง่ายขึ้นและ optimize มากกว่าด้วย(อย่างถ้าเราใช้พวก sort เขาคงเขียน function นั้นมาแบบเร็วที่สุดแล้วโดยที่เราไม่ต้องมาเขียนเอง เผลอ ๆ เขียนเองแล้วช้ากว่า)

มาลองเปิดโลกใหม่กับ Functional Programming กันเถอะ!
